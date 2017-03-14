# 딥러닝 예제로 보는 개발자 통계 요약
## Intro
- AI가 Computer로 하는 모든 것?
- ML이 그 안 포함, 또 그 안에 DL이 있다.
- ML은 Regression부터라고 생각한다.
- ML을 가지고 블로그의 노출 순위같은 문제를 풀었다. 하지만 못푸는 문제들도 많다.

## 왜 통계인가?
K-means는 평균, 가까운데 점을 놓고 다시 평균이동시키고.
EM을 변형한 것이라고 볼 때 이 알고리즘을 이걸 완벽히 이해할 수 있는 방법이다.

## 딥러닝을 통계적 시선으로
- 기존의 DL에 대한 교육을 받은 사람들은 파라미터가 많고 속도가 드리고 복잡하다고 생각했다. 그러나 딥러닝이 너무 선전함.
- 딥러닝은 우리가 생각할 수 없는 방식으로 반복하여 문제를 해결 하는 방법이다.
딥러닝을 우리가 해야하는 분야가 맞다.
- Gerneralized linear Module이 연속적인 것 뿐  
![](/images/2017/01/Generalized Linear Model.png)
- DNN은 통계적으로보면 GLM의 중첩. 반복적인 GLM이라고 볼 수 있음.
- 통계와 딥러닝  
![](/images/2017/01/statistics_DNN.png)
- 통계 영역에서 이미 연구된 것들을 토대로 영감을 받을 수 있다. 통계에서는 있지만 딥러닝에서는 없는 영역을 살펴보는 것.

### ReLu
- 통계과 딥러닝의 접점이다.
- 보통 Sigmoid를 사용하지만 Relu가 큰 영감을 줬다.
- 목적은 Activation이기만하면된다. sigmoid말고도 Relu나 사용가능하다. non linear하게 찌그러뜨려서 목적만 달성하면 된다.

### Dropout
- 데이터의 개수보다 추정해야하는 Weight의 수가 많으면 문제가 반드시 발생  
![](/images/2017/01/dropout.png)

### 분포통계
아래 분포의 관계로부터 출발, 구조를 파악하자.  
![](/images/2017/01/dist.png)

### 회귀통계
![](/images/2017/01/Linear_regression.svg.png)
Generalized Linear Model은 Linear, Logistic, Possion, Gamma regression을 포함한다.
아래는 헤깔릴 수 있는 두 용어 정리  
![](/images/2017/01/GLM.png)
