
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





4.TRN Cell 구조
---------------

먼저 전체적인 구조는 일반적인 RNN Cell과 동일합니다.

<img src="/2020/Aug/Paper/TRN/img/02.PNG" width="50%" height="40%" title="문제 정의" alt="TRN Cell 01"></img>

이미지 출처 : TRN 논문 : https://arxiv.org/pdf/1811.07391.pdf 

본 논문에서는 online action detection 성능을 향상시키기 위해서 과거와 현재, 미래를 모두 고려하는 TRN 이라는 새로운 RNN cell 구조를 제안했습니다. TRN Cell도 일반적인 RNN Cell과 똑같이, 각 프레임에 대해서 feature extractor로 추출된 피처벡터 <img src="/2020/Aug/Paper/TRN/img/xt.PNG" width="2%" height="2%"></img>와 이전 시점의 hidden state <img src="/2020/Aug/Paper/TRN/img/h-1.PNG" width="4%" height="4%"></img>를 각 시점 t마다 입력으로 받습니다. 그리고 해당 시점마다 행동인지 아닌지, 만약 행동이라면 어떤 행동인지에 대한 확률 벡터 <img src="/2020/Aug/Paper/TRN/img/pt.PNG" width="2%" height="2%"></img>를 출력합니다. 그리고 다음 시점으로 hidden state <img src="/2020/Aug/Paper/TRN/img/ht.PNG" width="2%" height="2%"></img>를 넘겨줍니다. 


  


