
Temporal Recurrent Networks for Online Action Detection
=======================================================
논문 세미나를 준비하면서 공부한 기록입니다. 

틀린곳이 있다면 지적해주시면 감사할거 같아요. 

논문 링크 : https://arxiv.org/pdf/1811.07391.pdf

저자 정보 : Mingze Xu, Mingfei Gao, Yi-Ting Chen, Larry S. Davis, and David J. Crandall

개제 정보 : ICCV 2019


1.논문 선정 이유
---------------
제가 이 논문을 선택한 이유는 TRN 이전의 online action detection과 크게 다른 양상으로 발전한 논문이기 때문입니다. 


TRN 이전에는 현재 진행되고 있는 action을 판단하는데, 과거와 현재의 정보만을 사용했지만,TRN에서는 과거와 현재를 기반으로 미래의 정보를 예측해 과거 현재 미래의 3가지의 정보를 모두 사용한 것이 특징입니다. 


2.Online Action Detection?
---------------

먼저 online action detection에 대해서 설명 드리겠습니다. 

Action detection은 크게 online action detection 과 offline action detection으로 나뉘는데요, 똑같이 untrimmed video에 대한 detection 을 하는 것은 동일하지만, Offline action detection이 모든 프레임을 관측한 뒤 결정을 내린다면, online action detection은 모든 프레임을 관측하지 않고 실시간 스트리밍 동영상에 대해서 빠르게 판단해야 하기 때문에 더 어려운 문제입니다.

문제를 정의해보면 다음과 같습니다.

<img src="/2020/Aug/Paper/TRN/img/01.PNG" width="50%" height="40%" title="문제 정의" alt="문제 정의"></img>

즉, 과거와 현재까지의 프레임을 기반으로 현재의 행동에 대한 확률 벡터를 출력해야합니다.  

3.논문의 Insight
----------------


<img src="/2020/Aug/Paper/TRN/img/03.PNG" width="50%" height="40%" title="문제 정의" alt="TRN Cell 01"></img>

행동이 일어나는 도중에 이를 판단 하는 것은 어려운 문제입니다. 사람이 다가오며 손을 뻗을 때 그것이 악수가 될지, 갑자기 그가 펀치를 날릴지 예측하기 어렵기도 합니다. 이럴 때 사람은 이전 경험들과 함께 앞으로 일어날 일들을 미리 상상해 유연하게 대처할 수 있습니다. 뇌과학과 인지과학에서 밝혀진 바에 따르면, 인간의 뇌는 현재 상황에 대한 판단을 내릴 때 미래에 대한 예측을 적극적으로 활용해 판단을 한다고 합니다. __즉 현재에 대한 판단에 미래에 대한 예측이 중요하게 작용한다는 것이지요.__ 

그런데,TRN 이전에도 LSTM등 RNN을 많이 사용하였지만, 미래의 정보까지 이용하지는 못했습니다. 이전 연구결과들은 현재와 과거의 정보만을 이용해 action detection 을 하였습니다. 저자들은 __미래에 대한 정보까지 함께 고려하면 성능이 향상될 것__ 이라는 가설을 세우고 이를 검증하기위해서 TRN 이라는 새로운 RNN Cell을 설계했습니다. 미래 프레임에 대한 정보를 예측하여 과거, 현재, 미래에 대한 정보를 모두 이용하여 행동 분류를 수행하는 네트워크 구조를 통해 성능을 끌어올린 것이 주요 내용입니다. 

[논문에 대한 개요]

__- 이전 연구결과들은 과거 또는 현재의 정보만을 이용해 판단 하였다.__

__- 미래를 예측하여 판단에 이용하는 구조가 detection 성능을 향상시킬지를 본 논문에서 검증하였다.__

__- RNN cell 자체에 Decoder RNN cell를 포함하여  Future state를 생성하고 이를 예측에 이용한다.__





4.TRN Cell 구조
---------------

먼저 전체적인 구조는 일반적인 RNN Cell과 동일합니다.

<img src="/2020/Aug/Paper/TRN/img/02.PNG" width="50%" height="40%" title="문제 정의" alt="TRN Cell 01"></img>

이미지 출처 : TRN 논문 : https://arxiv.org/pdf/1811.07391.pdf 

