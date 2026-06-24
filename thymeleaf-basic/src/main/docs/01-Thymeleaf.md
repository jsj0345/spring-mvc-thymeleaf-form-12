## 타임리프 기본 기능 

### 1. 타임리프
**타임리프 특징**
- 서버 사이드 HTML 렌더링(SSR)
- 네츄럴 템플릿

**서버 사이드 HTML 렌더링 (SSR)**
타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.

**네츄럴 템플릿**
타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고,
서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.
JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어서, JSP 파일 자체를 그대로 웹 브라우저에서 열어보면
JSP 소스코드와 HTML이 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다.
오직 서버를 통해서 JSP가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.
반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다.
물론 이 경우 동적으로 결과가 렌더링 되지는 않는다.
하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바로 확인할 수 있다.
이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿이라 한다. 

**타임리프 기본 기능**
타임리프를 사용하려면 다음 선언을 하면 된다.

**타임리프 사용 선언**
`<html xmlns:th="http://www.thymelear.org">`

**기본 표현식**
타임리프는 다음과 같은 기본 표현식들을 제공한다. 지금부터 하나씩 알아보자.

```text
- 간단한 표현:
-> 변수 표현식 : ${...}
-> 선택 변수 표현식 : *{...}
-> 메시지 표현식 : #{...}
-> 링크 URL 표현식 : @{...}
-> 조각 표현식 : ~{...}

- 리터럴 
-> 텍스트: 'one text', 'Another one!' , ...
-> 숫자 : 0, 34 , 3.0, 12.3, ...
-> 불린 : true, false
-> 널 : null
-> 리터럴 토큰 : one, sometext, main, ...

- 문자 연산 :
-> 문자 합치기 : +
-> 리터럴 대체 : |The name is ${name}|

- 산술 연산 :
-> Binary operations : +, -, *, /, %
-> Minus sign (unary operator) : -

- 불린 연산 :
-> Binary operators : and, or
-> Boolean negation (unary operator) : !, not

- 비교와 동등 :
-> 비교 : >, <, >=, <= (gt, lt, ge, le)
-> 동등 연산 : ==, != (eq, ne)

- 조건 연산 :
-> If-then : (if) ? (then)
-> If-then-else : (if) ? (then) : (else)
-> Default : (value) ? : (defaultvalue)

- 특별한 토큰 :
-> No-Operation : _  
```

### 2. 텍스트 - text, utext
타임리프의 가장 기본 기능인 텍스트를 출력하는 기능 먼저 알아보자.

타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의해서 동작한다. 
HTML의 콘텐츠에 데이터를 출력할 때는 다음과 같이 `th:text`를 사용하면 된다.
`<span th:text="${data}>`
HTML 태그의 속성이 아니라 HTML 컨텐츠 영역 안에서 직접 데이터를 출력하고 싶으면
다음과 같이 `[[...]]`를 사용하면 된다.
`콘텐츠 안에서 직접 출력하기 = [[${data}]]`

```java
package hello.thymeleaf.basic;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/basic")
public class BasicController {

  @GetMapping("/text-basic")
  public String textBasic(Model model) {
    model.addAttribute("data", "Hello Spring!");
    return "basic/text-basic"; 
  }
  
}
```

`/resources/templates/basic/text-basic.html`
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
    <h1>콘텐츠에 데이터 출력하기</h1>
    <ul>
        <li>th:text 사용 <span th:text="${data}"></span></li>
        <li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>
    </ul>
