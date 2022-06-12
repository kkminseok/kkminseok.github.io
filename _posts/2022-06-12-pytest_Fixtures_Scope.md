---
title: pytest fixture의 scope에 대한 공부
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-23 03:00:00 +0800
categories: [Python,FastAPI]
tags: [FastAPI]
math: true
mermaid: true
image: 
  path: /assets/img/SQLAlchemy/pytest_fixture.png
comments : true
---

프로젝트를 수행하다가 pytest에서 사용하는 `fixture`의 `scope`가 궁금해졌다.

총 5가지의 상태가 있다. (function, class, module, package, session)

# 1. Function

이 상태는 아무것도 명시해주지 않을때 자동적용되는 범위이다.

함수가 단위로 실행된다. 즉, pytest에 작성한 함수를 실행할때마다 수행된다는 것이다.

# 2. Class

```python
@pytest_asyncio.fixture(scope="class")
def dummy_data(request):
    request.cls.num1 = 10
    request.cls.num2 = 20
    print("Execute fixture")


class TestCalculatorClass:
    def test_distance(self, dummy_data):
        print("Test distance function")
        assert distance(self.num1, self.num2) == 10

    def test_sum_of_square(self, dummy_data):
        print("Test sum of square function")
        assert sum_of_square(self.num1, self.num2) == 500


# source code
def distance(num1, num2):
    return abs(num1 - num2)


def sum_of_square(num1, num2):
    return num1 ** 2 + num2 ** 2

```

```bash
Process finished with exit code 0
Execute fixture
PASSED                   [ 50%]Test distance function
PASSED              [100%]Test sum of square function
```

위의 코드를 보면 클래스 단위당 **한 번**호출 하는것을 알 수 있다.

참고로

```python

@pytest_asyncio.fixture(scope="class")
def dummy_data(request):
    request.cls.num1 = 10
    request.cls.num2 = 20
    print("Execute fixture")


@pytest.mark.usefixtures("dummy_data")
class TestCalculatorClass:
    def test_distance(self):
        print("Test distance function")
        assert distance(self.num1, self.num2) == 10

    def test_sum_of_square(self):
        print("Test sum of square function")
        assert sum_of_square(self.num1, self.num2) == 500

```

위에 `@pytest.mark.usefixutres("fixture명")`을 통해 인스턴스 함수 파라미터에 fixture를 적어서 지저분하게 만드는걸 방지할 수 있다.

# 3. Module and package

마찬가지로 모듈당 한 번, 패키지당 한 번 호출하는데 어떤 식이냐

밑의 코드는 파일 여는것을 함수로 만들고, `module`을 적용하고, 두 함수에 이를 호출하였다.

디폴트값인 `function`에서는 함수 호출때마다 파일을 열고 닫았지만

```python
@pytest.fixture(scope="module")
def read_config():
    with open("app.json") as f:
        config = json.load(f)
        logging.info("Read config")
    return config

def test1(read_config):
    logging.info("Test function 1")
    assert read_config == {}

def test2(read_config):
    logging.info("Test function 2")
    assert read_config == {}
```

```bash
# test/test_code.py::test1 
# ----------- live log setup -----------
# INFO     root:test_code.py:88 Read config
# ----------- live log call -----------
# INFO     root:test_code.py:93 Test function 1
# PASSED                                                                                                                                         [ 75%]
# test/test_code.py::test2 
# ----------- live log call -----------
# INFO     root:test_code.py:98 Test function 2
# PASSED 
```

로그를 보면 딱 한 번만 여는걸 알 수 있다.

# 4. Session

여기서 말하는 세션이란, `pytest`를 실행할때마다 하나의 세션을 얻는데, 이 세션을 말한다.

즉, 실행할때마다 실행되는 것들이다. 이를 통해 테스트용 데이터베이스를 연결하거나 끊는데에 사용된다.

