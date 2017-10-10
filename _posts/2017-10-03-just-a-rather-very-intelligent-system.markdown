---
layout: post
title:  "Just A Rather Very Intelligent System"
date:   2017-10-3 22:4:38 +0900
tags: jarvis intelligent system ai
---
# J.A.R.V.I.S
* * *
JARVIS stands for 'Just A Rather Very Intelligent System'. As you know, JARVIS is a highly advanced computerized AI developed by Tony Stark to manage almost everything, especially matters related to technology, in Tony's life (1, 2).

자세한 내용은 레퍼런스 혹은 아이언맨 영화를 보면 자비스가 무엇인지 확실히 알 수 있다.

시작하기 전에 이 글을 작성하기 시작한 이유는,,, 마침 발가락이 다쳤고, 추석 연휴가 2주정도 되어 심심함을 달래기 위해 시작했다.

내 방에서 작동하는 자비스를 만들기 위해 우리는 자비스 알고리즘을 아래와 같이 정의할 것이다.

1. **Input** (especially Speech from mic, but it could be others, such as buttons, or text)
2. **Speech To Text** (in order to recognize input commands)
3. **Execute commands** (such as, turn on the light in my room, say what I want to know, close a window in my room and so on)
4. **Text To Speech** (in order to inform us about results of the command)
5. **Output** (to speaker)

문서 작성, 개발, 리서치 등 병행하여 수행할 예정으로 앞뒤가 안 맞을 가능성이 높으므로, 이점 고려하여 읽어주길 바란다.

* * *
## 1. Input
* * *
자비스에게 명령을 내리기 위한 수단으로는 여러 방법이 있다, UI로 button을 조작, text를 입력, 등이 있지만, 여기서는 speech를 입력으로 사용할 것이다.