</body>
</html>
```

치환된 결과가 서로 같다는 것을 볼 수 있다. 

**Escape**
HTML 문서는 `<`,`>` 같은 특수 문자를 기반으로 정의된다. 
따라서 뷰 템플릿으로 HTML 화면을 생성할 때는 출력하는 데이터에 이러한 특수 문자가 있는 것을 주의해서 사용해야 한다.
앞에서 만든 예제의 데이터를 다음과 같이 변경해서 실행해보자.

`"Hello Spring!"` -> `"Hello <b>Spring</b>"`

`<b>` 태그를 사용해서 Spring!이라는 단어가 진하게 나오도록 해보자.

- 웹 브라우저 : `Hello <b>Spring!</b>`
- 소스보기 : `Hello &lt;b&gt;Spring!&lt;/b&gt;`

개발자가 의도한 것은 `<b>`가 있으면 해당 부분을 강조하는 것이 목적이었다.
그런데 `<b>`태그가 그대로 나온다.
소스보기를 하면 `<` 부분이 `&lt;`로 변경된 것을 확인할 수 있다. 

**HTML 엔티티**
웹 브라우저는 `<`를 HTML 태그의 시작으로 인식한다.
따라서 `<`를 태그의 시작이 아니라 문자로 표현할 수 있는 방법이 필요한데, 이것을 HTML 엔티티라 한다.
그리고 이렇게 HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 이스케이프라 한다.
타임리프가 제공하는 `th:text`, `[[...]]`는 기본적으로 이스케이프를 제공한다.

- `<` -> `&lt;`
- `>` -> `&gt;`

**Unescape**
이스케이프 기능을 사용하지 않으려면 어떻게 해야할까?
타임리프는 다음 두 기능을 제공한다.

- `th:text` -> `th:utext`
- `[[...]]` -> `[(...)]`

```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@GetMapping("/text-unescaped")
public String textUnescaped(Model model) {
  model.addAttribute("data", "Hello <b>Spring!</b>");
  return "basic/text-unescaped";
}
```

`resources/templates/basic/text-unescaped.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thyemleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>text vs utext</h1>
    <ul>
        <li>th:text = <span th:text="${data}"></span></li>
        <li>th:utext = <span th:utext="${data}"></span></li>
    </ul>

    <h1><span th:inline="none">[[...]] vs [(...)]</span></h1>
    <ul>
        <li><span th:inline="none">[[...]] = </span>[[${data}]]</li>
        <li><span th:inline="none">[(...)] = </span>[(${data})]</span></li>
    </ul>
</body>
</html>
```

- `th:inline="none"` : 타임리프는 `[[...]]`를 해석하기 때문에, 화면에 `[[...]]` 글자를 보여줄 수 없다. 
이 테그 안에서는 타임리프가 해석하지 말라는 옵션이다. 

실행해보면 다음과 같이 정상 수행되는 것을 확인할 수 있다.
- 웹 브라우저 : Hello Spring!
- 소스보기 : `Hello <b>Spring!</b>`

주의!
실제 서비스를 개발하다 보면 escape를 사용하지 않아서 HTML이 정상 렌더링 되지 않는 수 많은 문제가 발생 한다.
escape를 기본으로 하고, 꼭 필요할 때만 unescape를 사용하자. 

**escape, unescape**
escape는 HTML 문자를 다른 문자로 해석해서 HTML 오작동을 막는다.
unescape는 그대로 해석을 하지만 오작동의 우려가 있다. 

### 3. 변수 - SpringEL
타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.
변수 표현식 : `${...}`

그리고 이 변수 표현식에는 스프링 EL이라는 스프링이 제공하는 표현식을 사용할 수 있다.

```java
import lombok.Data;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@GetMapping("/variable")
public String variable(Model model) {

  User userA = new User("userA", 10);
  User userB = new User("userB", 20);

  List<User> list = new ArrayList<>();
  list.add(userA);
  list.add(userB);

  Map<String, User> map = new HashMap<>();
  map.put("userA", userA);
  map.put("userB", userB);

  model.addAttribute("user", userA);
  model.addAttribute("users", list);
  model.addAttribute("userMap", map);

  return "basic/variable";
}

@Data
public static class User {
  
  private String username;
  private int age;
  
