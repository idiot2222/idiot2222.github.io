---
title: Query Plan Cache
date: 2022-11-29 23:00:00 +0900
categories: [Framework, JPA]
tags: [jpa, hibernate, orm, query plan cache, parameter binding]
---

<br/>
<br/>
<br/>
<br/>

### 참고

- [Hibernate Query Cache Plan - Baeldung](https://www.baeldung.com/hibernate-query-plan-cache)

<br/>
<br/>

# Query Plan Cache

- Hibernate가 SQL을 작성할 수 있도록, JPQL과 Criteria의 쿼리들은 실행 전에 모두 Abstract Syntax Tree로 파싱된다.
  - [Abstract Syntax Tree란?](https://ko.wikipedia.org/wiki/추상_구문_트리)
  - 쿼리 컴파일에는 시간이 걸리므로, Hibernate는 Query Plan Cache로 성능을 보완한다.
- 네이티브 쿼리의 경우,
  - Hibernate는 named parameter와 쿼리 반환 타입에 대한 정보를 추출하여 ParameterMetadata에 저장한다.
- Hibernate는 모든 실행에 대해서 먼저 Query Plan Cache를 확인하고
  - 사용 가능한 실행 계획이 없는 경우에 새 계획을 생성하고 나중에 사용할 수 있도록 실행 계획을 Query Plan Cache에 저장해둔다.
  - 사용 가능한 실행 계획이 있는 경우에 그것을 사용한다.

<br/>
<br/>
<br/>
<br/>

# 설정

- Query Plan Cache는 다음과 같은 속성을 줄 수 있다.
  - hibernate.query.plan_cache_max_size
    - Query Plan Cache의 최대 크기 설정
    - 디폴트는 2048
  - hibernate.query.plan_parameter_metadata_max_size
    - Query Plan Cache의 ParameterMetadata 인스턴스의 최대 숫자 설정
    - 디폴트는 128

- Query Plan Cache의 최대 크기보다 더 많은 수의 쿼리를 실행하게 되면
  - Hibernate가 쿼리를 컴파일 해야하기 때문에 느려지게 된다.

<br/>
<br/>
<br/>
<br/>
