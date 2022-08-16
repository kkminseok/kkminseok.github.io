---
title: TestPostGresContainer Python 컨테이너 이름 바꾸기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-08-16 01:00:00 +0800
categories: [Python,FastAPI]
tags: [FastAPI]
math: true
mermaid: true
image: 
  path: /assets/img/zinsap/containerName.png
comments : true
---

사이드 프로젝트중..

진귀한 장면을 발견했다.

![](/assets/img/zinsap/containerName.png)

컨테이너 이름이 무작위로 막 생성되어있었다.

사실 몇개는 이미 컨테이너가 띄워진 상태로 돌아가고 있어서 다 꺼버렸다.

원인을 찾던중..

도커로 Postgres를 띄우는 코드는 사이드프로젝트 통합테스트 코드밖에 없다고 생각하여 코드를 뜯어봤다.

오픈소스로 제공하는 `testcontainers.postgres`를 Python언어로 사용하고 있었는데, 분명 이 속에서 Docker Container의 이름을 지정하는 곳이 있을거라 생각했다.

```python
class PostgresTestContainer(PostgresContainer):
    def get_connection_url(self):
        # windows docker connection
        return super().get_connection_url().replace('localnpipe', 'localhost')


@pytest.fixture(scope="session", autouse=True)
def db_container(session_mocker):
    with PostgresTestContainer(image="postgres:9.5", port=5432) as postgres:
        ...
```

`PostgresTestContainer`클래스는 `PostgresContainer`를 상속받고 있길래 `PostgresContainer`요 코드로 들어가봤다.

```python
#
#    Licensed under the Apache License, Version 2.0 (the "License"); you may
#    not use this file except in compliance with the License. You may obtain
#    a copy of the License at
#
#         http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
#    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
#    License for the specific language governing permissions and limitations
#    under the License.
import os

from testcontainers.core.generic import DbContainer


class PostgresContainer(DbContainer):
    """
    Postgres database container.

    Example
    -------
    The example spins up a Postgres database and connects to it using the :code:`psycopg` driver.
    ::

        with PostgresContainer("postgres:9.5") as postgres:
            e = sqlalchemy.create_engine(postgres.get_connection_url())
            result = e.execute("select version()")
    """
    POSTGRES_USER = os.environ.get("POSTGRES_USER", "test")
    POSTGRES_PASSWORD = os.environ.get("POSTGRES_PASSWORD", "test")
    POSTGRES_DB = os.environ.get("POSTGRES_DB", "test")

    def __init__(self,
                 image="postgres:latest",
                 port=5432, user=None,
                 password=None,
                 dbname=None,
                 driver="psycopg2",
                 **kwargs):
        super(PostgresContainer, self).__init__(image=image, **kwargs)
        self.POSTGRES_USER = user or self.POSTGRES_USER
        self.POSTGRES_PASSWORD = password or self.POSTGRES_PASSWORD
        self.POSTGRES_DB = dbname or self.POSTGRES_DB
        self.port_to_expose = port
        self.driver = driver

        self.with_exposed_ports(self.port_to_expose)

    def _configure(self):
        self.with_env("POSTGRES_USER", self.POSTGRES_USER)
        self.with_env("POSTGRES_PASSWORD", self.POSTGRES_PASSWORD)
        self.with_env("POSTGRES_DB", self.POSTGRES_DB)

    def get_connection_url(self, host=None):
        return super()._create_connection_url(dialect="postgresql+{}".format(self.driver),
                                              username=self.POSTGRES_USER,
                                              password=self.POSTGRES_PASSWORD,
                                              db_name=self.POSTGRES_DB,
                                              host=host,
                                              port=self.port_to_expose)

```

이 클래스는 `DbContainer`를 상속받고 있고, 컨테이너 이름을 설정하지는 않는것 같다. 부모 클래스로 들어가봐야겠다.

