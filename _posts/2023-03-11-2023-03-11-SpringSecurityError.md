---
title: SpringSecurity - jwt인증 실패했을때 커스텀 에러 넘기기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-03-11 00:02:00 +0800
categories: [Java,Spring]
tags: [SpringSecurity]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## ☑️ 개요

해당 기능을 작성하는 이유가 있다.

기존 프로젝트 코드에서는 인증절차 실패할 경우 무조건 **401 Unauthorized** Status를 반환하고 있었다.

그 코드는 다음과 같으며,

```java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf()
                .disable()
                .authorizeHttpRequests((auth) -> auth
                        .requestMatchers("/swagger-ui/**", "/v3/**").permitAll()
                        .requestMatchers("/api/users/signup", "/api/users/verification").permitAll()
                        .anyRequest().authenticated())
                .requestCache().disable()
                .securityContext().disable()
                .sessionManagement().disable()
                .headers().frameOptions().disable()
                .and()
                .formLogin()
                .disable()
                //밑의 이 부분
                .exceptionHandling().authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED))
                .and()
                .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
```

postman으로 요청을 보내서 인증에 실패할 경우 밑의 사진처럼 뜬다.

<img width="848" alt="image" src="https://user-images.githubusercontent.com/30401054/224479101-558f0cea-29ae-47fc-bd2b-ee3731e24ae7.png">

이것은 문제는 Response Status만 알고, 어디서 어떻게 에러가 터졌는지 몰라서 같이 협업하는 안드로이드 개발자가 매우 답답함을 느꼈다.

처음에는 401 에러가 뜰때마다 토큰의 문제라고 생각 안 하고 로그를 확인하였으며 매우 많은 시간을 보냈다.

일단 다른 일들이 급하여 이 부분 수정을 미루고 있었는데, 시간이 생겨서 우선순위 큐에 첫 번째에 있는 놈처럼 바로 처리하기로 했다.

## ☑️ 수정

위 코드에서 

```java
.exceptionHandling().authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED))
```

이 부분을 커스텀하면 되는데, `HttpStatusEntryPoint`는 `AuthenticationEntryPoint`라는 인터페이스의 구현체이다.

즉, `AuthenticationEntryPoint`를 구현한 클래스를 갈아끼우면 된다.

나는 `CustomAuthenticationEntryPoint`라는 클래스를 만들어서 위의 인터페이스를 구현할 것이다.



```java
@Component
@Slf4j
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {

    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        //...
    }
}
```

`commence`라는 메소드를 구현하게 되어있고, 이 메소드를 통해 인증에 실패했을때의 처리를 진행할 수 있다.

내가 반환할 에러 객체는 다음과 같이 구성되어있다.

```java
public class ErrorResponse {

    private final LocalDateTime timestamp = LocalDateTime.now();
    private final int status;
    private final String error;
    private final String code;
    private final String message;

    public ErrorResponse(Error error){
        this.status = error.getStatus().value();
        this.error = error.getStatus().name();
        this.code = error.name();
        this.message = error.getMessage();
    }
}
```

즉, 여기에 있는 값을 인증에 실패했을때 내려줄 것이며 이는 대략적으로 

```text
{
    "timestamp": "2023-03-11T20:23:42.663273",
    "status": 409,
    "error": "CONFLICT",
    "code": "USER_ALREADY_EXITS",
    "message": "user already exits"
}
```

이런것처럼 구성을 가질 것이다. 에러발생시간, Http 상태코드, Http 상태메시지, 커스텀한 에러 코드, 커스텀한 에러 내용 이라는 값을 가진다.

이제 이를 반환해보겠다.

## ☑️ CustomAuthenticationEntryPoint 작성

`response`의 값들을 수정하여 내려줄때의 상태를 커스텀해줄 수 있다.

```java
// CustomAuthenticationEntryPoint.java
@Override
public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        log.error("UnAuthentication!!! message : " + authException.getMessage());

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        //..
}
```

여기까지는 쉽다. 먼저, 배포서버에서 로그를 볼 수 있게 로그를 찍고 우리는 `Json`형태의 값을 내려주며 `401` 상태코드를 내려줄거라는 코드이다.

- 참고로 `setStatus`안에 들어오는 상태값들은 `응답본문`을 가지는 어떠한 코드도 들어올 수 있다.

이제 `ErrorResponse`의 객체를 응답본문에 담아서 넘겨줘야한다.