본 논문에서는 online action detection 성능을 향상시키기 위해서 과거와 현재, 미래를 모두 고려하는 TRN 이라는 새로운 RNN cell 구조를 제안했습니다. TRN Cell도 일반적인 RNN Cell과 똑같이, 각 프레임에 대해서 feature extractor로 추출된 피처벡터 <img src="/2020/Aug/Paper/TRN/img/xt.PNG" width="2%" height="2%"></img>와 이전 시점의 hidden state <img src="/2020/Aug/Paper/TRN/img/h-1.PNG" width="4%" height="4%"></img>를 각 시점 t마다 입력으로 받습니다. 그리고 해당 시점마다 행동인지 아닌지, 만약 행동이라면 어떤 행동인지에 대한 확률 벡터 <img src="/2020/Aug/Paper/TRN/img/pt.PNG" width="2%" height="2%"></img>를 출력합니다. 그리고 다음 시점으로 hidden state <img src="/2020/Aug/Paper/TRN/img/ht.PNG" width="2%" height="2%"></img>를 넘겨줍니다. 

<img src="/2020/Aug/Paper/TRN/img/04.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

TRN Cell은 3개의 핵심 모듈로 구성 되어 있습니다.

__Temporal decoder 모듈, future gate,  Spatio Temporal accumulator 입니다.__

1) Temporal decoder는 이전 hidden state와 현재 프레임에 해단 feature vector를 입력 받아 미래의 연속된 몇 개의 프레임에 대한 future representation 을 출력합니다.

2) Future gate에서는 fully connected layer를 이용하여 이 future representation 들을 fusion 합니다. 

3) Spatio Temporal Accumulator, 줄여서 STA 에서는 future gate에서 생성된 미래 상태벡터와 현재 프레임으로부터 입력된 feature 벡터, 그리고 이전 cell의 과거의 hidden state
즉 과거, 미래, 현재의 정보를 모두 고려하여 시점 t에 대한 최종 확률 벡터를 출력합니다.

4.1 Temporal Decoder
-------------------- 

먼저, TRN의 핵심이라고 할 수 있는 Temporal Decoder에 대해 설명드리겠습니다.  
<img src="/2020/Aug/Paper/TRN/img/05.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

TRN Cell은 Cell 내부에 연속된 decoder RNN cell을 내장하고 있는것이 특징입니다. TRN Cell에서 사용된 RNN Cell들은, 기본적인 RNNCell의 입출력 구조를 가지고 있으면, 어떤것이든지 백본으로 사용가능합니다. 예를들면, 대표적인 GRU나 LSTM과 같은 것이 있겠습니다. 논문 저자들은 LSTM을 백본으로 사용하였습니다. 

<img src="/2020/Aug/Paper/TRN/img/06.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

입력되는 hidden state를 바로 쓰지 않고 embedding layer인 fully connected 를 거쳐 linear transformation한 벡터를 입력합니다.

<img src="/2020/Aug/Paper/TRN/img/07.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

임베딩된 hidden state vector와 image feature vector를 이용해 decoder RNN cell은 이어질 미래의 프레임 t에 대한 각 확률벡터를 예측합니다.
이전 cell 에서 산출된 action score는 각 decoder cell 에서 생성된 pt를 역시 fully connected layer를 통해 linear transformation 하여 다음 rnn cell에 입력합니다.

<img src="/2020/Aug/Paper/TRN/img/08.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

백본 RNN cell의 출력을 각 시점에 대한 future representation vector ft로 사용합니다. 
이렇게 프레임it에 대한 미래의 ld 만큼의 프레임을 예측하는 future representation sequence가 생성됩니다. 

<img src="/2020/Aug/Paper/TRN/img/09.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

Future gate에서는 temporal decoder에서 생성된 벡터 시퀀스를 STA RNN 셀에 입력 벡터로 입력하기 위해 
하나의 벡터로 사상하기 위한 fully connected 레이어를 두었으며, 이전에 먼저 average pooling을 수행한 뒤 fc레이어를 거쳐 
벡터 ~xt로 임베딩합니다. 활성화 함수는 relu를 사용합니다.

<img src="/2020/Aug/Paper/TRN/img/10.PNG" width="60%" height="60%" title="문제 정의" alt="TRN Cell 01"></img>

최종적으로 TRN cell의 확률벡터를 출력하는 STA RNN Cell은 총 2개의 입력을 받습니다. 
Future gate의 출력 벡터와 현재 프레임의 image feature 벡터를 concatenate한 입력벡터
이전 TRN cell의 hidden state 

해당 cell의 출력인 hidden state ht를 마지막으로 classification layer인 fully connected layer에 입력합니다. 
소프트맥스를 통해서 최종 행동분류 확률 벡터를 출력합니다. 


4.2 Future gate
--------------------

4.3 Spatio Temporal Accumulator
--------------------




