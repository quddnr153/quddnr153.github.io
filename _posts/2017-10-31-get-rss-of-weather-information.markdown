---
layout: post
title:  "Get RSS of weather information"
date:   2017-10-31 23:10:0 +0900
tags: web weather rss xml nodejs
---
# 기상청 기상예보
* * *
우리나라 날씨정보를 수집하는 곳이라 하면 바로 기상청이 있다. [기상청](http://www.kma.go.kr/)

날씨 정보에 관한 모든 데이터를 수집하는 곳이니 만큼 여러 가지 정보를 기상청에서 찾아볼 수 있다.

우리가 사용할 정보들은 [여기](http://www.kma.go.kr/weather/lifenindustry/sevice_rss.jsp) 에서 찾을 수 있다 (홈 > 날씨 > 생활과 산업 > 서비스 > 인터넷 > RSS).

[여기](http://www.kma.go.kr/weather/lifenindustry/sevice_rss.jsp)를 보면 여러 RSS 주소를 받을 수 있다.

RSS 데이터에 대한 정의는 RSS 페이지 중간에 ***정의 문서 링크***가 있으니 확인하기 바란다.

* * *
# CURL 로 데이터 확인하기
* * *
각 RSS 에 어떤 데이터가 들어있는지 간략하게 확인하기 위한 방법으로 curl 을 사용해보려 한다.

아래와 같이 명령어를 입력하면, (테스트 환경은 ubuntu 14.04)
```
curl http://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=108
```

Result
```
<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

...


<data>
					<mode>A02</mode>
					<tmEf>2017-11-05 00:00</tmEf>
					<wf>맑음</wf>
					<tmn>6</tmn>
					<tmx>18</tmx>
					<reliability>보통</reliability>
				</data>

				
				<data>
					<mode>A02</mode>
					<tmEf>2017-11-05 12:00</tmEf>
					<wf>맑음</wf>
					<tmn>6</tmn>
					<tmx>18</tmx>
					<reliability>보통</reliability>
				</data>

...

</rss>
```

xml 형식의 RSS 데이터를 볼 수 있다.

* * *
# 활용
* * *
각자 원하는 언어로 RSS 데이터를 파싱하여 날씨 제공 서비스를 쉽게 제공할 수 있겠다.

여기서 예시로 nodejs 와, xml2js 를 이용하여 간단하게 콘솔로 찍어보는 예시를 들어보겠다.

***weather.js***
```
// 기상청 기상예보 RSS
var RSS = 'http://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=108';

// Load module
var parseString = require('xml2js').parseString;
var request = require('request');

// Download RSS
request(RSS, function(err, response, body) {
  if (!err && response.statusCode == 200) {
    analyzeRss(body);
  }
});

// Analyze RSS
function analyzeRss(xml) {
  // Transform xml to js object
  parseString(xml, function(err, obj){
    if (err) {
      console.log(err);
      return;
    }

    // Print weather information
    var rssBody = obj.rss.channel[0].item[0].description[0].body[0];

    for (var cityIndex in obj.rss.channel[0].item[0].description[0].body[0].location) {
      var city = rssBody.location[cityIndex].city;
      for (var dataIndex in obj.rss.channel[0].item[0].description[0].body[0].location[cityIndex].data) {
        var datum = rssBody.location[cityIndex].data[dataIndex];
        console.log(city + ' ' + datum.tmEf + ' ' + datum.wf + ' ' + datum.tmn + '~' + datum.tmx);
      }
    }
  });
}

```

Result
```
서울 2017-11-03 00:00 구름많고 비 10~14
서울 2017-11-03 12:00 구름많음 10~14
서울 2017-11-04 00:00 맑음 2~14
서울 2017-11-04 12:00 맑음 2~14
서울 2017-11-05 00:00 맑음 3~15
서울 2017-11-05 12:00 맑음 3~15
서울 2017-11-06 00:00 구름조금 6~16
서울 2017-11-06 12:00 구름조금 6~16
서울 2017-11-07 00:00 구름많음 8~18
서울 2017-11-07 12:00 구름많음 8~18
서울 2017-11-08 00:00 구름많고 비 9~17
서울 2017-11-09 00:00 맑음 6~15
서울 2017-11-10 00:00 구름조금 5~15
인천 2017-11-03 00:00 구름많고 비 10~13
인천 2017-11-03 12:00 구름많음 10~13
인천 2017-11-04 00:00 맑음 4~12
인천 2017-11-04 12:00 맑음 4~12
.
.
.
.
.
.
제주 2017-11-03 00:00 구름많음 15~19
제주 2017-11-03 12:00 구름많음 15~19
제주 2017-11-04 00:00 구름많음 12~16
제주 2017-11-04 12:00 구름조금 12~16
제주 2017-11-05 00:00 구름조금 11~18
제주 2017-11-05 12:00 구름조금 11~18
제주 2017-11-06 00:00 구름많음 12~20
제주 2017-11-06 12:00 구름많음 12~20
제주 2017-11-07 00:00 구름많음 15~20
제주 2017-11-07 12:00 구름많음 15~20
제주 2017-11-08 00:00 구름많음 15~19
제주 2017-11-09 00:00 구름많음 13~17
제주 2017-11-10 00:00 구름조금 13~17
서귀포 2017-11-03 00:00 구름많음 15~21
서귀포 2017-11-03 12:00 구름많음 15~21
서귀포 2017-11-04 00:00 구름많음 11~18
서귀포 2017-11-04 12:00 구름조금 11~18
서귀포 2017-11-05 00:00 구름조금 11~19
서귀포 2017-11-05 12:00 구름조금 11~19
서귀포 2017-11-06 00:00 구름많음 14~21
서귀포 2017-11-06 12:00 구름많음 14~21
서귀포 2017-11-07 00:00 구름많음 15~21
서귀포 2017-11-07 12:00 구름많음 15~21
서귀포 2017-11-08 00:00 구름많음 15~20
서귀포 2017-11-09 00:00 구름많음 13~18
서귀포 2017-11-10 00:00 구름조금 13~19
```

참고로 xml2js는 아래 명령어로 쉽게 설치 할 수 있다.
```
npm install xml2js
```


## Reference
- [기상청](www.kma.go.kr/)
- [xml2js](https://www.npmjs.com/package/xml2js)
