---
title: FastAPI Jwt 로그인 인증 필터 구현
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-08-03 00:00:00 +0800
categories: [Python,FastAPI]
tags: [FastAPI]
math: true
mermaid: true
image: 
  path: /assets/img/zinsap/authorized.png
comments : true
---

사이드 프로젝트에서 게시글 CRUD를 제작하다가 겪은 난관이다.

비회원과 회원 유저를 식별하고 비회원 유저인 경우 디시인사이드처럼 게시글의 password를 받아야했다.

회원인 경우 `jwt`토큰을 통해 회원을 식별해야했다.

그래서 java에서 쓰이는 `Filter`라는 것이 필요했다.

하지만 FastAPI는 그런걸 지원해주지 않았고, 비슷하게나마 `Depend`를 통해 구현할 수 있었다.

인증이 필요한 컨트롤러마다 전부 붙여주는건 매우 귀찮지만..

더 좋은 방법이 있다면 또 포스팅을 해야겠다.

```python
# api/post.py 
@router.post(
    "",
    status_code=status.HTTP_201_CREATED,
)
async def create(
    post: PostCreate = Body(..., embed=True, alias="post"),
    user: Optional[User] = Depends(get_current_user_authorizer()),
    db: AsyncSession = Depends(get_db)

):
    return await create_post(
        db=db,
        post=post,
        user=user,
    )
```

컨트롤러 부분이다. 여기서 주목해야할건 `get_current_user_authorizer`이다. 

```python
def get_current_user_authorizer(
    *,
    required: bool = False
) -> Callable:
    return get_current_user if required else get_current_user_optional
```

이런식으로 구현되어있다. required이 True면 인증받아야할 회원이 필연적으로 있어야하는거고 없으면 비회원 또는 회원 모두 접근가능한 서비스에 의한 호출을 의미한다.

비회원이나 회원모두 `requried`를 주지 않는경우에 대해서는 `get_current_user_optional`를 호출할것이므로 해당 함수를 작성해보겠다.

```python
async def get_current_user_optional(
    db: AsyncSession = Depends(get_db),
    token: str = Depends(get_authorization_header_retriever(required=False)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> Optional[User]:
    if token:
        return await get_current_user(db, token, jwt_config)
    return None
```

여기서 만약 토큰이 있다면, `get_current_user`라는 현재 접속한 유저를 식별하는 로직으로 갈 것이고 없다면 `None`을 반환하여 비회원임을 나타낼것이다.

```python
# get_authorization_header_retriever
def get_authorization_header_retriever(
    *,
    required: bool = False
) -> Callable:
    return get_authorization_header if required else get_authorization_header_optional

```

인증이 필요없는경우 에러를 발생시키면 안되므로 `get_authorization_header_optional`로 빠지게 설계해야 한다.

```python
async def get_authorization_header_optional(
    api_key: str = Security(GetAPIKeyHeader(name=HEADER_KEY)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> str:
    if api_key:
        return get_authorization_header(api_key, jwt_config)
    return ""

```

이렇게 하면 헤더에 인증키값이 없는경우 `""`를 반환하고 만약 있다면 인증키값을 복호화할 것이다.

```python
def get_authorization_header(
    api_key: str = Security(GetAPIKeyHeader(name=HEADER_KEY)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> str:
    try:
        token_prefix, token = api_key.split(" ")
    except ValueError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown Header"
        )
    if token_prefix != jwt_config.TOKEN_PREFIX:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown Header"
        )
    return token

```

그리고 이를 식별하는 `GetAPIKeyHeader`클래스 코드이다.

```python
class GetAPIKeyHeader(APIKeyHeader):
    async def __call__(
        self,
        request: requests.Request,
    ) -> Optional[str]:
        try:
            return await super().__call__(request)
        except StarletteHTTPException:
            return None
```

## 전체 코드

> `get_db`는 DataBase Session을 반환하고 `jwt_config`는 jwt에 필요한 알고리즘, 시크릿키, 만료시간 등을 가지고 있다.
{: .prompt-tip }

```python

HEADER_KEY = "Authorization"


class GetAPIKeyHeader(APIKeyHeader):
    async def __call__(
        self,
        request: requests.Request,
    ) -> Optional[str]:
        try:
            return await super().__call__(request)
        except StarletteHTTPException:
            return None


def get_current_user_authorizer(
    *,
    required: bool = False
) -> Callable:
    return get_current_user if required else get_current_user_optional


def get_authorization_header_retriever(
    *,
    required: bool = False
) -> Callable:
    return get_authorization_header if required else get_authorization_header_optional


def get_authorization_header(
    api_key: str = Security(GetAPIKeyHeader(name=HEADER_KEY)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> str:
    try:
        token_prefix, token = api_key.split(" ")
    except ValueError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown Header"
        )
    if token_prefix != jwt_config.TOKEN_PREFIX:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown Header"
        )
    return token


async def get_authorization_header_optional(
    api_key: str = Security(GetAPIKeyHeader(name=HEADER_KEY)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> str:
    if api_key:
        return get_authorization_header(api_key, jwt_config)
    return ""


async def get_current_user(
    db: AsyncSession = Depends(get_db),
    token: str = Depends(get_authorization_header_retriever()),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> User:
    try:
        email = get_email_from_token(
            token=token,
            jwt_config=jwt_config
        )
    except ValueError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown token"
        )
    try:
        user = await get_user_by_email(
            db=db,
            email=email,
        )
    except Exception:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Unknown token"
        )
    return user


async def get_current_user_optional(
    db: AsyncSession = Depends(get_db),
    token: str = Depends(get_authorization_header_retriever(required=False)),
    jwt_config: JwtConfig = Depends(get_jwt_config),
) -> Optional[User]:
    if token:
        return await get_current_user(db, token, jwt_config)
    return None

```



## 테스트

### 1. 비로그인 회원이 글을 쓸 때

```bash
curl -X 'POST' \
  'http://localhost:8000/post' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "post": {
    "title": "string",
    "content": "string",
    "password": "string"
  }
}'
```

![](/assets/img/zinsap/non_member_post.png)

성공

### 2. 로그인한 회원이 글을 쓸 때 (식별되지 않는 유저인 경우)

```bash
curl -X 'POST' \
  'http://localhost:8000/post' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 123' \
  -H 'Content-Type: application/json' \
  -d '{
  "post": {
    "title": "string",
    "content": "string",
    "password": "string"
  }
}'
```

`-H 'Authorization: Bearer 123' \`이 부분을 주목하면 토큰값으로 '123'을 넘기고 있다. 

![](/assets/img/zinsap/member_non_header.png)

실패

### 3. 로그인한 회원이 글을 쓸 때 (식별되는 경우)

```bash
curl -X 'POST' \
  'http://localhost:8000/post' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJpZCI6MSwiZXhwIjoxNjYyMDgxNzIyfQ.XTLA4qM4txDRqckkGnYePDAlwSaPnxPpywnKpxL2h_8' \
  -H 'Content-Type: application/json' \
  -d '{
  "post": {
    "title": "string",
    "content": "string",
    "password": "string"
  }
}'
```

> 로그인 컨트롤러를 통해 유저정보를 담고 있는 토큰값을 가지고 인증을 하고 있다.
{: .prompt-info }

![](/assets/img/zinsap/member_header.png)

여러번 포스팅해도 해당 회원를 찾아 post테이블에 회원정보를 입력하는것을 확인할 수 있다.

![](/assets/img/zinsap/post_database.png)

