## **REST API**

**1.REST API의 중심 규칙**
REST에서 가장 중요한 기본적인 규칙은 두 가지이다. URI는 자원을 표현하는 데에 집중하고 행위에 대한 정의는 HTTP Method를 통해 하는 것이 REST한 API를 설계하는 중심 규칙이다.

<u>1.**URI는 정보의 자원을 표현해야 한다</u>.**리소스명은 동사보다는 명사를 사용한다. URI는 자원을 표현하는데 중점을 두어야 한다. get 같은 행위에 대한 표현이 들어가서는 안된다.

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

<u>2.**자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)으로 표현한다.</u>**

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```



**2.HTTP Method**
주로 5가지의 Method(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD를 구현한다.

| Method | Action         | 역할                     |
| :----- | :------------- | :----------------------- |
| GET    | index/retrieve | 모든/특정 리소스를 조회  |
| POST   | create         | 리소스를 생성            |
| PUT    | update all     | **리소스의 전체를 갱신** |
| PATCH  | update         | **리소스의 일부를 갱신** |
| DELETE | delete         | 리소스를 삭제            |

**3.REST API의 구성**

REST API는 자원(Resource), 행위(Verb), 표현(Representations)의 3가지 요소로 구성된다. REST는 자체 표현 구조(Self-descriptiveness)로 구성되어 REST API만으로 요청을 이해할 수 있다.

| 구성 요소       | 내용                    | 표현 방법             |
| :-------------- | :---------------------- | :-------------------- |
| Resource        | 자원                    | HTTP URI              |
| Verb            | 자원에 대한 행위        | HTTP Method           |
| Representations | 자원에 대한 행위의 내용 | HTTP Message Pay Load |

**4.**

**GET**-todos리소스에서 모든 todo를 조회(index)한다.

**POST**- 새로운 todo를 생성한다.

**PUT** - PUT은 특정 리소스의 전체를 갱신할때 사용한다. todos 리소스에서 id 를 사용하여 todo를 특정하여 id를 제외한 리소스 전체를 갱신한다.
**PATCH**-PATCH는 특정 리소스의 일부를 갱신할때 사용한다. todos리소스의 id를 사용하여 todo을 특정하여 completed만을 true로 갱신한다.
**DELETE**-todos 리소스에서 id를 사용하여 todo를 특정하고 삭제한다.