  public User(String username, int age) {
    this.username = username;
    this.age = age; 
  }
  
}
```

class User 앞에 public을 꼭 붙여주어야 한다.
최신 타임리프에서는 User 클래스 앞에 반드시 public을 사용해야 한다. 
public이 아니면 이후에 설명할 `[[${user}]]`와 같은 객체 직열화에서 오류가 발생할 수 있다.

`/resources/templates/basic/variable.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
    <h1>SpringEL 표현식</h1>
    <ul>Object
        <li>${user.username} = <span th:text="${user.username}"></span></li>
        <li>${user['username']} = <span th:text="${user['username']}"></span></li>
        <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
    </ul>

    <ul>List
        <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>
        <li>${users[0]['username']} = <span th:text="${users[0]['username'}"></span></li>
        <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
    </ul>

    <ul>Map
        <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username"></span></li>
        <li>${userMap['userA']['username'] = <span th:text="${userMap['userA']['username']"></span></li>
        <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()"></span></li>
    </ul>
</body>
</html>
```

**SpringEL 다양한 표현식 사용**

**Object**
- `user.username` : user의 username을 프로퍼티 접근 -> `user.getUsername()`
- `user['username']` : 위와 같음 -> `user.getUsername()`
- `user.getUsername()` : user의 `getUsername()`을 직접 호출

**List**
- `users[0].username` : List에서 첫 번째 회원을 갖고 username 프로퍼티 접근 ->
`list.get(0).getUsername()`
- `users[0]['username']` : 위와 같음. 
- `users[0].getUsername()` : List에서 첫 번째 회원을 찾고 메서드 직접 호출 

**Map**
- `userMap['userA'].username` : Map에서 userA를 찾고, username 프로퍼티 접근 ->
`map.get("userA").getUsername()`
- `userMap['userA']['username']` : 위와 같음. 
- `userMap['userA'].getUsername()` : Map에서 userA를 찾고 메서드 직접 호출

**지역 변수 선언**
`th:with`를 사용하면 지역 변수를 선언해서 사용할 수 있다. 지역 변수는 선언한 태그 안에서만 사용할 수 있다.

`/resources/templates/basic/variable.html` 추가
```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

### 3. 기본 객체들
- HTTP 요청 파라미터 접근 : `param`
-> ex) `${param.paramData}`
- HTTP 세션 접근 : `session`
-> ex) `${session.sessionData}`
- 스프링 빈 접근 : `@`
-> ex) `${@helloBean.hello('Spring!')}`

```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Component;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@GetMapping("/basic-objects")
public String basicObjects(Model model, HttpServletRequest request,
                           HttpServletResponse response, HttpSession session) {
  session.setAttribute("sessionData", "Hello Session");
  model.addAttribute("request", request);
  model.addAttribute("response", response);
  model.addAttribute("servletContext", request.getServletContext());
  return "basic/basic-objects";
}

@Component("helloBean")
static class HelloBean {
  public String hello(String data) {
    return "Hello " + data; 
  }
}
```

`/resources/templates/basic/basic-objects.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>식 기본 객체 (Expression Basic Objects)</h1>
    <ul>
        <li>request = <span th:text="${request}"></span></li>
        <li>response = <span th:text="${response}"></span></li>
        <li>session = <span th:text="${session}"></span></li>
        <li>servletContext = <span th:text="${servletContext}"></span></li>
        <li>locale = <span th:text="${#locale}"></span></li>
    </ul>

    <h1>편의 객체</h1>
    <ul>
        <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
        <li>session = <span th:text="${session.sessionData}"></span></li>
        <li>spring bean = <span th:text="${@helloBean.hello('Spring!')"></span></li>
    </ul>
</body>

</html>
```

결과는 다음과 같다.
```text
식 기본 객체 (Expression Basic Objects)
request = org.apache.catalina.connector.RequestFacade@516fdd41
response = org.springframework.web.context.request.async.StandardServletAsyncWebRequest$LifecycleHttpServletResponse@1121a541
session = org.thymeleaf.context.WebEngineContext$SessionAttributeMap@7426fe7
servletContext = org.apache.catalina.core.ApplicationContextFacade@581cf618
locale = ko_KR

편의 객체
Request Parameter = HelloParam
session = Hello Session
spring bean = Hello spring!
```

