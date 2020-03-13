---
title: "[Spring]Spring boot 프로젝트에 MessageSource 적용 후기"
categories:
  - review
tags:
  - Spring
  - MessageSource
  - Internationalization
---
# Spring MessageSource
- 국제화(i18n)을 제공하는 인터페이스
- 메세지 설정 파일을 모아놓고 각 국가마다 로컬라이징을 함으로서 쉽게 각 지역에 맞춘 메세지를 제공할 수 있다. 
- 에러 메세지나 로그 등 다양한 텍스트에 사용 가능


# 설정 방법
## 구현체 선택
SpringBoot는 spring에 내장되어 있어, Dependency나 추가 패키지 설치가 필요하진 않다. 다만 MessageSource 자체로는 인터페이스여서, spring에서 제공하는 구현체를 사용하거나 클래스를 만들어 사용해야 한다. [Spring MessageSource](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/MessageSource.html)

Spring에서 제공하는 구현체는 크게 2개가 있는데, 나는 classpath 내의 파일이나 메세지 등 자원에 접근하는 [ResourceBundle](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html?is-external=true)과 String.format과 유사하게 파라미터를 받아 문자열을 만들어주는 [MessageFormat](https://docs.oracle.com/javase/8/docs/api/java/text/MessageFormat.html?is-external=true)(실제론 조금 달라서, document를 읽어보는 걸 추천한다.)을 이용한 [ResourceBundleMessageSource](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/support/ResourceBundleMessageSource.html)를 이용했다.

ResourceBundleMessageSource는 ResourceBundle의 한계를 지니고 있기에 이를 보완한 [ReloadableResourceBundleMessageSource](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/support/ReloadableResourceBundleMessageSource.html) 또한 고려할 수 있다. 두 클래스의 차이는 아래 포스팅을 참조하면 된다.

https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/support/ReloadableResourceBundleMessageSource.html

## Bean 추가
Spring boot이므로, XML 파일을 만질 필요가 없다.

후에 서술할 Locale 관련 편의를 위해, MessageUtil이라는 클래스를 만들었다.


```java class:"lineNo"
import java.util.Locale;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.support.MessageSourceAccessor;
import org.springframework.context.support.ResourceBundleMessageSource;
import org.springframework.stereotype.Component;

@Component
public class MessageUtil {
    @Autowired private MessageSourceAccessor messageSourceAccessor;

    @Bean public MessageSource messageSource() {
        ResourceBundleMessageSource source = new ResourceBundleMessageSource();
        source.setBasename("messages");
        source.setDefaultEncoding("UTF-8");
        return source;
    }
    
    @Bean public MessageSourceAccessor messageSourceAccessor() {
        return new MessageSourceAccessor(messageSource());
    }

    public String getMessage(String code) {
        return messageSourceAccessor.getMessage(code, Locale.getDefault());
    }
    
    public String getMessage(String code, String[] args) {
        return messageSourceAccessor.getMessage(code, args, Locale.getDefault());
    }

    public String getMessage(String code, Locale locale) {
        return messageSourceAccessor.getMessage(code, locale);
    }

    public String getMessage(String code, String[] args, Locale locale) {
        return messageSourceAccessor.getMessage(code, args, locale);
    }
}
```
위 코드를 WebMvcConfig 파일이나 다른 bean 클래스 내부에 넣어주면, Spring 실행 시 MessageSource와 MessageSourceAccessor가 bean 객체로 생성되고, `@Autowired`에 의해 `private MessageSourceAccessor messageSourceAccessor` 객체가 주입된다.

그 아래로는 getMessage함수를 override했다. Locale 유무와 관계없는 동작이나 args를 string으로만 받기 위함이어서, 추가 기능이 필요하지 않다면 MessageSourceAccessor를 사용하면 된다.

`처음 코드를 작성할 땐 확장성을 고려해 객체를 가지게 했는데, 이렇게 보니 MessageSourceAccessor를 상속해서 쓰는 게 더 나을 것 같다.`

```java
        ResourceBundleMessageSource source = new ResourceBundleMessageSource();
        source.setBasename("messages");
        source.setDefaultEncoding("UTF-8");
        return source;
```
이 부분에서는 MessageSource의 설정을 정할 수 있다. `application.properties`에도 해당 옵션이 있으나, 이 경우엔 적용되지 않아서 꽤 애를 먹었었다.

개인적으로는, 설정 파일을 읽고 난 뒤 Bean 객체를 생성해서 객체가 덮어씌워지는게 아닌가 싶다.
그렇다고 그냥 MessageSourceAccessor를 autowired하면 에러가 난다.

어찌됐건, 위 코드의 2번째 줄은 classpath에 대한 messages 설정 파일들의 경로를 나타낸다. classpath에 대해선 아래 링크를 참조하면 좋다. 

[Spring의 classpath:의 경로 위치](https://developer-joe.tistory.com/225)

spring boot classpath에 해당하는 디렉토리에 `messages_ko.properties`, `message_en.properties` 등의 파일을 만들면 된다. 각각 한글, 영어 메세지를 담을 수 있고, 메세지가 없는 지역에 주는 기본 메세지는 `messages.properties` 파일에 놓으면 된다.

이 외의 언어도 `message_{lang.}.properties ` 형식의 파일명으로 만들면 된다.

## Message 설정 파일 정의
다음은 프로젝트에 적용했던 messages_ko.properties 파일의 일부이다.
```properties
# Input validation
input.validation.null=입력값 {0}이/가 비었거나 null입니다.
input.validation.min=입력값 {0}은/는 최소 {1}자 이상입니다.
input.validation.max=입력값 {0}은/는 최대 {1}자 이하입니다.
```
기본 locale에 대해 설정한 messages.properties에는 이렇게 저장되어 있다.
```properties
# Input validation
input.validation.null=Parameter {0} is empty or null
input.validation.min=Length of {0} is at least {1} characters
input.validation.max=Length of {0} is at most {1} characters
```
기본적으로 (key) = (message) 형태의 포맷을 가진다.

이렇게 설정해 둔 메세지들은

```java
@Autowired MessageUtil messageUtil;
messageUtil.getMessage("input.validation.max", new String[] {"nickname", "3"});
```
등의 방법으로 호출할 수 있고, 이 때 이 `nickname`, `3`은 순서대로 {0}, {1}에 대입되어, 실제 메세지는 `입력값 nickname은/는 최소 3자 이상입니다.` 혹은 
`Length of nickname is at least 3 characters`가 만들어진다.

## Locale 설정
국가별로 다른 언어로 응답을 주는 건 좋으나, 어떤 사용자가 어떤 지역에서 요청하는지 파악하는 건 또 다른 문제이다.

또한, 같은 고객이라도 브라우저 언어나 지역 설정을 바꿨을 때 서비스 메세지도 달라져야 한다.

다양한 방법이 제시될 수 있으나, 내가 사용한 방법은

1. 처음 로그인 시 cookie에 지역 정보를 담는다.
2. 이후, 매 요청마다 locale이 변화했는지 체크한다.

간단한 아이디어고, 구현 또한 간단하다.
```java
@Bean public LocaleChangeInterceptor localeChangeInterceptor() {
    LocaleChangeInterceptor localeChangeInterceptor = new LocaleChangeInterceptor();
    localeChangeInterceptor.setParamName("lang");

    return localeChangeInterceptor;
}

@Bean public LocaleResolver cookieLocaleResolver() {
    // 쿠키 기준(세션이 끊겨도 브라우져에 설정된 쿠키 기준으로)
    CookieLocaleResolver localeResolver = new CookieLocaleResolver();
    localeResolver.setDefaultLocale(Locale.KOREA);

    return localeResolver;
}
```

위 코드를 WebMvcConfig 파일이나 적당한 곳에서 빈 객체로 만든 뒤, LocaleChangeInterceptor를 추가하면 된다.

이렇게 추가된 LocaleChangeInterceptor는 내부 LocaleResolver 빈 객체를 자동으로 주입받아 사용한다.

LocaleResolver는 [AcceptHeaderLocaleResolver](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/i18n/AcceptHeaderLocaleResolver.html), [CookieLocaleResolver](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/i18n/CookieLocaleResolver.html), [FixedLocaleResolver](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/i18n/FixedLocaleResolver.html), [SessionLocaleResolver](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/i18n/SessionLocaleResolver.html) 등의 구현체가 있으며, 각각 헤더, 쿠키, 고정, 세션에서 locale을 받아온다.

## 결론
처음엔 되게 가벼워 보였는데, 설정할게 이것저것 많고 Locale 관련 객체도 많아서 적용이 오래 걸렸다.

그렇게 적용하고 나니 기왕이면 여러 언어로 적용하고 싶었는데, 팀 내에 영어 말고 다른 외국어 능력자가 없어서 포기했다. ㅜㅜ


