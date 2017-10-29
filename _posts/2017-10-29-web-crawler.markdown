---
layout: post
title:  "Web Crawler - nodejs"
date:   2017-10-29 23:47:0 +0900
tags: web crawler scraping node nodejs
---
# Precodition
* * *

앞서 Elasticsearch 를 공부하면서, 무언가 활용하기 위해 추가적인 공부가 필요하다 생각이 들었다. 좀 더 배운 것을 활용하기 위해, wikipedia 문서들을 한번 elasticsearch 를 사용한 검색 엔진에서 사용해보자 라는 생각을 해 봤다. wiki 문서들을 indexing 하기 위해선 wiki 문서들을 크롤링하는 과정이 필요하기에 추가적인 공부를 시작하게 되었다.

(wikipedia 페이지 크롤링을 하면서 여러 문서를 다운로드 하면, wikipedia 서버에 부담을 주게 되어있다... 아마도,,, 문제가 되지않을까? 설마 법적인 문제가 있으려나? 알아보고 마지막에 크롤링을 할지 안할지 정하겠다.)

# What is the Web Crawler
* * *

A *Web crawler*, sometimes called a *spider*, is an Internet bot that systematically browses the WWW (World Wide Web), typically for the purpose of Web indexing [1].

"웹 크롤러는 자동화된 방법으로 WWW를 탐색하는 컴퓨터 프로그램이다."

검색 엔진과 같은 여러 사이트에서는 데이터의 최신 상태 유지를 위해 웹 크롤링을 한다.

웹 크롤러는 페이지의 복사본을 생성하고, 검색 엔진은 이를 빠른 검색을 위해 인뎅싱한다.

또한 크롤러는 hyperlinks 와 HTML code를 검증하는 데에 사용한다.

하나 더, web scraping (web sites 로 부터 data 를 추출하는 작업) 용도로 사용되기도 한다.

# Open source Crawlers
* * *

Wiki 문서를 보면 혹은 google 에 검색만 해봐도 여러 open source crawler 를 찾을 수 있다.

하지만 우리의 목적은 crawler 가 뭔지 어떻게 동작하는지 공부하기 위함이다. 그렇기 때문에 Open source 들 사용법에 대해 가이드는 나중 posting 으로 떠넘기겠다.

# Languages
* * *

web crawler 를 만들기 위해서는 컴퓨터 언어 하나를 선택해야 한다. 우리가 알고 있는 언어들 Java, Python, javascript, C++, Go 등 어느 언어이를 사용하든지 상관없다.

posting 을 간단히 하기 위해서는 위에 알고있는 모든 언어로 구현을 해보는 것이 아니라, 우리의 *목적인 web crawler 공부하기* 에 집중하기 위해서 진입장벽이 낮은 javascript 진영의 nodejs 를 사용하도록 하겠다 (본인이 posting 을 하기위해 참고한 책이 nodejs 를 사용기 때문이기도 하다).

## nodejs
* * *

nodejs 설치를 위한 Guide 는 [nodesource](https://github.com/nodesource/distributions) 에 아주아주 자세히 나와 있다.

여기서는 내가 개발용으로 사용하는 Ubuntu 환경에서 개발하기 때문에, 그 기준으로 간단한 설명만 하겠다.

nodejs 설치
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

npm
- nodejs 를 사용하는 이유중 하나이기도 하다.
- npm 은 nodejs 의 모듈을 관리하는 패키지 관리자이다.

npm 을 이용하여 아래와 같이 모듈을 설치하는데,
```
npm install request
```
이 방법은 명령은 실행한 directory 에 모듈이 다운되도록 한다. 그렇기 때문에 다른 directory 에서는 설치한 모듈을 사용할 수 없다.

이를 방지하기 위해,
```
npm install -g (module name)
```
을 사용하여 글로벌로 모듈을 관리한다.

npm permissions 문제가 발생한다면, [fixing npm permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions)를 참고하자.




## Reference
- [Web crawler in wikipedia](https://en.wikipedia.org/wiki/Web_crawler)
