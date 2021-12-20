# REST API

---

<br>

### REST란?

REST(REpresentational State Transfer): REST는 분산 하이미디어 시스템(ex. 웹)을 위한 아키텍쳐 스타일이면서 아키텍쳐 스타일의 집합(하이브리드 아키텍쳐 스타일).

### 아키텍쳐 스타일이란?

아키텍쳐 스타일은 제약 조건들의 집합이다.

**즉, 모든 제약 조건들을 다 지켜야 REST를 따른다고 할 수 있다.**

- REST 원리를 따르는 시스템은 종종 **RESTful**이란 용어로 지칭된다.

### REST의 목적?

독립적 진화

- 서버와 클라이언트가 각각 독립적으로 진화한다.
  - 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다. (ex. 웹 페이지를 변경했다고 웹 브라우저(ex. 크롬, 사파리)를 업데이트 할 필요는 없다.)
- REST를 만들게 된 계기 : "How do I improve HTTP without breaking the web" (Fielding, Roy T)

<br>

### REST를 구성하는 스타일

- Client-Server
  - 클라이언트/서버 : 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다
- Stateless
  - 무상태 : 각 요청 간 클라이언트의 콘텍스트가 서버에 저장 되어서는 안된다.
- Cacheable
  - 클라언트는 응답을 캐싱할 수 있어야 한다.
    - 잘 관리되는 캐싱은 클라이언트-서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 scalability와 성능을 향상시킨다.
- **Uniform Interface**
  - 인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 한다.
- Layered System
  - 계층화 : 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다. 중간 서버는 로드 밸런싱 기능이나 공유 캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용하다.
- Code-On-Demand(Optional)
  - 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.(JavaScript)

**오늘날 HTTP만 따라도 대부분의 조건들을(Uniform Interface 제외) 만족 할 수 있다.**

<br>

### Uniform Interface의 제약 조건

- Identification of resource
  - 자원의 식별: 요청 내에 기술된 개별 자원을 식별할 수 있어야 한다. 웹 기반의 REST 시스템에서의 URI의 사용을 예로 들 수 있다. 자원 그 자체는 클라이언트가 받는 문서와는 개념적으로 분리되어 있다. 예를 들어, 서버는 데이터베이스 내부의 자료를 직접 전송하는 대신, 데이터베이스 레코드를 HTML, XML이나 JSON 등의 형식으로 전송한다.
- Manipulation of resources through representations
  - 메세지를 통한 리소스의 조작: 클라이언트가 어떤 자원을 지칭하는 메시지와 특정 메타데이터만 가지고 있다면 이것으로 서버 상의 해당 자원을 변경·삭제할 수 있는 충분한 정보를 가지고 있는 것이다.
- **Self-descriptive message**
  - 자기서술적 메세지: 각 메시지는 자신을 어떻게 처리해야 하는지에 대한 충분한 정보를 포함해야 한다. 예를 들어 MIME type과 같은 인터넷 미디어 타입을 전달한다면, 그 메시지에는 어떤 파서를 이용해야 하는지에 대한 정보도 포함해야 한다. 미디어 타입만 가지고도, 클라이언트는 어떻게 그 내용을 처리해야할 지 알 수 있어야 한다. 메시지를 이해하기 위해 그 내용까지 살펴봐야 한다면, 그 메시지는 자기서술적이 아니다. 예를 들어, 단순히 "application/xml"이라는 미디어 타입은, 실제 내용을 다운로드 받지 않으면 그 메시지만 가지고는 무엇을 해야할지에 대해 충분히 알려주지 못한다.
- **Hypermedia As The Engine of Application State(HATEOAS)**
  - 애플리케이션의 상태에 대한 엔진으로서 하이퍼미디어: 만약에 클라이언트가 관련된 리소스에 접근하기를 원한다면, 리턴되는 지시자에서 구별될 수 있어야 한다. 충분한 콘텍스트 속에서의 URI를 제공해주는 하이퍼텍스트 링크의 예를 들 수 있겠다.

HTML를 사용하는 흔한 웹페이지와 다르게 JSON을 쓰는 HTTP API는 Self-descriptive message와 HATEOAS를 지키기 위한 과정이 매우 복잡하다.

- 위는 **사람-기계**인 HTML과 다르게 JSON은 **기계-기계**여서 발생하는 문제이다.
  - JSON이 Self-descriptive message를 지키기 위해서는 (HTML은 IANA에 이미 다 정의 되어있음)
    1. 미디어 타입을 하나 정의한다.
    2. 미디어 타입 문서를 작성한다. 이 문서에 "id"가 뭐고 "title"이 뭔지 의미를 정의한다.
    3. IANA에 미디어 타입을 등록한다. 이 때 만든 문서를 미디어 타입의 명세로 등록한다.
  - JSON이 HATEOAS를 지키기 위해서는 (HTML은 a 태그가 존재)
    1. 모든 데이터에 링크를 추가한다.
    2. 링크를 표현하는 방법을 정의 해야한다.

**따라서, 오늘날 대부분의 REST API는 Self-descriptive message와 HATEOAS를 지키지 않고 있습니다.**

- 즉, REST API가 아니지만 REST API라고 부른다.(현재 상태)

<br>

출처: [Day1, 2-2. 그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc&t=1343s), [REST](https://ko.wikipedia.org/wiki/REST)
