---
title: Spring Security 오류 FindBy~()가 DB에서 객체를 못 찾을 때
author: 강민석
date: 2021-10-02 00:34:50 +0800
categories: [Java,Spring_CS]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

Spring 스터디를 하면서 Spring Securtiy를 통해 로그인, 로그아웃 구현을 하였다.

처음에는 내 식대로 커스텀마이징 하다가 많인 오류가 생겼고, 그냥 따라치기로 했다.

도대체 뭐가 문제였을까?

일단 알아야할 것이 있었다.

```java
    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception{
        httpSecurity.
                authorizeRequests()
                .antMatchers("/login","/signup","/user").permitAll() //누구나 접근 허용
                .antMatchers("/").hasRole("USER") //USER, ADMIN만 접근 가능.
                .antMatchers("/admin").hasRole("ADMIN") //Admin만 접근 가능.
                .anyRequest().authenticated() //나머지 요청들은 권한의 종류에 상관없이 권한이 있어야 접근 가능.
            .and()
                .formLogin()
                .loginPage("/login") // 로그인 페이지 설정.
                .defaultSuccessUrl("/") // 로그인 성공시 라다이렉션 페이지
            .and()
                .logout()
                .logoutSuccessUrl("/login") // 로그아웃 성공시 리다이렉션 페이지
                .invalidateHttpSession(true) // 세션 날리기
        ;
    }
```

위의 코드에서 원래는 loginProcess()가 있어야하는데, 없어도 된단다. Security에서 loginProcess()가 없을 때 UserDetails를 상속받은 서비스에서 필수 구현 메소드인 loadUserByUserName()를 찾아 해당 로직을 수행한다.

Service 코드

```java
@Override
    public UserInfo loadUserByUsername(String email) throws UsernameNotFoundException{
        Optional<UserInfo> findEmail = userRepository.findByEmail(email);
        System.out.println(findEmail + " !@!!#@!#");
        return userRepository.findByEmail(email).orElseThrow(() -> new UsernameNotFoundException(email));
    }
```

해당 로직을 수행할 때 Repositry에서의 코드는 다음과 같다.

```java
public interface UserRepository extends JpaRepository<UserInfo,Long> {
    Optional<UserInfo> findByEmail(String email);
```

근데 DB에는 잘 저장되어 있는데 계속 Optional값을 뱉어내는 것이다.

이유가 뭘까 하다가 임시방편으로 @Query를 이용해서 해결했다.

```java
public interface UserRepository extends
JpaRepository<UserInfo,Long>{
    @Query("select u from UserInfo u where u.email like %?1")
    Optional<UserInfo> findByEmail(String email);

}
```

하지만 Spring에서 직접 쿼리를 내보내는 것은 권장하지 않기에 근본적인 원인을 해결해야 했다.

위의 방식으로 로그인을 했더니, 일반 User계정 로그인은 잘 되고 Admin은 객체를 못 찾겠다고 또 오류를 뱉길래 그 로그를 그대로 구글링했다.

참고 : <https://kimji0139.tistory.com/34>

위 글에 따르면 view에서 name태그를 username으로 바꿔야한다고 하더라.

그래서 네임태그를 바꾸고 @Query문을 지우니 해결..

아마 SpringSecuriy 내부에서 로직을 돌릴 때 thymeleaf와 어떤 관계가 있나보다.

-----

(정확하지 않음.) themleaf에서

```html
 <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}" />
 ```

라는 문법을 통해 Form을 누르면(뭐 로그인하는 버튼) hidden 속성으로 CSRF토큰이라는 세션을 날림.  
->  
Spring security Config에서 설정한

```java
protected void configure(HttpSecurity httpSecurity) throws Exception
```

이 함수가 작동함.
함수 내부에 있는

```java
loginProcess("URL")
```

를 작성했으면 Controller에 있는 "URL"을 로직을 수행 작성 안 했으면 그냥 Security 내부에서 Login 처리를 함. Login 처리를 위해 필수 구현함수인

```java
public UserInfo loadUserByUsername(String email) throws UsernameNotFoundException
```

loadUser~함수를 실행함.위 함수를 통해 Repo에 접근함(repo는 JPArepository를 Extends한 것) 그 속에

```java
Optional<UserInfo> findByEmail(String email);
```

JPArepositroy를 Extends하면 저렇게 함수를 작성하기만해도 알아서 JPQL을 만듦(수정하고 싶으면 내가 했던 @Query를 사용하면 됨)

근데 그 내부에서 값을 view에서 name으로 지정한 username을 기준으로 찾나봄?

csrf관련 : <https://reiphiel.tistory.com/entry/spring-security-csrf>