`basic-objects.html`을 보면 한가지 의문점이 있다.
여태까지는 Model에 있는 key 이름을 가지고 데이터에 접근을 했는데 `basicObjects`에서 model의 key값 데이터를 보면
`sessionData`가 있다. 이러면 sessionData.~ 이런식으로 접근 해야 할것 같은데
html 파일을 보면 `{session.sessionData}`로 접근하고 있다. 왜 이런걸까?

`model.addAttribute("user", userA);`
- 동작 원리 : model은 Spring MVC가 제공하는 객체인데 여기에 담긴 데이터는
딱 해당 요청이 처리되고 뷰가 렌더링 되는 동안에만 유지된다.
- Thymeleaf의 편의성 : Thymeleaf는 Spring MVC와 찰떡궁합이여서, model에 담긴 속성들을
별도의 접두사 없이 `${user}`, `${users}`처럼 곧바로 변수 이름으로 접근할 수 있도록 기본 설계되어 있습니다. 

반면 `basicObjects`메서드에서는 `model`이 아니라 서블릿의 `HttpSession`에 데이터를 저장했다.
`session.setAttribute("sessionData", "Hello Session")`
- 동작 원리 : 세션은 하나의 요청이 끝나도 사라지지 않고, 웹 브라우저를 닫거나 만료될 때까지
로그인 정보처럼 유저의 상태를 계속 유지하는 저장소이다.
- 이름 충돌 방지 : 세션은 웹 애플리케이션 전반에서 공유되기 때문에, Spring MVC의 `model` 데이터와
격리되어 있어야 합니다. 만약 세션에 넣은 것도 그냥 `${sessionData}`로 쓰게 되면, 나중에 
`model.addAttribute("sessionData", ...)`를 했을 때 어떤 데이터가 진짜인지 컴퓨터도 헷갈리고
개발자도 헷갈릴수도 있다.
- Thymeleaf의 접근법 : 그래서 Thymeleaf는 세션 내의 데이터에 접근할 수 있도록 `session`이라는
기본 편의 객체를 미리 지정해 두었습니다. `session.sessionData` 표현식은 세션 내부에서
`sessionData`라는 Key를 찾아 꺼내오라는 명확한 경로를 지정해 주는 것이다. 

또 의문점이 하나 더 있는데 `${param.paramData}` 이건 뭘까?
- Thymeleaf의 접근법 : 요청 파라미터의 데이터에 접근할 수 있도록 'param'이라는 기본 편의 객체를 미리
지정해 두었습니다. `param.paramData` 표현식은 요청 파라미터에서 key값에 대응하는 value값을 갖고 오라는 것이다. 

### 4. 유틸리티 객체와 날짜
타임리프는 문자, 숫자, 날짜, URI등을 편리하게 다루는 다양한 유틸리티 객체들을 제공한다.

**타임리프 유틸리티 객체들**
- `#message` : 메시지, 국제화 처리
- `#uris` : URI 이스케이프 지원
- `#dates` : `java.util.Date` 서식 지원
- `#calendars` : `java.util.Calendar` 서식 지원
- `#temporals` : 자바 8 날짜 서식 지원
- `#numbers` : 숫자 서식 지원
- `#strings` : 문자 관련 편의 기능
- `#objects` : 객체 관련 기능 제공
- `#bools` : boolean 관련 기능 제공
- `#arrays` : 배열 관련 기능 제공
- `#lists`, `#sets`, `#maps` : 컬렉션 관련 기능 제공
- `#ids` : 아이디 처리 관련 기능 제공,

**참고**
이런 유틸리티 객체들은 대략 이런 것이 있다 알아두고, 필요할 때 찾아서 사용하면 된다.

**자바8 날짜**
타임리프에서 자바8 날짜인 `LocalDate`, `LocalDateTime`, `Instant`를 사용하려면 추가 라이브러리가 필요하다.
스프링 부트 타임리프를 사용하면 해당 라이브러리가 자동으로 추가되고 통합된다. 

자바8 날짜용 유틸리티 객체
-> `#temporals`

**사용 예시**
`<span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"></span>`

**BasicController 추가**
```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.time.LocalDateTime;

@GetMapping("/date")
public String date(Model model) {
  model.addAttribute("localDateTime", LocalDateTime.now());
  return "basic/date"; 
}
```

