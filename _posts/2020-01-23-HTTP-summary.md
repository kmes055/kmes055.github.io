---
title: "[교육후기]HTTP의 이해"
categories:
  - review
tags:
  - 교육
  - Linux
---
# HTTP의 이해

## HTTP
* HypterText Transfer Protocol. OSI level 7 Application layer에 해당
* Transfer Protocol: Header + Payload 로 data를 주고받는 프로토콜.
* +이런저런 이론 이야기

## WWW
* World Wide Web
* 4개의 구성 요소
    * HTML
    * HTTP 
    * Web Browser: IE, Chrome 등
    * Httpd: 웹서버의 초기 버전
* 정리하면, HTML를 HTTP로 요청하면 HTML이 HTTP로 응답이 오는 구조임

## URL
* Uniform Resource Locator. 네트워크 상에서 자원이 어디 있는지를 알려주는 규약
* `scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]` 형식

## HyperText
* 전통적인 Text(신문) + Hyper. 즉시 다른 레퍼런스에 접근 가능하며 표, 이미지 외 기타 표현 가능한 컨텐츠를 포함한 텍스트
* HTML은 Hypertext를 만드는 마크업 언어

## Response code
* API에 등에서는 201 Created, 202 Accepted, 203 Accepted, 204 No Content 등도 사용함
* 301 Moved Permenantly, 302 Found 는 영구 이동, 임시 이동? 차이. 점검 등의 사유로 redirection할때는 302가 좋음
* 401 UnAutorized 403 Forbidden 405 Method Not Allowed 등도 있음

## Custom headers
* 사용자 임의 지정 헤더: RFC 6648에서 폐기
    * X- prefix
    * X-forward-host 등
    * 표준은 아닌데, 사실상 표준이라고 봐도 될 정도로 널리 쓰임

## Cookie / Session
### HTTP 특징
* Connectionless, Stateless
* 이 2개의 특성으로 인해 사용자를 구분할 수가 없음

### Cookie
* 서버가 클라이언트에 붙여둔 일종의 스티커
* 서버가 클라이언트에 Set-Cookie: 하고 나면, 이후 받은 쿠키를 받아서 보내면 사용자를 분류할 수 있음
### Cookie 종류
* 세션 쿠키
    * 브라우저를 사용하는 동안만 유효
* 영구 쿠키
    * 브라우저가 종료돼도 유지되는 쿠키
    * Expires, Max-Age 등으로 관리
### 쿠키 사용 사례
* 세션 관리
* 개인화
* 사용자 추적