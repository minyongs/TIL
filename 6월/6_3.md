# Rest API

## Rest API의 특징

- **리소스 중심**: 모든 데이터는 리소스로 간주되며, 각 리소스는 고유한 URI(Uniform Resource Identifier)를 가진다.
- **HTTP 메서드 사용**: CRUD(Create, Read, Update, Delete) 작업을 HTTP 메서드(GET, POST, PUT, DELETE 등)를 통해 수행한다.
- **상태 비저장성**: 서버는 클라이언트의 상태를 유지하지 않으며, 각 요청은 독립적이다.
- **캐시 가능성**: 응답 데이터는 캐시될 수 있어 성능 향상에 기여한다.
- **계층화된 시스템**: 클라이언트는 중간 서버(프록시, 게이트웨이 등)를 통해 간접적으로 서버와 통신할 수 있다.

- `@Controller` 대신  `@RestController` 사용.
- `Controller` 에서 View 로 반환해주는 코드가 필수적인 타임리프와는 달리 Rest API 를 설계할땐 데이터만 리턴 해주면 된다 !

```java
//thymeleaf 
   @GetMapping("/writeform")
    public String writeFormGet(Model model){
        model.addAttribute("board",new Board());
        return "board/writeform";
   } //->model.addAttribute 와 같이 데이터를 넘겨주는 작업 필요.
```

```java
//rest api
  @GetMapping("/{id}")
    public Todo getTodo(@PathVariable("id") Long id){
        return todoService.getTodoById(id);
    }
    //->Todo객체가 json 으로 넘어가기 때문에 객체로 리턴만 해주면 된다.
```

## Json (JavaScript Object Notation)

### JSON의 특징

1. **경량**: 텍스트 기반 데이터 형식으로, XML보다 가볍고 간단함.
2. **가독성**: 사람이 읽고 쓰기 쉬우며, 기계가 파싱하고 생성하기도 쉬움.
3. **언어 독립성**: 대부분의 프로그래밍 언어에서 JSON을 쉽게 파싱하고 생성할 수 있는 라이브러리를 제공.

### JSON 구조

```json
//객체
{
  "name": "John",
  "age": 30,
  "isStudent": false
}
//키-값 쌍의 집합으로 , 중괄호 {} 로 감싸짐.
```

```json
//배열
[
  "Apple",
  "Banana",
  "Cherry"
]
//값의 순서 있는 리스트로 대괄호 []로 감싸짐.
```

### JSON의 데이터 타입

- 문자열 (String): 큰따옴표 `""`로 감싸진 텍스트
- 숫자 (Number): 정수나 부동 소수점 숫자
- 객체 (Object): 중괄호 `{}`로 감싸진 키-값 쌍의 집합
- 배열 (Array): 대괄호 `[]`로 감싸진 값의 리스트
- 불리언 (Boolean): `true` 또는 `false`
- 널 (Null): `null`

### URL 맵핑

- `GET`→ 목록, 개인정보등 정보를 조회할때 사용.
- `POST`→클라이언트 측에서 서버로 정보를 보내야 할때 사용( ex.글 작성, 유저 추가 ,todo 추가….).
    - `@RequestBody` 로 값 받아옴!
- `PUT/PATCH`
    - `PUT`→내용 전체의 수정이 필요할때(ex. 엔티티 자체가 교체될때).
    - `PATCH`→속성만 간단히 바뀔 때(ex. boolean 값이 True→False로 바뀌는 정도일때).
- `DELETE`  →데이터 삭제가 필요할 때.

[Http://localhost:8080/api/todos](Http://localhost:8080/api/todos) — Get(R) // todoList 리턴
[Http://localhost:8080/api/todos](Http://localhost:8080/api/todos)/{id} — Get(R) // id 에 해당하는 Todo 리턴
[Http://localhost:8080/api/todos](Http://localhost:8080/api/todos) — Post(C) // todo 한 건 저장
[Http://localhost:8080/api/todos](Http://localhost:8080/api/todos) — put/Patch(U) // todo 수정
[Http://localhost:8080/api/todos](Http://localhost:8080/api/todos) — Delete(D) // todo 삭제

→ URL 은 최대한 리소스에 대한 정보만 명시하고 그 리소스로 행할 행동은 `GET` , `POST`, `PUT` ,`PATCH`,`DELETE` 로 결정한다 !

## JSON 테스트 방법

→서버 사이드 렌더링 방식과 같이 시각적으로 나타나지 않기 때문에 다양한 테스트 방식 존재.

- Postman,Talend API ,Swagger 등
- CURL
- .http 파일