`/resources/templates/basic/date.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>LocalDateTime</h1>
    <ul>
        <li>default = <span th:text="${localDateTime}"></span></li>
        <li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"></span></li>
    </ul>

    <h1>LocalDateTime - Utils</h1>
    <ul>
        <li>${#temporals.day(localDateTime)} = <span th:text="${#temporals.day(localDateTime)}"></span></li>
        <li>${#temporals.month(localDateTime)} = <span th:text="${#temporals.month(localDateTime)}"></span></li>
        <li>${#temporals.monthName(localDateTime)} = <span th:text="${#temporals.monthName(localDateTime)}"></span></li>
        <li>${#temporals.monthNameShort(localDateTime)} = <span th:text="${#temporals.monthNameShort(localDateTime)}"></span></li>
        <li>${#temporals.year(localDateTime)} = <span th:text="${#temporals.year(localDateTime)}"></span></li>
        <li>${#temporals.dayOfWeek(localDateTime)} = <span th:text="${#temporals.dayOfWeek(localDateTime)}"></span></li>
        <li>${#temporals.dayOfWeekName(localDateTime)} = <span th:text="${#temporals.dayOfWeekName(localDateTime)}"></span></li>
        <li>${#temporals.dayOfWeekNameShort(localDateTime)} = <span th:text="${#temporals.dayOfWeekNameShort(localDateTime)}"></span></li>
        <li>${#temporals.hour(localDateTime)} = <span th:text="${#temporals.hour(localDateTime)}"></span></li>
        <li>${#temporals.minute(localDateTime)} = <span th:text="${#temporals.minute(localDateTime)}"></span></li>
        <li>${#temporals.second(localDateTime)} = <span th:text="${#temporals.second(localDateTime)}"></span></li>
        <li>${#temporals.nanosecond(localDateTime)} = <span th:text="${#temporals.nanosecond(localDateTime)}"></span></li>
    </ul>
</body>
</html>
```

### 5. URL 링크
타임리프에서 URL을 생성할 때는 `@{...}` 문법을 사용하면 된다.

**BasicController 추가**

```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@GetMapping("/link")
public String link(Model model) {
  model.addAttribute("param1", "data1");
  model.addAttribute("param2", "data2");
  return "basic/link"; 
}
```

`resources/templates/basic/link.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>URL 링크</h1>
<ul>
    <li><a th:href="@{/hello}">basic url</a></li>
    <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
    <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
    <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
</ul>
</body>
</html>
```

**단순한 URL**
- `@{/hello}` -> `/hello`

**쿼리 파라미터**
- `@{/hello(param1=${param1}, param2=${param2})}`
-> `/hello?param1=data1&param2=data2`
-> `()`에 있는 부분은 쿼리 파라미터로 처리된다.

**경로 변수**
- `@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}`
-> `/hello/data1/data2`
-> URL 경로상에 변수가 있으면 `()` 부분은 경로 변수로 처리된다.

**경로 변수 + 쿼리 파라미터**
- `@{/hello/{param1}(param1=${param1}, param2=${param2})}`
-> `/hello/data1?param2=data2`
-> 경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.

상대경로, 절대경로, 프로토콜 기준을 표현할 수도 있다.
- `/hello` : 절대 경로
- `hello` : 상대 경로 

### 6. 리터럴

리터럴은 소스 코드상에 고정된 값을 말하는 용어이다.
예를 들어서 다음 코드에서 `"Hello"`는 문자 리터럴, `10`,`20`는 숫자 리터럴이다.

타임리프는 다음과 같은 리터럴이 있다.
- 문자 : `'hello'`
- 숫자 : `10`
- 불린 : `true`, `false`
- null : `null`

타임리프에서 문자 리터럴은 항상 `'`(작음 따옴표)로 감싸야 한다.
`<span th:text="'hello'">`

그런데 문자를 항상 `'`로 감싸는 것은 너무 귀찮은 일이다. 공백 없이 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서
다음과 같이 작은 따옴표를 생략할 수 있다.

