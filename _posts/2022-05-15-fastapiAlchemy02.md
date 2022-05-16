---
title: FastAPI 공식문서 따라하기 번외[10][1] - Alchemy연동
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

# ☑️ 3.1. Create the Pydantic models

`sql_app/schemas.py`에 작성할 것이다.

> `SQLAlchemy` 과 `Pydantic`에서 설명하는 `models`을 혼동하면 안 된다. `models.py`에 있는 것은 `SQLAlchemy`의 모델이고, `schemas.py`은 `Pydantic`의 모델이다.<br> `Pydantic` 모델은 `schema`라고 불러질 수 있다. 
{: .prompt-tip}

# ☑️ 3.2. Create initial Pydantic models / schemas

`ItemBase`와 `UserBase`라는 `Pydantic` 모델을 만들 것이다. 공식문서에서는 `모델`대신에 `스키마`라고 명명하기에 나도 스키마라고 설정할 것이다.

속성을 만들거나 읽는용도의 오브젝트이다.

`ItemCreate`와 `UserCreate`라는 클래스는 위의 클래스들을 상속 받을 것이고(같은 속성을 갖습니다.), 추가적인 속성이 필요하다면 그 속성값을 만들 것이다.

밑의 예제에서는 `password`라는 속성값을 만든다.

하지만, 보안적으로 `password`라는 값은 다른 `Pydantic`모델에는 존재하지 말아야한다. 왜냐하면 `user`를 읽을때 `API`가 `password`를 보내면 안 되기 때문이다.

`SQLAlchemy` 모델은 속성을 정의할 때 `=`연산자를 사용하고, `Column`에 파라미터를 넣어서 넘긴다.  
```python
name = Column(String)
```

반면 `Pydantic` 모델은 `:`을 이용한다.
```python
name: str
```

따라서 `=`와 `:`을 현혹하면 안된다.

-----

# ☑️ 3.3. Create Pydantic models/ schemas for reading / returning

위에서 `Pydantic` 모델을 만들었기에 이를 이용해서 데이터를 읽고, `API`를 통해 받아올 데이터가 있으면 받아올 것이다.

`item`을 만들기 전에는, 우리는 어떤 데이터에 `ID`를 할당할지는 중요하지 않지만 우리가 데이터를 읽을 때에는 `ID`를 알고 있어야 한다.

같은 방식으로 `user`라는 데이터를 읽기 위해서는 우리는 `user` 안에 `items`를 선언해야 한다.

`items`를 읽기 위해서는 `items`의 `ID` 뿐만 아니라 모든 데이터를 `Pydantic` 모델인 `Item`에 정의해야 한다.

```python
from pydantic import BaseModel


class ItemBase(BaseModel):
    title: str
    description: str | None = None


class ItemCreate(ItemBase):
    pass


class Item(ItemBase):
    id: int
    owner_id: int



class UserBase(BaseModel):
    email: str


class UserCreate(UserBase):
    password: str


class User(UserBase):
    id: int
    is_active: bool
    items: list[Item] = []


```

# ☑️ 3.4. Use Pydantic's `orm_mode`

`Pydantic`모델인 `Item`과 `User`를 읽기 위해서는 추가적인 설정이 필요하다. 이를 `config`class라고 지을 것이다.

위의 코드에서 추가되는 부분이 있다.

```python
from pydantic import BaseModel


class ItemBase(BaseModel):
    title: str
    description: str | None = None


class ItemCreate(ItemBase):
    pass


class Item(ItemBase):
    id: int
    owner_id: int
    
    #추가된 부분
    class Config:
        orm_mode = True


class UserBase(BaseModel):
    email: str


class UserCreate(UserBase):
    password: str


class User(UserBase):
    id: int
    is_active: bool
    items: list[Item] = []

    #추가된 부분
    class Config:
        orm_mode = True

```

> `orm_mode = True` 구문을 보면 `=`를 사용하여 할당하였다. `:`는 타입을 선언할 때 쓰는것이고 `config value`는 타입을 선언하는 것이 아니다.
{: .prompt-tip}

`orm_mode`는 `Pydantic` 모델을 읽는데, `dict`타입 뿐만 아니라 `ORM`모델일지라도 읽어버린다.

만약 `id`라는 값을 `dict`에서 가져오고 싶다면

