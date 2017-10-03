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

# REFERENCE
* * *
- (1) http://ironman.wikia.com/wiki/J.A.R.V.I.S.
- (2) [MARVEL WIKR](http://marvel.wikia.com/wiki/Just_A_Rather_Very_Intelligent_System_(Earth-199999))
- (3) [How to do Speech Recognition with Deep Learning](https://medium.com/@ageitgey/machine-learning-is-fun-part-6-how-to-do-speech-recognition-with-deep-learning-28293c162f7a)