```java
@Override
public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
    log.error("UnAuthentication!!! message : " + authException.getMessage());

    response.setContentType(MediaType.APPLICATION_JSON_VALUE);
    response.setStatus(HttpStatus.UNAUTHORIZED.value());

    // 추가된 부분
    ErrorResponse errorResponse = new ErrorResponse(Error.UNAUTHORIZED_USER);
    String responseBody = objectMapper.writeValueAsString(errorResponse);
    try (OutputStream os = response.getOutputStream()) {
        os.write(responseBody.getBytes(StandardCharsets.UTF_8));
        os.flush();
    }
}

@Getter
public enum Error {

    UNAUTHORIZED_USER("Unauthorized user. Please check your token.", HttpStatus.UNAUTHORIZED),
    ...
}
```

내가 작성한 커스텀 에러 객체를 생성하고 이를 직렬화하는 코드이다. 

> 제가 커스텀한 에러 객체들이 필요하다면 요청하시면 드리겠습니다.
{: .prompt-tip}

아무튼 위의 내용을 직렬화를 하여, 응답본문에 작성한다.

`objectMapper`는 어딨냐? 라고 말할 수 있는데, 이는 내가 따로 싱글톤으로 구현했다.

싱글톤으로 만들어서 딱 한번만 해당 객체를 생성하도록 하여 객체 생성 리소스를 줄이기 위함이다.

그래서 싱글톤 + 전체적인 코드는 다음과 같다.

```java
@Component
@Slf4j
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {

    private final ObjectMapper objectMapper;

    public CustomAuthenticationEntryPoint(){
        this.objectMapper = new ObjectMapper()
                //timestamp 직렬화를 위해 필요한 코드
                .registerModule(new JavaTimeModule())
                .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
    }

    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        log.error("UnAuthentication!!! message : " + authException.getMessage());

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.UNAUTHORIZED.value());

        ErrorResponse errorResponse = new ErrorResponse(Error.UNAUTHORIZED_USER);
        String responseBody = objectMapper.writeValueAsString(errorResponse);
        try (OutputStream os = response.getOutputStream()) {
            os.write(responseBody.getBytes(StandardCharsets.UTF_8));
            os.flush();
        }
    }
}
```


여기서 절대 놓치지 말아야할 한 가지가 있는데,

```java
this.objectMapper = new ObjectMapper()
                //timestamp 직렬화를 위해 필요한 코드
                .registerModule(new JavaTimeModule())
                .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
```

이 코드이다.

기존 `ObjectMapper`는 `java.util.Date`와 `java.util.Calendar`를 통해 시간에 대해 직렬화를 수행하는데, Java8부터 나온 `LocalDate` 시리즈들을 인식하지 못한다.

그래서 `registerModule(new JavaTimeModule())` 코드를 통해 모듈을 추가 등록하면서 `LocalDate` 시리즈들도 직렬화 할 수 있게 한다.

그러면 ObjectMapper가 `JavaTimeModule` 모듈에 있는 직렬화 방식을 사용할까? 기존 제공하던 `java.util.Date`나 `java.utilCalendar`를 통해 직렬화를 할까?

`.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)`를 통해 기본적으로 제공하는 직렬화 방식을 막는다. 이 코드를 넣지 않는다면 예기치 못한 방식으로 직렬화 할 것이다.

- `disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)`를 추가하지 않았을 때

<img width="764" alt="image" src="https://user-images.githubusercontent.com/30401054/224483040-8365c912-3cdb-4d14-a12e-4b2ed05361b5.png">

- `disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)`를 추가했을 때

<img width="777" alt="image" src="https://user-images.githubusercontent.com/30401054/224483098-0043dfd2-202e-40fb-a5b8-fc713f34a7bd.png">


## ☑️ 등록

사실 저기까지 구현한다해서 제대로 동작하지 않는다.

왜냐하면 커스텀한 이 객체를 등록하지 않았기 때문이다.

이제 다시 `Config`관련 코드로 가서 해당 객체를 끼워넣어주자.

```java
@Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf()
                ...
                .exceptionHandling().authenticationEntryPoint(customAuthenticationEntryPoint)
                ...

        return http.build();
    }
```

자 이제 이러면 성공적으로 작동한다.

여기서 인터페이스 장점도 볼 수 있다. 구현체만 갈아끼우면 얼마든지 여러 코드를 커스텀할 수 있다는 것이다.

