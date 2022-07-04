# REST
REST는 `Representational State Transfer`의 약자로 자원을 이름으로 구분하여 
해당 자원의 상태를 주고받는 모든 것을 의미한다.

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고
2. HTTP Method(POST, GET, PUT, DELETE)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미한다.

## CRUD Operation
CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인
Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로
REST에서의 CRUD Operation 동작 예시는 다음과 같다.

- Create: 데이터 생성(POST)
- Read: 데이터 조회(GET)
- Update: 데이터 수정(PUT)
- Delete: 데이터 삭제(DELETE)

# REST 구성 요소
- 자원(Resource): HTTP URI
- 자원에 대한 행위(Verb): HTTP Method
- 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load

# REST의 특징
- Server-Client(서버-클라이언트 구조)
- Stateless(무상태)
- Cacheable(캐시 처리 가능)
- Layered System(계층화)
- Uniform Interface(인터페이스 일관성)

# REST 장점
- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

# REST API
REST API란 REST의 원리를 따르는 API를 의미한다.

## REST API 중심 규칙
1. URI는 정보의 자원을 표현해야 한다. (리소스명은 동사보다는 명사를 사용)
```text
GET /members/1
```
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE등)로 표현
```text
DELETE /members/1
```

## 주의할 점
1. 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용
```text
http://localhost/houses/apartments
http://localhost/animals/mammals/whales
```

2. URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
```text
http://localhost/houses/apartments/ (X)
http://localhost/houses/apartments  (O)
```

3. 하이픈(-)은 URI 가독성을 높이는데 사용
4. 밑줄(_)은 URI에 사용하지 않는다.
5. URI 경로에는 소문자가 적합하다.
6. 파일 확장자는 URI에 포함시키지 않는다.