```python
id = data["id"]
```

로 쓸수 있지만 또한,

```python
id = data.id
```

로도 쓸 수 있다.

이런 방식으로 `Pydantic` 모델은 `ORM`와 양립할 수 있고, `path operation`에다가 `response_model`을 선선할 수 있다.

데이터베이스 모델을 리턴할 수 있고, 그 데이터를 읽을 수 있게 도와준다고 한다.

## Techinal Details about ORM mode

`SQLAlchemy`은 `lazy loading`을 사용하는데, 간단히 말하면 데이터를 한 번에 다 받아오는 것이 아니고 관계없는 데이터는 그때 받아오는 것을 의미한다.

예를들어서 `items`에 다음과 같이 접근한다고 가정하자.
```python
current_user.items
```

`SQLAlchemy`은 `items` 테이블에 가서 `user`에 맞는 `items`를 가져오는데, 접근하지 않으면 가져올 일이 없다는 것이다.
> 이렇게 설명하는게 맞나?.. 해석을 잘못한거 같음

`orm_mode`가 없는데 `SQLAlchemy`모델을 `path operation`에서 리턴 받았다면 그 모델은 연관관계를 설정했음에도 불구하고 연관관계 데이터를 포함하지 않았을 것이라고 한다.

즉,`ORM mode`에서는, `Pydantic`에 접근하고 사용자가  원하는 데이터 명시하여 데이터를 얻을 수 있다.

-----

# ☑️ 4.1. CRUD utils

`sql_app/crud.py`에 CRUD관련 사항을 작성할 것이다.

공식문서에서는 CR까지만 작성한다고 한다.

# ☑️ 4.2. Read Data

`sqlalchemy.orm`에서 `Session`을 임포트해야 한다.

`Session`은 `db`라는 타입을 파라미터에 선언할 수 있게 하여 타입체크를 더 효율적으로 하게 해준다.

`models`(SQLAlchemy models)과 `schemas`(Pydantic models) 또한 임포트 해야한다.

공식 문서에서 명세를 알려줬는데..

- ID와 email을 통해 user 한 명을 읽을 것이다.
- 여러 users를 읽을 것이다.
- 여러 items를 읽을 것이다.

라고 합니다.

```python
from sqlalchemy.orm import Session

from . import models, schemas


def get_user(db: Session, user_id: int):
    return db.query(models.User).filter(models.User.id == user_id).first()


def get_user_by_email(db: Session, email: str):
    return db.query(models.User).filter(models.User.email == email).first()


def get_users(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.User).offset(skip).limit(limit).all()

def get_items(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.Item).offset(skip).limit(limit).all()

```

# ☑️ 4.2. Create Data

- `SQLAlchemy`모델의 인스턴스를 만들것이다.
- `database session`에 만든 인스턴스를 `add`할 것이다.
- 바뀐 database정보를 `commit`할 것이다.
- `refresh`해서 데이터베이스에 새로운 데이터가 들어왔는지 확인한다.

```python
from sqlalchemy.orm import Session

from . import models, schemas

def create_user(db: Session, user: schemas.UserCreate):
    fake_hashed_password = user.password + "notreallyhashed"
    db_user = models.User(email=user.email, hashed_password=fake_hashed_password)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user


def create_user_item(db: Session, item: schemas.ItemCreate, user_id: int):
    db_item = models.Item(**item.dict(), owner_id=user_id)
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item

```

>`fake_hashed_password`는 보이는것 처럼 해쉬함수를 `password`에 더해 안전한 값으로 바꿔버린다.예제에서는  간단하게 보여주기 위해 진짜 해쉬함수로 넣지 않았다. <br>만약 클라이언트에서 암호화되지 않은 패스워드를 넘기면, 그값으로 해쉬함수로 만들고 저장해야한다.
{: .prompt-tip}

> `dict` 자료형으로 `Pydantic`model을 담아야 `Item`의 각 속성값을 읽을 수 있다. `item.dict()`처럼 말이다.<br>만약 `dict`의 `key-value`쌍으로 `SQLAlchemy`모델인 `Item`에 넘기고 싶다면 `Item(**item.dict())`처럼 사용하면 된다.<br>`owner_id`는 `Pydantic`모델에서 제공하지 않기에 `Item(**item.dict(), owner_id=user_id)`로 넘겨야 한다.



