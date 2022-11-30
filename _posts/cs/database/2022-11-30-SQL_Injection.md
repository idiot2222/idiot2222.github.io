---
title: SQL Injection
date: 2022-11-30 23:40:00 +0900
categories: [CS, Database]
tags: [cs, database, sql injection]
---

<br/>
<br/>
<br/>
<br/>

![img-description](asstes/img/posting/db/sql_injection-01.png)
_출처: https://xkcd.com/327/_

<br/>
<br/>

# SQL Injection

- 클라이언트의 입력값에 SQL 코드를 넣어 디비를 조작하는 공격 기법이다.
- 이해를 위한 간단한 예시를 들어보면,
  - 팀원들의 나이를 정리해둔 페이지가 있다고 한다.
  - "팀장님은 " + age + "세 입니다."
  - 여기서 악의적으로 age를 "말 많은 꼰대 머리는 탈모 증"이라고 입력했다.
  - 결과가 "팀장님은 말 많은 꼰대 머리는 탈모 증세 입니다."가 되어버렸다.
  - 악의적인 입력으로 남들이 보면 오해하기 쉬운 결과가 나온 것이다.
- 이 예시에서는 작은 해프닝으로 끝나겠지만, SQL에 악의적인 코드를 주입한다면 데이터가 난장판이 될 것이다.
  - 예시에서는 숫자 타입에 문자열을 넣었지만, 실제 db에서는 문자열 타입에 대한 공격이라고 생각하면 된다.

<br/>
<br/>
<br/>
<br/>

# 예방 방법

- Parameter Binding
  - 가장 간단한 방법이다.
  - 클라이언트로부터 받는 입력값을 직접 SQL에 넣는 것이 아닌 파라미터 바인딩으로 넣는 것이다.

<br/>

```java

String bookTitle = "book'; DELETE FROM Book WHERE id > 0--"

preparedStatement = connection.preparedStatement("SELECT * FROM Book WHERE title = '" + bookTitle + "'");

```

<br/>

- 데이터 바인딩 없이 입력값을 바로 sql에 넣은 형태이다.
  - bookTitle은 누군가 악의적으로 작성한 코드이다.
  - 뒤에 방해가 될 만한 것은 --으로 주석 처리 해버렸다.
- 이 경우 실질적으로 실행되는 쿼리는 두 개이다.
  - SELECT * FROM Book WHERE title = 'book';
  - DELETE FROM Book WHERE id > 0
- 아무 생각 없이 짠 조회 코드가 순식간에 테이블의 모든 데이터를 삭제해버렸다.

<br/>

```java

String bookTitle = "book'; DELETE FROM Book WHERE id > 0--"

preparedStatement = connection.preparedStatement("SELECT * FROM Book WHERE title = ?");

preparedStatement.setString(1, bookTitle);

```

- 위는 jdbc에서 파라미터 바인딩하는 코드이다.
  - bookTitle 역시 위와 같이 조회는 대충 해버리고 테이블의 데이터를 삭제하는게 진짜 목적이다.
- PreparedStatement의 setString()에서 예방한다.
  - 멀티 쿼리를 제한한다.
    - ;로 끝내고 다른 쿼리를 집어넣는 행위를 막는 방법이다.
    - 설정을 바꿔 멀티 쿼리를 허용할 수 있다.
  - 악의적인 공격 시도를 위한 문자를 지워버린다.
    - 문자열을 나타내는 ' 작은 따옴표를 \'로 전부 바꿔버려 쿼리 수행을 막는다.

<br/>
<br/>
<br/>
<br/>
