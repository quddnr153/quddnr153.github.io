---
layout: post
title:  "usage of Watson - Speech to Text"
date:   2017-10-5 14:13:0 +0900
tags: ibm stt watson ai recognizer
---
# Watson Speech to Text
* * *
**TODO: Speech API에 대한 내용 추가**

서론이 길어지면 포스팅이 지루해 진다. 이번 포스팅의 주제는 **Watson Speech to Text 사용하기!** 이다

포스팅 내용은 watson을 사용하기 위한 preconditions, apply my app, and evaluation 이다.
[see more - watson](https://www.ibm.com/watson/services/speech-to-text/)

2017/10/6 일자로 한국어는 지원하지 않는 상태이다 (지원언어: Brazilian Portuguese, French, Japanese, Mandarin Chinese, Modern Standard Arabic, Spanish, Uk English, US English)

* * *
## Precondition
* * *
- IBM Bluemix account 만들기
- Speech to Text service 만들기
- Getting started tutorial (cURL-based tutorial)

* * *
### IBM Bluemix account 만들기
* * *
여기서는 Watson을 사용하기 위한 IBM Bluemix 계정을 만들어 보겠다.

우선, [watson speech to text](https://www.ibm.com/watson/services/speech-to-text/)로 들어간다.

- 아래와 같이 **Get started free** 클릭

![Get started free](/assets/img/watson-home-page.PNG)

IBM Bluemix 계정이 있으면 로그인, 아니면 계정 생성을 한다 (추가 설명은 필요 없을 것 같아 생략).

* * *
### Speech to Text service 만들기
* * *
위 과정으로 로그인을 하면, 혹은 로그인 상태에서 **Get started free**로 들어가면, 서비스 생성 화면이 아래와 같이 나온다.

- 필요 정보를 입력하고 **작성** 클릭

![watson service creation](/assets/img/watson-service-creation.PNG)


* * *
### Getting started tutorial (cURL-based tutorial)
* * *
IBM Speech to Text 서비스는 audio를 text로 바꿔준다. 이는 [cURL-based](https://en.wikipedia.org/wiki/CURL)로 사용할 수 있다.

간단하게 cURL-based란 URL syntax를 사용하여 파일을 getting 또는 sending 하기 위한 command line 도구 이다. 예를 들면, example.com 페이지를 추적하려면 아래와 같은 command line 명령어를 사용한다.
```
curl www.example.com
```

#### STEP 1: Log in, create the service, and get your credentials

위에서 log in 과 create the service 는 설명하였다.

service를 사용하기 위해선 credetials정보를 알아야하기 때문에 아래와 같이 credential 복사 작업이 필요하다.

1. click **Service credentials (한글 페이지일 경우, 서비스 신임 정보)**
2. 서비스를 생성하면서 지정한 credential 정보의 username과 password를 복사한다.

![watson service credentials](/assets/img/watson-service-credentials.PNG)


#### STEP 2: Transcribe audio with no options

다음 단계로는 test audio파일을 전송하여 작동 확인을 하는 작업이다.

1. Download the sample audio file [sample](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac)
2. curl 명령어를 이용하여 sample파일을 전송하여 JSON형식으로 데이터 받기

- {username} and {password}는 위에서 복사한 credentials로 replace
- {path_to_file}은 sample 파일이 있는 위치로 수정

```
curl -X POST -u {username}:{password} \
--header "Content-Type: audio/flac" \
--header "Transfer-Encoding: chunked" \
--data-binary @{path_to_file}audio-file.flac \
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
위와 같이 보내면, 아래 JSON형식으로 받을 수 있다.
```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.8691191673278809,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms
swept through colorado on sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}

```
- 아래 image는 테스트 내용을 보여준다.

![watson sample test 01](/assets/img/watson-sample-test-01.jpg)

- 추가로 다른 형식을 사용해 보겠다 (wav file - [see more](https://console.bluemix.net/docs/services/speech-to-text/input.html#input)).

![watson sample test 02](/assets/img/watson-sample-test-02.jpg)


#### STEP 3: Transcribe audio with additional options

세번째 튜토리얼은 document를 참고하도록 남겨두겠다. 왜냐하면, 옵션들이 여러개 있기 때문에, 모든 예제를 담을 수 없기에...

[Speech to Text Getting started tutorial documents](https://console.bluemix.net/docs/services/speech-to-text/getting-started.html#gettingStarted)

* * *
## Apply my app
* * *
SDK 가 여러 언어로 지원되고 있기 때문에, 개인 application에 적용하기 쉬울 것이다 ([overview for developers](https://console.bluemix.net/docs/services/speech-to-text/developer-overview.html#developerOverview)).

계획상 어플을 개발하고 포스팅하려 했지만, 한글지원이 안되기에 시간절약! Java, node, python, swift등등 여러 언어가 지원하고 또한 docs 도 잘 작성되어있어 **[Watson Developer Cloud](https://github.com/watson-developer-cloud)** 를 참고하여 개발하면 된다.

TODO: nodejs 앱을 사용하는 중,,, 정리는 나중에...
