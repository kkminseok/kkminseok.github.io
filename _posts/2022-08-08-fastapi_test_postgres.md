---
title: Python Docker + Postgres 테스트
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-08-08 00:00:00 +0800
categories: [Python,FastAPI]
tags: [FastAPI]
math: true
mermaid: true
image: 
  path: /assets/img/zinsap/integration_success.png
comments : true
---

사이드 프로젝트에서 잘 진행되던 통합테스트가 펑펑 터져버렸다.

이유인 즉슨, Docker로 띄운 PostgresCotainer가 여러 테스트함수에서 공유하다보니, 

데이터중복도 발생하고 정합성 등 문제가 발생했다.

예를들어 test1()에서 `User`객체를 생성하면 이게 사라지지 않고 test2()에서도 존재하여 영향을 미친다는 것이다.

일단 기존 Docker로 띄운 Postgres코드이다.

```python
@pytest.fixture(scope="session", autouse=True)
def db_container(session_mocker):
    with PostgresTestContainer(image="postgres:9.5", port=5432) as postgres:
        url = postgres.get_connection_url().replace("psycopg2", "asyncpg")
        mock_os = os.environ | {
            "SQLALCHEMY_DATABASE_URL": url,
            "START_UP_DB_INIT": "True",
            "SECRET_KEY": "yaguen",
            "ALGORITHM": "HS256",
            "ACCESS_TOKEN_EXPIRE_MINUTES": "10",
            "MAIL_PASSWORD": os.environ.get("MAIL_PASSWORD")
        }
        session_mocker.patch.dict(os.environ, mock_os, clear=True)
        yield postgres

@pytest.fixture(scope="session")
def client() -> TestClient:
    with TestClient(app) as c:
        yield c
```

`scope`범위를 `session`으로 지정하여 테스트가 끝날때까지 yield 밑으로 가질 않는다. 즉, 한 번 띄우면 모든 테스트가 끝날때까지 살아있다는 것.

처음에는 `scope`범위를 `function`으로 줄였다.

그러면 도커로 띄웠다가 내렸다가를 반복해서 테스트 시간이 엄청 오래걸렸다.

'통합테스트는 원래 오래 걸리는거 아냐?' 하면서 걍 시도해봤는데..

그 밑에 있는

```python
@pytest.fixture(scope="session")
def client() -> TestClient:
    with TestClient(app) as c:
        yield c
```

에서 문제가 발생했다. 

![](/assets/img/zinsap/integration_client.png)

Os에러가 발생하였는데, 이를 어떻게 할까 하다가 fixture를 빼보기도 하고 그런 삽질을 하고 있었다.

그러던 와중 `PostgresTestContainer`함수를 작성한 친구가 도커 띄우는데 오래걸리니까 일부러 `db_container`의 스코프를 `session`으로 해둔거라고 했다.

그래서 결국 `session`으로 두고 데이터 정합성을 해결할 방법을 찾아봤는데..

`PostgresTestContainer`에는 `sql`문을 실행할 수 있는 메서드가 있다고 친구가 그랬다.

하지만, 결국 테이블이 많아지면 많아질수록 `sql`문을 매 번 수정해주는건 번거로워서 테스트 특성상 데이터를 다 날리는게 낫다고 생각했다.

그래서 가장 먼저 든 생각은 `truncate`를 이용해서 테이블 데이터를 다 날릴 생각이었지만..

연관관계가 또 깊어지면 `truncate`로 해결할 수 없을거라 생각했다.

그래서 아예 초기상태로 만들어야하는 `fixture`가 필요했다.

왜냐하면 함수가 실행될 때마다 테이블을 다 초기화시켜야하므로 `fixture`가 필요했고 `scope`범위는 `function`으로 두어야겠다고 생각했다.

그래서 만든 함수는..

```python

@pytest.fixture()
async def delete_container(db_container) -> None:
    from app.db.session import Base
    engine = create_async_engine(db_container.get_connection_url().replace("psycopg2", "asyncpg"))
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)
        await conn.run_sync(Base.metadata.create_all)

```

현재 잘돌아가긴하는데.. 더 좋은 방법이 없으려나 모르겠다.

`fixture`에 있는 `db_container`를 받고, 그 url을 기준으로 테이블을 삭제하고 만들고 하는 코드다.

문제점은 `db_container`내부에 있는 `url = postgres.get_connection_url().replace("psycopg2", "asyncpg")`의 구문이 바뀌면 바뀐 url을 똑같이 매핑해줘야하지만 url이 바뀔 경우는 거의 없을거라 판단한다. 

> `fixture`의 `scope`에 아무값도 없으면 `function`이 들어간다.
{: .prompt-tip}

이를 통해 박살나던 통합테스트를 다 돌아가게 해결하였다.

![](/assets/img/zinsap/integration_success.png)


## ✍️전체코드

```python
import os
import pytest
from sqlalchemy.ext.asyncio import create_async_engine
from testcontainers.postgres import PostgresContainer
from starlette.testclient import TestClient
from app.main import app


class PostgresTestContainer(PostgresContainer):
    def get_connection_url(self):
        # windows docker connection
        return super().get_connection_url().replace('localnpipe', 'localhost')


@pytest.fixture(scope="session", autouse=True)
def db_container(session_mocker):
    with PostgresTestContainer(image="postgres:9.5", port=5432) as postgres:
        url = postgres.get_connection_url().replace("psycopg2", "asyncpg")
        mock_os = os.environ | {
            "SQLALCHEMY_DATABASE_URL": url,
            "START_UP_DB_INIT": "True",
            "SECRET_KEY": "yaguen",
            "ALGORITHM": "HS256",
            "ACCESS_TOKEN_EXPIRE_MINUTES": "10",
            "MAIL_PASSWORD": os.environ.get("MAIL_PASSWORD")
        }
        session_mocker.patch.dict(os.environ, mock_os, clear=True)
        yield postgres


@pytest.fixture(scope="session")
def client() -> TestClient:
    with TestClient(app) as c:
        yield c


@pytest.fixture()
async def delete_container(db_container) -> None:
    from app.db.session import Base
    engine = create_async_engine(db_container.get_connection_url().replace("psycopg2", "asyncpg"))
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)
        await conn.run_sync(Base.metadata.create_all)

```

