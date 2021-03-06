# ClovaCall

### ClovaCall : 콜센터의 ASR을 위한 목표 지향적인 한국어 대화 Corpus

> **목적**
> 

1.  ASR의 필수성과 발전에 비해 관련 회사들은 여전히 구식의 데이터를 사용하는 방식이에 기존의 데이터보다 차별성이 존재하는 데이터 소개

- Clova dataset
    - 기존의 데이터는 영어 기반-> 한국어 기반
    - 오픈 도메인 대화나 오디오북같은 일반적인 내용 -> 레스토랑 예약 도메인의 목표지향적인 대화

2.  2개의 ASR 모델로 Clova의 데이터셋의 효과 검증

> **연구내용**
> 

**<기존의 문제점>**    
기존의 Corpus는 영어기반이며 일상생활 대화나 오디오북 내용같은 내용의 담고 있음.   
또한 소수의 한국어 기반 Corpus가 공개되도 일반적으로 오픈 도메인 대화를 담고있어서   
도메인별 작업에 적용할 때 데이터별 분포와 어휘 차로 ASR 모델이 낮은 인식 성능을 보임.   

**<Clova 대화 Corpus>**

- AICC의 주요 기능
    
    : ASR, 자연어처리(목적분류,slot filling 등), 목적지향적 스타일에 따른 대화 관리,응답 생성,음성합성
    
- 해당 논문은  ASR component에 초점을 맞추고 레스토랑 예약 시나리오의 대규모 대화 데이터 구성

**<Clova dataset의 장점>**

1. 대부분 10초의 짧은 대화이기에 Librispeech의 오디오북과 같이 end point detection이나 정렬문제를 겪지 않음   
    - Librispeech : 현재 음성인식 연구에 있어서 가장 널리 사용되는 대규모 영어 음성 데이터
                   (16kHz로 약 1000시간 분량의 오디오북 데이터)
    - End Point Detection(EPD) : 음성신호 시작점과 종료점을 검출하는 음성 끝점검출   
    (높은 성능의 음성 인식을 위해 마이크 입력 신호 중 음성인식의 시작과 끝을 구분할, 음성구간의 시작점과 종료점 검출 필수적, 비음성 구간의 잡음 제거)
2. 대부분의 단어와 표현이 App 도메인과 관계없이 사용되기 때문에 AICC기반 예약 서비스에 유용  
ex) 시간,날씨 등

**<데이터 구축 과정>**    
(60746개의 발성과 짧은 문장 쌍의 데이터)

1. sentence pool 만들기(10개의 카테고리, 86개의 의도(통화의 목적), 7개의 전환된 상황을 정의)
    - 각 사람에게 주어진 상황,의도에 대한 답변을 받음
  
2. 통화기반 데이터 생성(생성된 데이터를 통화로 읽게 함)
    - 각 사람마다 10개의 문장을 읽게 함 ( 짧은 대화이기에 end point detection,정렬 문제 존재 X)
    - 각 사람마다 index를 붙임( 스피커 식별 작업을 쉽게 하기 위해)

3. 데이터 세분화(결과 [https://github. com/clovaai/ClovaCall](https://github))
    - 여러 사람의 데이터이기에 많은 noise발생
    - 수집된 데이터에 대한 정성평가 실시
    - 소리의 파형 중 특정 에너지 수준 이하의 시작 또는 끝의 음소거 부분 제거
    (librosa 사용, 25db를 임계치로 사용)
    - 가장 많이 사용되는 말을 가진 상위 30개의 의도 선택
    

**<ClovaCall과 AIHub의 차이>**

1. 많이 사용되는(높은 등급 순위) 단어를 비교했을 때, ClovaCall가 AIHub보다 훨씬 더 많이 사용
2. ClovaCall과 AIHub의 사용된 character의 패턴 비슷, word의 패턴이 다름
   (ClovaCall에서 자주 사용,But AIHub에서 사용 X)

**<실험 설정>**

1. Dataset 
: ClovaCall 외에 2개의 추가 데이터셋 사용
    - QA call dataset(회사의 일상 대화) : 업무별 데이터의 필요성 검증
    - NIA AIHub의 대규모 오픈 도메인 대화 corpus : ASR 모델 사전 제작에 필요
2. training
    - pretraining and finetuing(파라메터 미세 조정)
    - training from scratch
    - training from scratch with data augmentation
      : 목표 지향 상황에서 데이터셋의 효능애 초점 
        → 성능을 올리기 위한 언어 모델 사용 X
    
3. 데이터 처리
    - 주파수를 맞춰줌(8KHz -> 16KHz)
    - input : log-spectrograms( 20ms window size, 10ms stride size : librosa package사용)
4. ASR 모델
    - DS2와 LAS와 같은 두가지 ASR 모델 사용
        - DS2 : CNN + RNN, CTC loss 사용
            - CNN
                
                : 32개의 channel(feature 수,차원의 수)을 가진 2D-Convolutional layers (2차원 합성곱)을 가짐
                   → 각 layer에 대해 4와 2의 stride(filter를 순회하는 간격)
                
            - RNN
                
                : 5개의 양방향 LSTM
                
                (LSTM : 히든 state에 cell- state를 추가한 구조 , vanishing gradient problem 극복)
                → output : softmax distribution
                
            
        - LAS(Listen, Attend and Spell) : sequence to sequence framework와 attention 기법을 사용하여 음성 인식
        
5. Metrics : CER 사용
    - X : 예측
    - Y : 답
    - D: the Levenshtein distance between X, Y
    - CER : D/L * 100

**<결과>**

1. Pretraining and finetuning
#Model
    - AIHub만 사용 : 성능이 낮음
    - AIHub로 훈련 → QA,CC-Base,CC-Full로 튜닝 : 성능이 향상
2. From-scratch training
    - CC보다 QA의 성능이 더 좋게나옴
        
        → QA의 데이터셋이 더 큼으로 더 다양한 상황이 들어있기 때문이라 추측
        
3. From-scratch training with data augmentation
    - 별 다른 결과 발견 X
        
        → nosie에 인한 왜곡이 많이 발생+too enhanced noise-based regularization 때문이라 추측
        

> **정리**
> 

오픈 데이터만 사용한 모델의 성능은 낮음  
이에 각 분야에 따른(특화된) 데이터 셋이 필요함  
각 분야별 데이터로 scratch 학습한 모델보다 오픈 데이터로 사전 학습한 모델이 샘플링속도와  
자주 사용되는 단어 빈도수가 다름에도 정확도가 매우 향상됨  

