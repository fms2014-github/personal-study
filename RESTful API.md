# RESTful API

# REST란?

Representational State Transfer의 약자이며 http 명세를 바탕으로 리소스를 조회, 생성, 수정, 삭제의 요청 작업을 하기 위한 요청 인터페이스 제약 조건을 의미한다.

REST의 제약조건은 다음과 같은 구성으로 이루어져 있다.

- 자원(Resource)
- 행위(Http Method)
- 표현(Representational)

각 구성으로 이루어진 제약조건은 다음 4가지로 표현할 수 있다.

1. 요청은 자원(Resource)를 식별할 수 있어야 한다. 이를 위해 균일한 자원(Resource) 식별자를 사용한다.
2. 클라이언트는 원하는 경우 리소스를 수정하거나 삭제하기에 충분한 정보를 표현(Representational) 데이터에서 가지며 서버는 리소스를 자세히 설명하는 표현(Representational) 메타데이터를 전송하여 이 조건을 충족 시켜야 한다.
3. 클라이언트는 표현(Representational) 데이터를 추가로 처리하는 방법에 대한 정보(Http Method)를 수신하고 이를 위해 서버는 클라이언트가 리소스를 적절하게 사용할 수 있는 방법에 대한 표현(Representational) 메타데이터가 포함된 명확한 메시지를 전송한다.
4. 클라이언트는 작업을 완료하는 데 필요한 다른 모든 관련 리소스에 대한 정보를 수신한다. 이를 위해 서버는 클라이언트가 더 많은 리소스를 동적으로 검색할 수 있도록 표현에 하이퍼링크를 넣어 전송한다.

# REST의 특징

- 무상태성(Stateless)
- 캐시 가능(Cacheable)
- 일관된 인터페이스
- 계층형 구조
    - 클라이언트와 서버 사이에 여러 중간 계층을 추가하거나 또 다른 서버로 요청을 보내거나 다른 서버로 부터 응답을 받을 수 있는 계층형 구조로 설계가 가능하다.
- Self-descriptiveness(자체 표현 구조)

# RESTful API 설계 가이드

- URI는 정보의 자원을 표현해야한다
    - 맴버 고유번호가 1인 회원정보를 제거
    
    <aside>
    ❌ POST /member/delete/1
    
    </aside>
    
    <aside>
    ✅ DELETE /member/1
    
    </aside>
    
- URI 마지막에 /는 포함하지 않는다
    
    <aside>
    ❌ https://www.domail.com/member/1/
    
    </aside>
    
- 단어간 구분이 필요할 경우 -(dash)를 사용한다.
    
    <aside>
    ✅ https://www.domail.com/members/member-list
    
    </aside>
    
- 소문자를 사용한다.
    
    <aside>
    ✅ https://www.domail.com/users/post-commnets
    
    </aside>
    
- 행위는 URL에 포함하지 않는다.
    
    <aside>
    ❌ GET /member/delete/1
    
    </aside>
    
    <aside>
    ✅ DELETE /member/1
    
    </aside>
    
- Content-Type는 application/json를 우선 제공한다.
- HTTP methods인 POST, GET, PUT, DELETE 등을 적재적소에 사용하여 자원을 표현한다.
- 적절한 HTTP Status를 리턴하여 성공과 에러상태를 표현한다.