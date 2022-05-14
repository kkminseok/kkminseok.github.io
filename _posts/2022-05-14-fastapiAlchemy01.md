---
title: FastAPI 공식문서 따라하기 번외[10] - Alchemy연동
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-14 00:00:00 +0800
categories: [Python,FastAPI]
tags: [FastAPI]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---


> <https://fastapi.tiangolo.com/tutorial/sql-databases/?h=sql> 공식문서 따라하는 글
{: .prompt-tip }

`SQLAlchemy`를 사용해보겠다. 공식문서에서는 `SQLite`를 사용하였으므로 `SQLite`를 DBserver로 두겠다.

# ☑️ 1.1. ORM

`FastAPI`는 어떤 `database`와도 잘 작옹한다.

흔히 `ORM`(object-relational mapping Library)라는 패턴을 사용한다.

`ORM`에는  `object`의 `code`와 `database tables`을 "`map`"으로 바꾸는 기능을 가지고 있다.

`ORM`을 사용하면 `class`로 표현되는 테이블을 만들수 있는데, 해당 클래스의 각 속성값은 이름(`name`)과 타입(`type`)이 있는 `column`으로 표현된다.

예를들어 `orion_cat`(`Pet`의 인스턴스)이라는 오브젝트가 있다고 치면, 이 오브젝트는 `orion_cat.type`이라는 속성값을 `type`이라는 컬럼에 가지고 있다. 그리고 `cat`이라는 속성값을 가질 수 있다.

`ORM`은 또한 `table`과 `entites`의 연관관계를 구성하는데 도와주고, 커넥션을 만드는데 도와준다.

이러한 방식을 통해 `orion_cat`은 `orion_cat.owner`라는 속성값을 가질 수 있고 `owner`는 `owners`라는 테이블의 값이고 pet의 owner라는 데이터를 포함할 수 있다.

또한, 이를 통해 `orion_cat.owner.name`은 `owners` 테이블의 `name`이라는 속성값으로 접근할 수 있다.

이렇게 `ORM`은 사용자가 `pet`이라는 오브젝트에 접근하여 해당`pet`의 `owner`를 찾을 수 있게 `owneers`테이블에서 알맞은 `owner`를 찾는다.

이러한 `ORM`의 종류로는 Django-ORM, SQLalchemy ORM, Peewee 등이 있다.

공식 문서에서는 `SQLAlchemy ORM`을 사용하여 예제를 작성할 것이다.

> 공식문서에서는 `Peewee`를 사용한 것도 해당 문서에서 나타내고 있다고 한다.
{: prompt-tip}

-----

# ☑️ 1.2. File structure

공식문서에서는 파일구조를 다음과 같이 나타냈다.

```text
.
└── sql_app
    ├── __init__.py
    ├── crud.py
    ├── database.py
    ├── main.py
    ├── models.py
    └── schemas.py
```

```bash
mkdir sql_app
cd sql_app
touch __init__.py
touch crud.py
touch database.py
touch main.py
touch models.py
touch schemas.py
```

를 통해 디렉터리 구성을 똑같이 해주었다.

-----

# ☑️ 1.3. Create the SQLAlchemy parts

`sql_app/databse.py`파일이다.

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./sql_app.db"
#Postgres 사용을 원할경우
# SQLALCHEMY_DATABASE_URL = "postgresql://user:password@postgresserver/db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

