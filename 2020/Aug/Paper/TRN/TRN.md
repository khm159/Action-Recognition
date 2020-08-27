
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

TRN 이전에도 LSTM등 RNN을 많이 사용하였지만, 미래의 정보까지 이용하지는 못했습니다.

3.TRN Cell 구조
---------------

먼저 전체적인 구조는 일반적인 RNN Cell과 동일합니다.

<img src="/2020/Aug/Paper/TRN/img/02.PNG" width="50%" height="40%" title="문제 정의" alt="TRN Cell 01"></img>

이미지 출처 : TRN 논문 : https://arxiv.org/pdf/1811.07391.pdf 

본 논문에서는 online action detection 성능을 향상시키기 위해서 과거와 현재, 미래를 모두 고려하는 TRN 이라는 새로운 RNN cell 구조를 제안했습니다. TRN Cell도 일반적인 RNN Cell과 똑같이, 각 프레임에 대해서 feature extractor로 추출된 피처벡터 <img src="/2020/Aug/Paper/TRN/img/xt.PNG" width="5%" height="5%"></img>와 이전 시점의 hidden state h_t-1를 각 시점 t마다 입력으로 받습니다. 그리고 해당 시점마다 행동인지 아닌지, 만약 행동이라면 어떤 행동인지에 대한 확률 벡터 <img src="/2020/Aug/Paper/TRN/img/pt.PNG"></img>를 출력합니다. 그리고 다음 시점으로 hidden state h_t를 넘겨줍니다. 