```python
#
#    Licensed under the Apache License, Version 2.0 (the "License"); you may
#    not use this file except in compliance with the License. You may obtain
#    a copy of the License at
#
#         http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
#    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
#    License for the specific language governing permissions and limitations
#    under the License.

from testcontainers.core.container import DockerContainer
from testcontainers.core.waiting_utils import wait_container_is_ready
from deprecation import deprecated
ADDITIONAL_TRANSIENT_ERRORS = []
try:
    from sqlalchemy.exc import DBAPIError
    ADDITIONAL_TRANSIENT_ERRORS.append(DBAPIError)
except ImportError:
    pass


class DbContainer(DockerContainer):
    def __init__(self, image, **kwargs):
        super(DbContainer, self).__init__(image, **kwargs)

    @wait_container_is_ready(*ADDITIONAL_TRANSIENT_ERRORS)
    def _connect(self):
        import sqlalchemy
        engine = sqlalchemy.create_engine(self.get_connection_url())
        engine.connect()

    def get_connection_url(self):
        raise NotImplementedError

    def _create_connection_url(self, dialect, username, password,
                               host=None, port=None, db_name=None):
        if self._container is None:
            raise RuntimeError("container has not been started")
        if not host:
            host = self.get_container_host_ip()
        port = self.get_exposed_port(port)
        url = "{dialect}://{username}:{password}@{host}:{port}".format(
            dialect=dialect, username=username, password=password, host=host, port=port
        )
        if db_name:
            url += '/' + db_name
        return url

    def start(self):
        self._configure()
        super().start()
        self._connect()
        return self

    def _configure(self):
        raise NotImplementedError


class GenericContainer(DockerContainer):
    @deprecated(details="Use `DockerContainer`.")
    def __init__(self, image):
        super(GenericContainer, self).__init__(image)

```

이 클래스도 컨테이너의 이름을 설정하지는 않는다. `DockerContainer`를 상속받으므로 저 코드로 또 들어가봐야겠다.

```python
from deprecation import deprecated
from docker.models.containers import Container

from testcontainers.core.docker_client import DockerClient
from testcontainers.core.exceptions import ContainerStartException
from testcontainers.core.utils import setup_logger, inside_container, is_arm

logger = setup_logger(__name__)


class DockerContainer(object):
    def __init__(self, image, docker_client_kw: dict = None, **kwargs):
        self.env = {}
        self.ports = {}
        self.volumes = {}
        self.image = image
        self._docker = DockerClient(**(docker_client_kw or {}))
        self._container = None
        self._command = None
        self._name = None
        self._kwargs = kwargs

    def with_env(self, key: str, value: str) -> 'DockerContainer':
        self.env[key] = value
        return self

    def with_bind_ports(self, container: int,
                        host: int = None) -> 'DockerContainer':
        self.ports[container] = host
        return self

    def with_exposed_ports(self, *ports) -> 'DockerContainer':
        for port in list(ports):
            self.ports[port] = None
        return self

    @deprecated(details='Use `with_kwargs`.')
    def with_kargs(self, **kargs) -> 'DockerContainer':
        return self.with_kwargs(**kargs)

    def with_kwargs(self, **kwargs) -> 'DockerContainer':
        self._kwargs = kwargs
        return self

    def maybe_emulate_amd64(self) -> 'DockerContainer':
        if is_arm():
            return self.with_kwargs(platform='linux/amd64')
        return self

    def start(self):
        logger.info("Pulling image %s", self.image)
        docker_client = self.get_docker_client()
        self._container = docker_client.run(self.image,
                                            command=self._command,
                                            detach=True,
                                            environment=self.env,
                                            ports=self.ports,
                                            name=self._name,
                                            volumes=self.volumes,
                                            **self._kwargs
                                            )
        logger.info("Container started: %s", self._container.short_id)
        return self

    def stop(self, force=True, delete_volume=True):
        self.get_wrapped_container().remove(force=force, v=delete_volume)

    def __enter__(self):
        return self.start()

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.stop()

    def __del__(self):
        """
        Try to remove the container in all circumstances
        """
        if self._container is not None:
            try:
                self.stop()
            except:  # noqa: E722
                pass

    def get_container_host_ip(self) -> str:
        # infer from docker host
        host = self.get_docker_client().host()
        if not host:
            return "localhost"

        # check testcontainers itself runs inside docker container
        if inside_container():
            # If newly spawned container's gateway IP address from the docker
            # "bridge" network is equal to detected host address, we should use
            # container IP address, otherwise fall back to detected host
            # address. Even it's inside container, we need to double check,
            # because docker host might be set to docker:dind, usually in CI/CD environment
            gateway_ip = self.get_docker_client().gateway_ip(self._container.id)

            if gateway_ip == host:
                return self.get_docker_client().bridge_ip(self._container.id)
            return gateway_ip
        return host

    def get_exposed_port(self, port) -> str:
        mapped_port = self.get_docker_client().port(self._container.id, port)
        if inside_container():
            gateway_ip = self.get_docker_client().gateway_ip(self._container.id)
            host = self.get_docker_client().host()

            if gateway_ip == host:
                return port
        return mapped_port

    def with_command(self, command: str) -> 'DockerContainer':
        self._command = command
        return self

    def with_name(self, name: str) -> 'DockerContainer':
        self._name = name
        return self

    def with_volume_mapping(self, host: str, container: str,
                            mode: str = 'ro') -> 'DockerContainer':
        # '/home/user1/': {'bind': '/mnt/vol2', 'mode': 'rw'}
        mapping = {'bind': container, 'mode': mode}
        self.volumes[host] = mapping
        return self

    def get_wrapped_container(self) -> Container:
        return self._container

    def get_docker_client(self) -> DockerClient:
        return self._docker

    def get_logs(self):
        if not self._container:
            raise ContainerStartException("Container should be started before")
        return self._container.logs(stderr=False), self._container.logs(stdout=False)

    def exec(self, command):
        if not self._container:
            raise ContainerStartException("Container should be started before")
        return self.get_wrapped_container().exec_run(command)

```