룰 : `A-Z`, `a-z`, `0-9`, `.`, `-`, `_`
ex) `<span th:text="hello">`

**오류**
`<span th:text="hello world!"></span>`
문자 리터럴은 원칙상 `'`로 감싸야 한다. 중간에 공백이 있어서 하나의 의미있는 토큰으로도 인식되지 않는다. 

**수정**
`<span th:text="'hello world!'"></span>`
이렇게 `'`로 감싸면 정상 동작한다.

**BasicController 추가**

```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@GetMapping("/literal")
public String literal(Model model) {
  model.addAttribute("data", "Spring!");
  return "basic/literal"; 
}
```

`/resources/templates/basic/literal.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>리터럴</h1>
    <!-- 주의! 다음 주석을 풀면 예외가 발생함 -->
    <!-- <li>"hello world!" = <span th:text="hello world!"></span> -->
    <li>'hello' + ' world!' = <span th:text="'hello' + ' world!'"></span></li>
    <li>'hello world!' = <span th:text="'hello world!'"></span></li>
    <li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
    <li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
</body>
</html>
```

**리터럴 대체**
`<span th:text="|hello ${data}|">`
마지막의 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.

`'` : 안에 있는 내용물은 무조건 텍스트로 받아들임.
`|` : 글자랑 변수는 알아서 구별해서 합쳐준다. (+ 기호 생략)

**연산**
타임리프 연산은 자바와 크게 다르지 않다. HTML 안에서 사용하기 때문에 HTML 엔티티를 사용하는 부분만 주의하자.

**BasicController 추가**

```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@GetMapping("/operation")
public String operation(Model model) {
  model.addAttribute("nullData", null);
  model.addAttribute("data", "Spring!");
  return "basic/operation"; 
}
```

`/resources/templates/basic/operation.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>산술 연산
        <ul>
            <li>10 + 2 = <span th:text="10 + 2"></span></li>
            <li>10 % 2 == 0 = <span th:text="10 % 2 == 0"></span></li>
        </ul>
    </li>
    
    <li>비교 연산
        <ul>
            <li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
            <li>1 gt 10 = <span th:text="1 gt 10"></span></li>
            <li>1 >= 10 = <span th:text="1 >= 10"></span></li>
            <li>1 ge 10 = <span th:text="1 ge 10"></span></li>
            <li>1 == 10 = <span th:text="1 == 10"></span></li>
            <li>1 != 10 = <span th:text="1 != 10"></span></li>
        </ul>
    </li>
    
    <li>조건식
        <ul>
            <li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)? '짝수':'홀수'"></span></li>
        </ul>
    </li>
    
    <li>Elvis 연산자
        <ul>
            <li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
        </ul>
    </li>
    
    <li>No-Operation
        <ul>
            <li>${data}?:_ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
            <li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
        </ul>
    </li>
</ul>
</body>
</html>
```

결과는 다음과 같다.
```text
산술 연산
10 + 2 = 12
10 % 2 == 0 = true
비교 연산
1 > 10 = false
1 gt 10 = false
1 >= 10 = false
1 ge 10 = false
1 == 10 = false
1 != 10 = true
조건식
(10 % 2 == 0) ? '짝수' : '홀수' = 짝수
Elvis 연산자
${data}?: '데이터가 없습니다.' = Spring!
${nullData}?: '데이터가 없습니다.' = 데이터가 없습니다.
No-Operation
${data}?: _ = Spring!
${nullData}?: _ = 데이터가 없습니다.
```

- 비교연산 : HTML 엔티리를 사용해야 하는 부분을 주의하자.
-> `>`(gt), `<`(lt), `>=`(ge), `<=`(le), `!`(not), `==`(eq), `!=`(neq, ne)
- 조건식 : 자바의 조건식과 유사하다.
- Elvis 연산자 : 조건식의 편의 버전(내용물이 있으면 그대로 나옴.) 
- No-Operaition : `_`인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다. 
이것을 잘 사용하면 HTML의 내용 그대로 활용할 수 있다. 마지막 예를 보면 데이터가 없습니다. 부분이 그대로 출력된다.