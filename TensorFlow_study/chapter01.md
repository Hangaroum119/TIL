해당 내용은 *텐서플로도 배우는 딥러닝(저자 솔라리스)*를 정리한 내용입니다

> 인공지능, 머신러닝, 딥러닝 소개
> 

## 1.1 딥러닝 알고리즘의 등장 배경


- **딥러닝이란?**

      : 머신러닝이란 학문 분야 방법론 중 하나로 인공신경망을 여러 층 쌓아 올린 기법

        인공신경망을 깊게(여러층) 쌓아 올려 학습하기 때문에 딥러닝이라 불림

- **머신러닝이란?**

      : 어떤 문제(T)에 관련된 경험(E)로부터 성과 측정 지표(P)를 가지고
        학습을 진행하는 컴퓨터 프로그램(1998년 톰 미첼 교수)

      : 인공지능이란 연구 분야의 방법론 중 하나로 컴퓨터가 인간과 같은 지능적인 행동을 
       할 수 있게 해주는 기법들을 연구하는 분야

(*예시)
[스팸 이메일 분류 문제 ]
 - T : 이메일이 스팸인지 분류하는 것
 - E : 스팸 필터가 레이블한 이메일이 스팸인지 아닌지 관찰하는 것
 - P :  이메일이 스팸인지 아닌지 정확하게 분류한 개수 혹은 비율*

- **개념의 크기**

      : 인공지능(AI) > 머신러닝 > 딥러닝

- 딥러닝 알고리즘은 사실 갑자기 등장한 것이 아닌 오랜 전에 나왔다. 
그럼에도 많은 주목을 받지 못했다 받는 **이유**는 무엇일까?

(1) 데이터의 축척
(2) GPU 필두로 컴퓨팅 파워의 발전
(3) 새로운 딥러닝 알고리즘의 개발(overfitting을 해결할 수 있는 ReLU,Dropout 등의 개발)

- **Hand-Crafted Feature와 Learned Feature**

      (1) Feature(특성, 속성 등) 
          : 관찰 데이터( ex)사람 )에 대한 속성(특징, ex) 몸무게,키,성별)을 표현한 것

      (2) Hand-Crafted Feature 
          : 전문가들의 의견을 통해 바탕으로 추출한 feature

      (3)  Learned Feature
          : 모델 학습 과정에서 자동으로 추출된 feature

- **머신러닝 학습 방법**

      (1) 지도 학습

      (2) 비지도 학습

      (3) 강화 학습 

## 1.2 지도학습


: 정답 데이터가 존재하는 상황에서 학습하는 알고리즘
: 입력데이터(x)와 해당 데이터의 정답 레이블(y)의 쌍을 이용해서 학습하는 알고리즘