필요한 장비로는 간단하다 마이크만 있으면 된다... 아주 간단하다, 추가적인 IO를 사용하기 위해서는 (Raspberry Pi를 사용한다고 가정) hardware를 설치해야한다 ([buttons and switches](https://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/robot/buttons_and_switches/)).

TODO: HOW TO record the speech to .wav file or others

* * *
## 2. Speech To Text (STT)
* * *
Input으로 speech를 받았다면 (아마 여기에선 Waveform audio format - WAV를 사용함),

대략,,, 소리 (파동)을 컴퓨터가 알 수 있는 숫자들로 변환하고, 숫자들을 가지고 알파벳을 만들고, 이 알파벳의 집합으로 단어를 **추론**한다

여기서 **추론** 한다는 의미를 생각해 보면, 예를 들어 알파벳의 집합들이 hhhhhhheeeelllllllloooooo라면 우리는 이를 hello 라는 단어일 것이라고 판단할 수 있다.
하지만, 컴퓨터는 모른다! 그렇기 때문에 여기에는 Machine learning이라는 그중에 Deep learning 기술을 사용하여 **아주아주아주** 많은 훈련 데이터로 모델을 학습시키고 추론하는 과정이 필요하다.

**과연 일반인이 만든 model을 학습 시켜서 우리가 원하는 accuracy(정확도)를 충족시킬 수 있을 까??** (3)

답은 거의 불가능하다. 마이크 품질, 잡음, 말투, ... 사투리 ... 등과 같은 거의 무한한 문제를 극복해야 한다.
이러한 문제들을 처리 할 수 있는 방대한 training set이 필요하다. 우리가? 구할수 있을까? 이 데이터를....? **IMPOSSIBLE**

Noting is impossible??? 아니 여기 존재한다, 불가능이다

이렇듯 음성인식 시스템을 구축?하려면 가난한 우리에겐 Open source API를 사용해야 한다.

* [Google Speech API](https://cloud.google.com/speech/)
* [Watson Speech to Text](https://www.ibm.com/watson/services/speech-to-text/)
* [Bing Speech API](https://azure.microsoft.com/ko-kr/services/cognitive-services/speech/)
* [Speech recognition module for Python](https://github.com/Uberi/speech_recognition)

* * *
### 2.1. Google Speech API
* * *
가장 먼저 사용해 볼 API는 Google Speech API이다.

사용 가이드로 [Docs](https://cloud.google.com/speech/docs/)를 보고 차근차근 따라하면 되지만, 영어 울렁증이 있는 분들은 [한글 블로그](http://jeongchul.tistory.com/544)를 참고하여 사용해 보면 된다.

내가 사용하면서 겪은 몇몇 [Trouble Shooting](https://quddnr153.github.io/2017/09/30/google-speech-api.html)을 보는 것도 도움이 될 수도...


사용 후기로는 인식률이 매우 좋다. 한글도 잘 인식된다 (매우 만족).

* * *
### 2.2. Watson Speech to Text
* * *
Watson Speech to Text의 튜토리얼 가이드는 다음을 참고하자 - [see more](https://quddnr153.github.io/2017/10/05/watson-speech-to-text.html)

* * *
### 2.3. Bing Speech API
* * *
TODO: 생략할거같음...

* * *
### 2.4. Speech recognition module for Python
* * *
지금 정리할 SpeechRecognition은 정말정말 간단하게 사용해 볼 수 있다.

[SpeechRecognition](https://github.com/Uberi/speech_recognition) 의 문서만 따라해도 문제없이 동작한다.

[Demo video of SpeechRecognition](https://youtu.be/cWzqeXqvHA8)


* * *
## 중간 정리
* * *
앞서 Speech To Text에 대해 알아봤고, 대략 세가지 API를 사용해 봤다.

Speech recognition을 개인이 만들 수 없는 시스템이기에 Open source의 힘을 빌렸다.

다음으로는 speech를 변환한 text를 어떻게 사용할까? 라는 문제를 해결해야한다.

나는 앞에 사용했던 API 중 가장 간단하게 적용한 SpeechRecognition module for Python 을 사용하기로 한다. (google 과 ibm 에서 물론 bing 에서 api token 또는 credentials 을 필요로하기에 불필요한 설정이 많이 필요했다.)



그렇다면, 무슨 방법을 이용할까? 이다. 간단하게 아래와 같은 생각이 든다.

1. 기존 API 모듈을 수정하여 사용 (하드코딩이 될듯...)
2. Command를 따로 처리할 Applciation (ex, command app)를 만들어, 기존 API 에서는 recognize 한 text를 command app으로 전송하는 작업만 추가 (현재 가장 합리적인 선택이라 생각)

위 생각으로 command application을 만드려 한다. 그렇다면 어떤식으로 만들어 볼까?

1. text file을 읽어 사용 (변환한 text를 file로 생성하고 읽고 삭제하는 작업이... 귀찮아보인다.)
2. text 를 url로 받아 사용 (REST API - 확장성을 고려한다면 좋은 방법이 아닐까?)

이와 같이 2 - 2를 선택한다 (rationale은 괄호안에 작성함).

* * *
## 3. Execute Command
* * *
STT로 parsing된 text command를 수행하는 application이 필요하다.

여러 방법이 있겠지만, 여기선 stt로 변환된 text를 java web application으로 request를 보내 처리하도록 한다.


* * *
## 4. Text To Speech (TTS), and 5. OUTPUT
* * *
JARVIS가 말할 수 있게 해주는 Text To Speech는 비교적 STT 보단 간단하다.

여기선 input으로 "jarvis"가 들어가면, "Yes, master"라고 대답을 해주는 output의 역할까지 해준다. (아직, 명령을 수행하는 부분이 하드코딩 돼있기에, 추후에 이 부분도 지능을 붙이는걸로~)

[기본 동작 동영상 링크](https://youtu.be/bThgvCe8z7I)


# REFERENCE
* * *
- (1) http://ironman.wikia.com/wiki/J.A.R.V.I.S.
- (2) [MARVEL WIKR](http://marvel.wikia.com/wiki/Just_A_Rather_Very_Intelligent_System_(Earth-199999))
- (3) [How to do Speech Recognition with Deep Learning](https://medium.com/@ageitgey/machine-learning-is-fun-part-6-how-to-do-speech-recognition-with-deep-learning-28293c162f7a)