여기서 보아하니 이름을 설정하는 곳을 찾은것 같다 `__init__`에서 초기화 작업을 해주고 있다.

확장성을 고려하여 `None`도 받아야한다. `None`을 받으면 컨테이너 이름을 랜덤으로 만들어서 띄우는데 이게 쌓이다보니 맨 위의 스크린샷처럼 되는것.

그래서

```python
class DockerContainer(object):
  def __init__(self, image, name = None, docker_client_kw: dict = None, **kwargs):
      ...
      self._name = name
    
```

처럼 `name=None`을 추가하고 `self._name = name`구문으로 초기화해주었다.

그리고 다시 통합테스트 코드로 가서

```python
class PostgresTestContainer(PostgresContainer):
    def get_connection_url(self):
        # windows docker connection
        return super().get_connection_url().replace('localnpipe', 'localhost')


@pytest.fixture(scope="session", autouse=True)
def db_container(session_mocker):
    with PostgresTestContainer(image="postgres:9.5", port=5432, name="ZinsaContainer") as postgres:
        ...
```

`name`을 추가하여 통합테스트를 실행해보았다.

![](/assets/img/zinsap/containerName2.png)

성공했다.

혹시몰라 인자에서 `name`을 빼보고 실행해보았다.

![](/assets/img/zinsap/containerName3.png)

랜덤한 이름으로 컨테이너를 잘 띄운다.

-----

## 후기

이걸 OCP라고하면 OCP일까? 기능을 확장시켰고 기존 요건인 None도 받을 수 있게 하였으니..

다만 걱정스러운건 이 소스를 제공한 사람들이 일부러 `name`을 인자로 받지 못하게 설계한것이 아닐까? 
충돌의 위험이 있을까?

잘 모르겠다. 컨테이너가 계속 쌓이는것이 불편해서 하나의 이름을 가진 컨테이너로 띄웠다 내렸다 하고 싶은 마음에 시도해보았다.

앞으로 부딪히다보면 이 방법의 단점을 알 수도 있을 것 같다.





