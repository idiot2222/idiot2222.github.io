---
title: docker compose
date: 2022-10-21 14:00:00 +0900
categories: [Devops, Docker]
tags: [devops, docker, docker compose]
---

<br/>
<br/>
<br/>
<br/>

# docker-compose

- yaml 파일로 설정 파일을 작성한다.
  - 명령어만 사용하는 것보다 훨씬 편리하다.
  - 오타가 나도 수정이 간편하다.
- 다중 컨테이너 앱을 구성할 수 있다.
  - 게시판 기능을 하는 wordpress와
  - 데이터베이스인 mysql 컨테이너를 한 번에 같이 띄울 수 있다.

```yaml

version: '2'
services:
  db:
    image: mariadb:10.9
    volumes:
      - ./mysql:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    image: wordpress:latest
    volumes:
      - ./wp:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress

```

- version
  - docker-compose의 버전을 나타낸다.
- services
  - 하위에 띄울 컨테이너들의 정의를 넣는다

<br/>
<br/>

- docker-compose up
  - docker-compose.yml 파일을 작성하고
  - docker-compose up 명령어를 실행하면 설정에 따라 컨테이너들을 띄워준다.

- docker-compose down
  - 설정 파일에 따라 한 번에 띄운 컨테이너들을 한 번에 종료시킨다.
  - 물론 개별적으로 끌 수도 있다.

<br/>
<br/>
<br/>
<br/>