```

- 이 예제는 `SQLiteDB`에 connection을 맺고 있다.
- `sql_app.db`는 같은 디렉터리에 있어야 한다.(`./sql_app.db`로 끝냈기에)

-----

# ☑️ 1.4. Create the SQLAlchemy `engine`

위의 코드에서

```python
engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
)
```

이부분이 `SQLAlchemy`의 `engine`을 만드는 부분이다. 

맨 처음에는 `engine`을 만들어야한다.

```python
connect_args={"check_same_thread": False}
```

이 부분은 `SQLite`에서만 사용하는 구문이라고 한다. 

> `SQLite`는 각 스레드가 독립적인 요청을 처리할 때 하나의 스레드만 `SQLite`와 통신을 할 수 있다.<br>이는 다른 스레드들이 동일한 db커넥션을 접근하는 것을 방지하기 위해서이다.<br> 그러나 `FastAPI`에서는 일반 함수(def)를 사용하면 동일한 요청에 대해 두 개 이상의 스레드가 데이터베이스와 상호작용을 할 수 있으므로 `SQLite`에서 `connect_args={"check_same_thread": False}`를 허용해야 한다.<br> 또한 각 요청이 독립적인 하나만의 db커넥션 세션을 가져야하는것을 보장해야하는데, 그러기에 해당 구문을 안 쓴 `default`값이 필요하지 않는다고 한다.
:{. prompt-info}

# ☑️ 1.5. Create a `SessionLocal` class

`SessionLocal`클래스 각 인스턴스는 database의 session이 된다고 한다.

무슨 말인지 모르겠지만 일단 더 알아보겠다.

클래스 자체만으로는 `database session`이 아니라고 한다.

그러나 `SessionLocal`클래스의 인스턴스를 만들면 그 인스턴스는 `database session`이 된다.

예제에서는 `Session`이라는 것이 나중에 `SQLAlchemy`에서 임포트 되므로 이름이 겹치지 않게 `SessionLocal`이라는 변수로 지었다고 한다.

`sessionmaker`라는 함수를 통해 `SessionLocal` 클래스를 만들 것이다.

위의 코드에서

```python
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
```

------

# ☑️ 1.6. Create a `Base` class

`declarative_base()`라는 함수의 리턴값을 통해 `Base`를 만들것이다.

나중 예제에서 이 클래스를 상속하여 `ORM model`형태를 가진 데이터베이스 모델이나 클래스로 만들어질 것이라고 합니다.

위의 코드에서

```python
Base = declarative_base()
```

# ☑️ 2.1. Create the database models

`sql_app/models.py`를 작성한다고 한다.

`Base` 클래스를 통해서 `SQLAlchemy models`을 만들것이다.

`SQLAlchemy models`을 만들려면 `Base` 클래스가 사용되어야 한다고 한다.

> `SQLAlchemy`에서 사용하는 `model`이라는 단어는 데이터베이스와 연관된 **클래스**나 **인스턴스**을 지칭한다.<br>그러나 `Pydantic`에서 사용하는 `model`이라는 단어는 `data validation`, `conversion`, `Swagger`에서나 `redoc`에서 사용하는 클래스나 인스턴스를 말한다.
{: .prompt-tip}

예제에서 작성한 `database.py`에서 `Base`를 임포트 하고 그 클래스를 상속받은 클래스를 만들어야한다. 만들어진 클래스는 `SQLAlchemy models`이 될 것이다.

```python
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

from .database import Base


class User(Base):
    __tablename__ = "users"
    ...


class Item(Base):
    __tablename__ = "items"
    ...

```

이렇게 `Base`를 상속받고, 상속받기위해 `from .database import Base`구문을 작성한걸 볼 수 있다.

`__tablename__` 속성은 사용하고 있는 데이터베이스의 각 모델에 해당하는 테이블의 이름이고 곧, `SQLAlchemy`의 형태 값이다.

-----

# ☑️ 2.2. Create model attributes/columns

`model`의 속성값을 만들 것이다.

각 속성값들은 데이터베이스 테이블의 컬럼에 해당하는 값들이다.

`SQLAlchemy`의 `Column`을 통하여 default value를 설정할 수 있다.

또한 `Integer`, `String`, `Boolean`같은 "type"을 지정해줄 수 있다. 데이터베이스에서 정의된 타입을 써야한다.

```python
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

from .database import Base


class User(Base):
    __tablename__ = "users"

    #추가된부분
    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    hashed_password = Column(String)
    is_active = Column(Boolean, default=True)



class Item(Base):
    __tablename__ = "items"

    #추가된부분
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, index=True)
    description = Column(String, index=True)
    owner_id = Column(Integer, ForeignKey("users.id"))

```

-----

# ☑️ 2.3. Create the relationships

`SQlAlchemy ORM`을 통해 `relationship`을 사용할 수 있다.

이것을 통해 한 속성과 연관된 다른 테이블들의 속성값들을 포함하게 할 수 있다.

```python
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

from .database import Base


class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    hashed_password = Column(String)
    is_active = Column(Boolean, default=True)

    #추가된부분
    items = relationship("Item", back_populates="owner")


class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, index=True)
    description = Column(String, index=True)
    owner_id = Column(Integer, ForeignKey("users.id"))

    #추가된부분
    owner = relationship("User", back_populates="items")

```

`my_user.items`와 라는 구문으로 `User`가 가지고 있는 `items`라는 속성값에 접근을 시도할 때, `users`테이블에 지정된 `foreign key`를 통해 `SQLAlchemy models`형태의 `Item`리스트에 접근합니다.

`my_user.items`라는 구문으로 속성값에 접근하면 `SQLAlchemy`는 데이터베이스에서 `items`테이블과 그 테이블의 정보들을 가져와서 값을 채운다.

만약 `Item`에 있는 `owner`속성에 접근하면, `users` 테이블에 있는 `User`라는 `SQLAlchemy model`을 포함 시키게 된다. `foregin key`와 `owner_id`이라는 속성/컬럼을 통해 `users`테이블에서 가져올 `record`값을 알 수 있다.











2부는 다음에.. 영어 번역 어렵다.


