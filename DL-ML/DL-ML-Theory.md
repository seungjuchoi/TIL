# Intro
- Image-net 1.5milion  1000 object구분하는 것, 2012년에서는 CNN이 winner. 2012년 이후로는 모두 CNN위주가 된다. 이 분야에서 Back propergation의 본격적으로 쓰이기 시작했다.

# 개념 설명
## Distance 법
![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/08/Manhattan_distance.svg/283px-Manhattan_distance.svg.png)
    - L1 Distance: Manhattan (Blue line)  
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/4704625b5a645aae2cd0177cab7e8892b8f962bf)

    - L2 Distance: **Euclidean** (Green line)  
    ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/dc0281a964ec758cca02ab9ef91a7f54ac00d4b7)

## Test set and Validation set
- k-nearest neighbor 분류기에서 k는 유효한 데이터set에 의해 튜닝되는 Hyper parameter이다.
- 머신러닝 알고리즘을 디자인할 때, 테스트 셋은 매우 귀한 리소스이고, 이론적으로는 실제로 알고리즘을 평가할 때인 맨 마지막 단 한 번을 제외하고는 절대 쳐다봐서는 안 된다.
- 우리가 테스트 셋을 사용하여 Hyper parameter 들을 튜닝했다는 것은 곧 우리가 테스트 셋을 마치 학습 데이터셋(트레이닝 셋)처럼 사용한 것이고, 우리 모델의 테스트 셋에서의 성능은 실제로 다른 데이터에 적용할 때에 비해 너무 낙관적이게 되어버린다. 이것을 테스트 셋에 overfit이라고 한다.
- 해결책1: 검증 셋(validation set) 으로 불리는, 약간 적은 수의 트레이닝 셋과 나머지로 나눈다. CIFAR-10 데이터셋을 예로 들면, 학습 이미지들 중에 49,000 장을 트레이닝 셋으로 삼고, 나머지 1,000 개를 검증으로 남겨둔다.
- 해결책2: 교차 검증을 한다. 검증 Set을 바꿔가면서 검증한다. 보통 교차 검증보다 하나의 검증 셋을 정해놓는 것을 선호한다. 계산량이 많기때문이다.
- Hyper parameter 개수가 매우 많다면, 검증 데이터셋의 크기를 늘리는게 좋다.

# 학습 알고리즘
## 단순 비교
- 두개의 이미지를 비교하는 간단한 방법은 빼는 것
- 두 이미지가 똑같을 경우에는 결과가 0일 것이고, 두 이미지가 매우 다르다면 결과값이 클 것이다.
- Distance 법을 이용해서 차를 구하면 된다.

## K-NN
### K-NN 특징
![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/KnnClassification.svg/220px-KnnClassification.svg.png)
- 단순 비교방법보다 뛰어난 첫번째 방법으로 하나만 비교하는 것이 아니라 인접한 k개의 Image를 찾는 것이다.
- 최근접 알고리즘으로 가장 간단한 ML 알고리즘이다.
- k개의 최근접 이웃 사이에서 가장 공통적인 항목에 할당되는 객체로 과반수 의결에 의해 분류한다. k가 커질수록 Outliner가 강인해주지고 경계가 부드러워진다.
- Instance-based learning: memory-based learning이라고도 함. memory에 저장된 instance를 기반으로 새로운 문제를 비교하는 방식, 따라서 모든 데이터를 기억해야 한다. 너무 많은 계산량을 요구하는 것과 모든 데이터를 저장하고 있어야하는 것이 K-NN의 대표적인 단점
- lazy learning: 지역적이고 모든 계산이 끝날때까지 답이 나오지 않는다. 일반화가 query가 모두 처리될때까지 지속적으로 연기되는 것을 의미한다.
- 거리를 n이라고 가정하면 간단한 가중치 스키마는 1/n의 가중치를 주는 것이다.
- 거리는 유클리드를 많이 사용한다.
- k를 경험적으로 선택하는 방법은 부트스트랩이다.

### K-NN의 장점과 한계
- 학습시에는 계산량이 거의 없고 구현이 쉽다. 그냥 저장만 하면 된다. 그런데 테스트시에는 계산량이 많이 진다. 때문에 차원이 낮은 데이터의 경우에 좋다. 이미지는 매우 고차원이라 적합하지 않다.
- k-Nearest Neighbor는 이런 상황에 불리하다. Shift된 사진 눈코입이 없는 사진, 어두운 사진. 이미지의 클래스보다 배경이나 이미지의 전체적인 색깔 분포 등에 더 큰 영향을 받기 때문이다.
- 고양이를 인지하는 것이 여려운이유는 RGB 데이터가 많고 자세, 밝기, 각도에 따라 그 데이터가 너무 크게 변화하기 때문이다.
- 현재의 kNN 분류기가 너무 느리다면, 이를 가속하기 위해 Approximate Nearest Neighbor 라이브러리 (e.g. FLANN)를 사용하는 것을 고려해보라. (성능은 조금 떨어질 것이다)
- Linear classification는 Circle model에 약하다. wx+b 구조는 1차 함수와 비슷. gray scale의 경우는 사용 하기 힘들다.

# Classifier
## Linear Classifier
- Machine learning의 일종으로 확률적 분류 방법이다.
- Score 함수와 Loss 함수를 통해 선형화 시킨다. 예를 들어 Image 같은 data를 2차원 좌표의 벡터나 점으로 매핑한다. Convolution Neural Network에서는 비슷한 방법을 취하지만 훨씬 복잡하다.
## Linear Classifier 구성
1. Score function: 선형 함수, Pixel data를 넣으면 class score로 계산해주는 파라미터화된(w) 함수, 복잡한 구조로 계속 변경됨.
2. Loss function: 특정 paraemter를 적용시켜 score함수를 만들었을 때 실제 데이터와 얼마나 차이나는 가에 대해 parameter의 정확도를 측정하는 함수 (SVM/Softmax)
3. Optimazation: 은 Loss 함수가 최소화되는 최적은 Parameter값을 찾는 것.

### Score 함수
![](http://aikorea.org/cs231n/assets/imagemap.jpg)
이미지 픽셀들을 펼쳐서 열 벡터로 만들고 각 클래스에 대해 행렬곱을 수행하면 스코어 값을 얻을 수 있다
예시는 잘못된 W에 의해 잘못된 판단을 하고 있다는 것을 알고 있자.

![](http://aikorea.org/cs231n/assets/pixelspace.jpeg)
빨간색 선의 오른쪽에 있는 점들은 양의 (그리고 선형적으로 증가하는) 스코어 값을 가질 것이고, 왼쪽의 점들은 음의 (그리고 선형적으로 감소하는) 스코어 값을 가질 것이다.

앞으로 내용을 전개해 나갈 때 두 가지 파라미터를 (bias b와 weight W) 매번 동시에 고려해야 한다면 표현이 번거로워진다. 흔히 사용하는 트릭은 이 두 파라미터들을 하나의 행렬로 합치고, xixi를 항상 11의 값을 갖는 한 차원 - 디폴트 bias 차원 - 을 늘리는 방식이다. 이 한 차원 추가하는 것으로, 새 스코어 함수는 행렬곱 한 번으로 계산이 가능해진다

![](http://aikorea.org/cs231n/assets/wb.jpeg)

### Loss 함수 시각화
예를 들어 CIFAR-10의 경우, 파라미터(parameter/weight) 행렬은 크기가 [10 x 3073]이고 총 30,730개의 파라미터(parameter/weight)가 있다. 무작위로 뽑은 방향 W1을 잡고, 이 방향을 따라 가면서 손실함수(loss function)를 계산해본다. 'L(W+aW1)' 이 과정에서 a 값을 x축, 손실함수(loss function) 값을 y축에 놓고 간단한 그래프를 그릴 수 있다.

![](http://aikorea.org/cs231n/assets/svm_one.jpg)

미분 불가능한 손실함수의 경우 subgradient가 존재하고 이를 gradient로 대체한다.

### 최적화: Loss를 줄이기 위한 전략 3가지
- SVM의 Loss 함수의 볼록 함수이기 때문에 이상하게 생각할지 모른다. 몇가지 전략을 살펴보자.
- **첫번째**, Random search: 무작위 탐색 w값을 랜덤하게 추출하여 비교한다. loss가 가장 적을 때는 기억하는 간단한 min알고리즘이다.
- **두번째**, Random local search: 무작위 국소 탐색, 임의의 W에서 시작하여 또다른 임의의 방향으로 살짝 움직였을 때 Loss를 비교하여 거기로 움직이고 다시 탐색함.
- **세번째**, Following Gradient: 그라디언트 따라가기, 손실함수는 gradient와 관계가 있다. 모든 차원을 하나씩 돌아가면서 그 방향으로 작은 변화 h를 줬을 때, 손실함수(loss function)의 값이 얼마나 변하는지를 구해서, 그 방향의 편미분 값을 계산한다.

## Gradient 계산법 2가지 (미분)
1. 수치적(Numerical) gradient: 느리고 근사값이지만 쉬운 방법
  - 수식을 이용하여 그라디언트(gradient)를 수치적으로 계산
  - 임의의 함수 f에 입력값으로 넣을 벡터 x 가 주어졌을 때, x에서 f의 gradient)를 계산해주는 범용 함수가 있다.
  - h가 0으로 수렴할 때의 극한값이 그라디언트(gradient)의 수학적으로 정의다.
  - 방향은 gradient로 알 수 있지만 얼마만큼 가야하는 가는 알 수 없는데 그것을 Step 크기라고 명명하고 Hyper parameter로 생각하면 된다. 이것을 학습 속도라고도 할 수 있다. 이 parameter는 결과에 영향을 많이 미치기 때문에 결정하기 까다롭다.
  - Step size가 너무 큰 값을 가지게 되면 좋지 않을 수도 있다. 비유컨데 눈 가리고 올라가는데 큰 걸음으로 성큼성큼 올라가면 도랑에 빠질 수 있다.
  - 단점은 gradient를 수치적으로 계산하는 데 드는 비용은 파라미터(parameter/weight)의 수에 따라 선형적으로 늘어난다. 모든 parameter를 검사해야하기 때문인데 계산이 비효율적이다. 신경망(neural networks)들은 수천만개의 파라미터(parameter/weight)의 경우 문제가 심각해진다.
  - 또다른 단점은 h가 0으로 수렴할 때가 극한값인데, 여기서는 그냥 작은 “h”값을 쓰기 때문에 근사값이라는 것이다.  
  ![](/images/2017/01/Selection_001.png)
    ```javascript
    var x = -2, y = 3;
    var out = forwardMultiplyGate(x, y); // -6
    var h = 0.0001;

    // x 에 대한 변화율을 계산
    var xph = x + h; // -1.9999
    var out2 = forwardMultiplyGate(xph, y); // -5.9997
    var x_derivative = (out2 - out) / h; // 3.0

    // y 에 대한 변화율을 계산
    var yph = y + h; // 3.0001
    var out3 = forwardMultiplyGate(x, yph); // -6.0002
    var y_derivative = (out3 - out) / h; // -2.0
    ```
  - 윗 코드에서 h를 적당히 작은 수(0.0001)를 선택해서 미분을 구하고 있다.
  - **핵심**: 입력에 대한 변화율은 입력을 조금 변경시켜 나오는 출력 값의 차이를 관찰하여 계산한다.
    ```javascript
    var step_size = 0.01;
    var out = forwardMultiplyGate(x, y); // 변경전: -6
    x = x + step_size * x_derivative; // x 는 -1.97 로 변경
    y = y + step_size * y_derivative; // y 는 2.98 로 변경
    var out_new = forwardMultiplyGate(x, y); // -5.87! 만세.
    ```

2. 해석적 (Analytic) gradient: 빠르고 정확하지만 **미분** 이 필요하고 실수하기 쉬운 방법
  - Numerical보다 더 쉽고 훨씬 빠르게 기울기를 계산하는 방법이다. 수학적 공식을 활용한다.
  - Analytic방법은 입력 값을 조작할 필요가 없다. 수학(미분)으로 공식을 유도할 수 있습니다.
  - 미적분을 이용한 gradient: 미분을 이용해 해석적으로 gradient를 구하는 방법으로 근사치가 아닌 정확한 수식을 사용하기 때문에 계산이 빠르다. 하지만 수치적으로 구한 gradient과 다르게 구현하는 데 실수하기 쉽다.
  - 뉴럴 네트워크 라이브러리는 모두 공식 기울기를 계산한다. 하지만 수치적 gradient값과 비교를 해야하는 과정이 필요한데 이것을 gradient check라고 한다.
  - 해석적 gradient는 간단한 공식을 유도할 때나 쉽고 거대하고 복잡한 식에는 빠르지 않을 것 같다고 생각하기 쉽다. 하지만 복잡한 저체회로와 상관없이 각각의 게이트는 스스로의 문제에만 집중한다는 것 알 수 있다. 전체 회로와 큰 연관성이 없다는 말이다.

## Gradient 하강 (descent)
### 단순 하강
```
while True:
  weights_grad = evaluate_gradient(loss_fun, data, weights)
  weights += - step_size * weights_grad # 파라미터 업데이트(parameter update)
```
- 이 단순한 loop가 NN의 중심이다.
- 현재로는 이 gradient 하강이 손실함수를 최적화하는 용도로 많이 쓰인다.
- 그라디언트(gradient)를 따라서 움직인다는 기본적인 개념은 다른 알고리즘을 더한다고 하더라도 변하지 않는다.

### 미니배치 하강
```
while True:
  data_batch = sample_training_data(data, 256) # 예제 256개짜리 미니배치(mini-batch)
  weights_grad = evaluate_gradient(loss_fun, data_batch, weights)
  weights += - step_size * weights_grad # 파라미터 업데이트(parameter update)
```
- Mini-batch gradient 하강: 대규모의 테스트셋에서는 전체 학습데이터를 가지고 gradient을 구하는 것이 아니라 선택된 배치batches만을 이용해서 구한다. (예, 120만개 중에 256개짜리 batch만을 이용) 즉, 샘플링한다는 것.
- 이것이 mini-batch가 의미있는 이유는 학습데이터들의 예시들이 서로 상관관계가 있기 때문이다.
- 미니배치(mini-batch)에서 그라디언트(gradient)를 구해서 더 자주 파라미터(parameter/weight)을 업데이트하면 실제로 더 빠른 수렴하게 된다.
- 미니배치(mini-batch)의 크기도 하이퍼파라미터(hyperparameter)다. 대체로 2의 제곱수를 이용한다. 많은 벡터 계산이 이 경우 더 빠르기 때문이다.

## 추가 정리
- SVM의 손실함수(loss function)가 부분적으로 선형(linear)인 밥공기 모양이라는 것을 확인했다.
- 손실함수(loss function)를 파라미터(parameter/weight)로 미분하여 그라디언트(gradient)를 계산하는 법(과 그에 대한 직관적인 이해)가 신경망(neural network)를 디자인하고 학습시키고 이해하는데 있어 가장 중요한 기술이라는 점을 기억하자.
- 그라디언(gradient)를 해석적으로 구할 때 **연쇄법칙을 이용한, backpropagation** 이라고도 불리는 효율적인 방법을 사용하면 컨볼루션 신경망 (Convolutional Neural Networks)을 포함한 모든 종류의 신경망(Neural Networks)에서 쓰이는 상대적으로 일반적인 손실함수(loss function)를 효율적으로 최적화할 수 있다.

# 연쇄 법칙(Chain rule)과 Backpropagation
## 들어가기 전에
- F는 손실함수, Input으로 들어오는 x,w는 Input과 Weight로 Parameter를 계산하여 Gradient를 구하고 이를 다시 Parameter를 업데이트한다.
- Gradient는 편미분 값들의 벡터로 정의한다. 심플하게 x에 대한 편미분이라는 정확한 표현대신 x에 대한 gradient라고 표현하자.
- gradient는 f의 입력값에 대한 민감도로 볼 수 있다.
- 함수의 Gradient를 의미: ∇$f(x)$
- [dfdx,dfdy,dfdz] 변수로 Gradient가 표현되는데 이는 f에 대한 변수 x,y,z의 민감도(sensitivity) 보여준다.
- 아래 세개의 함수 형태에 주목하자.
  - $f(x,y)=xy$의 경우는 편미분값이 ∂f∂x=y,  ∂f∂y=x이다. x=4,y=−3 이면 f(x,y)=−12 가 되고, x에 대한 편미분 값은 x ∂f∂x=−3 으로 얻어진다. 이말은 즉슨, 우리가 x를 아주 조금 증가 시키면 전체 함수 값은 3배로 작아진다는 의미이다. (미분 값이 음수이므로). 이 것은 위의 수식을 재구성하면 이와 같이 간단히 보여 줄 수 있다 ( f(x+h)=f(x)+h∂f(x)∂x ). 비슷하게, ∂f∂y=4, 이므로, y 값을 아주 작은 h 만큼 증가 시킨다면 4h 만큼 전체 함수 값은 증가하게 될 것이다. (이번에는 미분 값이 양수)
  - $f(x,y)=x+y$의 경우는 편미분값이 ∂f∂x=1, ∂f∂y=1
  - $f(x,y)=max(x,y)$의 경우는 편미분값이 ∂f∂x=1(x>=y)  ∂f∂y=1(y>=x)이다. x=4,y=2 이면 max 값은 4 이고, 이 함수는 현재의 y 값에 영향을 받지 않는다. 바꾸어말하면, y값을 아주 작은 값인 h 만큼 증가시키더라도 이 함수의 결과 값은 4로 유지된다. 물론 y값을 매우 크게 증가 시킨다면 (예를 들면 2이상) 함수 f 값은 바뀌겠지만, 미분은 이런 큰 변화 값과는 관련이 없다. 미분이라는 것이 본래 그 정의에도 있듯(limh→0) 아주 작은 입력 값 변화에 대해서 의미를 갖는 값이기 때문이다.

## 연쇄 법칙을 통한 복합 표현식 (Chain rule)
- 연쇄 법칙은 이러한 그라디언트 표현식들을 함께 연결시키는 적절한 방법이 곱하는 것이라는 것을 보여준다.
- 수학적으로 연쇄법칙은 두 함수를 합성한 합성 함수의 도함수에 관한 공식이다.  
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/be86019d87b8b668b1429293a6faeb4dfb5b7b2a)  
 ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/d89a3475a6e57343c921475b16b9d41b60c98304)
- $f(x,y,z)=(x+y)z$를 생각해보자. x+y를 q로 치환할 수 있고 ∂f∂z=q ∂f∂q=z가 된다. 중간값인 ∂f∂q, 즉 q의 Gradient 값은 신경 쓸 필요가 없다. ∂f∂x=∂f∂q*∂q∂x로 표현할 수 있다.
- 수식 표현
  ```
  # set some inputs
  # f(x,y,z)=(x+y)z
  x = -2; y = 5; z = -4

  # perform the forward pass
  q = x + y # q becomes 3
  f = q * z # f becomes -12

  # perform the backward pass (backpropagation) in reverse order:
  # first backprop through f = q * z
  dfdz = q # df/dz = q, so gradient on z becomes 3
  dfdq = z # df/dq = z, so gradient on q becomes -4
  # now backprop through q = x + y
  dfdx = 1.0 * dfdq # dq/dx = 1. And the multiplication here is the chain rule!
  dfdy = 1.0 * dfdq # dq/dy = 1
  ```
- [dfdx,dfdy,dfdz] 변수들로 그라디언트가 표현되는데, 이는 f에 대한 변수 x,y,z의 민감도(sensitivity)를 보여준다.
- dfdq 대신에 단순히 dq를 쓰고 항상 그라디언트가 최종 출력에 관한 것이라 가정하자.
- 연쇄 법칙을 통해 게이트는 이 그라디언트 값을 받아들여 모든 입력에 대해서 계산한 게이트의 모든 그라디언트 값에 곱한다.
- 연쇄 법칙 덕분에 이러한 각 입력에 대한 추가 곱셈은 전체 신경망과 같은 복잡한 회로에서 상대적으로 쓸모 없는 개개의 게이트를 중요하지 않은 것으로 바꿀 수 있다.

## 연쇄 법칙 예시
![](/images/2017/01/Selection_004.png)
- 녹색은 input으로 전방 전달되고 (오른쪽으로)
- 붉은색은 녹색이 전달된 후 후방 전달로 backpropagation을 수행한다. q의 Gradient는 dq가 된다.
- Gradient의 값은 거꾸로 흐르는 것을 확인할 수 있다.
- 거꾸로 흘어햐 하는 중요한 핵심 이론이 있다. 각 gate의 기울기는 기본적으로 결과를 증가시키도록 만든다. 예를 들어 기울기가 -4라면 4의 힘으로 결과를 줄인다. 위의 예제에서는 q의 gate가 -4의 힘으로 줄어들어야 최종값을 키울 수 있다. 그러나 x+y의 +게이트는 기본적으로 q를 증가시키는 방법을 찾게 된다. 따라서 앞 기울기인 -4의 정보를 반영할 필요가 있게 되고 이것이 backpropagation의 원리이다.

## Backpropagation
- Backpropagation은 네트워크 전체에 대해 반복적인 연쇄 법칙(Chain rule)을 적용하여 그라디언트(Gradient)를 계산하는 방법 중 하나이다.
- 기울기를 게이트의 출력을 높이기 위한 방향으로 입력 값에 미치는 힘 또는 끌어당김으로 생각할 수 있다. 각 게이트는 전체 회로에 대해선 전혀 알지 못하고 자기 자신만을 고려한다. 입력이 들어오고 게이트는 출력과 입력에 대한 기울기를 계산한다.
- 중요한 것은 앞선 게이트로 부터 이 게이트에 미치는 어떤 힘이 있다는 것이다. 바로 현재 게이트의 출력에 대해 최종 회로의 출력의 기울기이다.
- 하나의 게이트만 있는 경우와 여러개의 게이트가 상호작용하여 복잡한 수식을 계산해야 하는 경우의 차이는 오직 각 게이트에서 곱셈 연산이 하나 추가되는 것뿐이다.
- 게이트들이 포함된 전체 회로의 세세한 부분을 모르더라도 완전히 독립적으로 Gradient 값들을 계산할 수 있다. (그냥 미분값이므로)
- 결국 전체 회로의 마지막 출력에 대한 게이트 출력의 그라디언트 값을 학습할 것이다.
  - 최종 f값이 크게 만드는 것이 목적이라고 가정해보자.
  - 덧샘이기 때문에 초기 Gradient는 1이었다.
  - 하지만 Backpropagation된 지역적 Gradient는 -4 바뀌었다. 이는 덧셈의 출력이 -4로 낮아져야 한다는 것을 의미한다.
  - 이 그라디언트 값을 받아들이고 이를 모든 입력들에 대한 지역적 그라디언트 값에 곱한다 (x와 y에 대한 그라디언트 값이 1 * -4 = -4가 되도록)
  - 다시 계산해보면 ((-2*-4) + (5*-4))는 -12가 되므로 -4배로 낮아진 것을 확인할 수 있다.
  - 이 덧셈 게이트의 출력은 감소할 것이고 이는 다시 곱셈 게이트의 출력이 증가하도록 만들 것이다.
- 연쇄법칙 덕분에 이러한 각 입력에 대한 추가 곱셉은 복잡한 NN회로에서는 상대적으로 무시 가능하다
- 설계에 앞서 쉬운 예제부터 시작하고 직접 write out 하는 것이 도움이 될것이라고 함.
- 드디어, 다음 강의 부터 NN의 정의와 Backpropagation이 주는 효율적 계산에 대해서 알아보려함.
- 보다 큰 최종 출력 값을 얻도록 게이트들이 자신들의 출력이 (얼마나 강하게) 증가하길 원하는지 또는 감소하길 원하는지 서로 소통하는 것이 Backpropagation이다.

## Pattern of backpropagation
![](https://tensorflowkorea.files.wordpress.com/2016/09/hackers-nn-3.png?w=625)
- 어떻게 회로에서 역방향으로 전파되는지 패턴을 볼 수 있다.
- + 게이트는 항상 앞의 기울기를 받아 입력쪽으로 그냥 통과시킨다. 이 두 입력들의 기울기 공식이 입력 값에 상관없이 그냥 +1 이기 때문에 체인 룰을 적용하면 전달받은 기울기에 1을 곱하므로 전달 받은 값 그대로가 됨.
- 입력에 대한 max(x,y) 의 기울기는 x, y 중 더 큰 쪽은 +1 이고 나머지 하나는 0이다. 이 게이트는 역전파되는 동안 기울기를 선택적으로 보내는 역할을 함. 즉 앞에서 받은 기울기를 정방향일 때 큰 값을 가진 입력으로 보내게 됨.

## Sigmoid 함수 예제
![](https://s0.wp.com/latex.php?latex=f%28x%2C+y%2C+a%2C+b%2C+c%29+%3D+%5Csigma%28ax+%2B+by+%2Bc%29&bg=ffffff&fg=444444&s=0)
- 이 함수는 입력 값을 0에서 1 사이의 값으로 구겨 넣기 때문에 일종의 압축(squashing) 함수다.
- 아주 큰 음수는 0에 가까운 값이 되고 큰 양수는 1에 가까운 값이 된다.
![](https://s0.wp.com/latex.php?latex=%5Csigma+%28x%29+%3D+%5Cdfrac%7B1%7D%7B1+%2B+e%5E%7B-x%7D%7D&bg=ffffff&fg=444444&s=0)  
![](https://tensorflowkorea.files.wordpress.com/2016/09/sigmoid.png)
- 기울기 공식과 그래프는 아래와 같다.  
![](https://tensorflowkorea.files.wordpress.com/2016/09/sigmoid-gradient.png)  
![](https://s0.wp.com/latex.php?latex=%5Cdfrac%7B%5Cpartial+%5Csigma+%28x%29%7D%7B%5Cpartial+x%7D+%3D+%5Csigma+%28x%29%281+-+%5Csigma+%28x%29%29&bg=ffffff&fg=444444&s=0)
- 게이트에 대해 하나씩 forward 메소드를 실행하고 그리고 나서 반대 방향으로 모든 게이트의 backward 메소드를 실행할 것  
- 위 수식을 표현하기 위해 +, \*, sig(시그모이드)도 필요
  - Unit
    ```javascript
    // Unit은 회로 그림의 선에 대응합니다
    var Unit = function(value, grad) {
      // 정방향에서 계산되는 값
      this.value = value;
      // 역방향일 때 계산되는 이 유닛에 대한 회로 출력의 변화율
      this.grad = grad;
    }
    ```
  - 곱셈
    ```javascript
    var multiplyGate = function(){ };
    multiplyGate.prototype = {
      forward: function(u0, u1) {
        // 입력 유닛 u0, u1 과 출력 유닛 utop 의 포인터를 저장합니다.
        this.u0 = u0;
        this.u1 = u1;
        this.utop = new Unit(u0.value * u1.value, 0.0);
        return this.utop;
      },
      backward: function() {
        // 출력 유닛의 기울기를 받아 곱셉 게이트의 자체 기울기와 곱하여(체인 룰)
        // 입력 유닛의 기울기로 저장합니다.
        this.u0.grad += this.u1.value * this.utop.grad;
        this.u1.grad += this.u0.value * this.utop.grad;
      }
    }
    ```
  - 덧셈
    ```javascript
    var addGate = function(){ };
    addGate.prototype = {
      forward: function(u0, u1) {
        this.u0 = u0;
        this.u1 = u1; // 입력 유닛의 포인터를 저장합니다
        this.utop = new Unit(u0.value + u1.value, 0.0);
        return this.utop;
      },
      backward: function() {
        // 입력에 대한 덧셈 게이트의 기울기는 1 입니다
        this.u0.grad += 1 * this.utop.grad;
        this.u1.grad += 1 * this.utop.grad;
      }
    }
    ```
  - 헬퍼함수
    ```javascript
    var sigmoidGate = function() {
      // 헬퍼 함수
      this.sig = function(x) { return 1 / (1 + Math.exp(-x)); };
    };
    sigmoidGate.prototype = {
      forward: function(u0) {
        this.u0 = u0;
        this.utop = new Unit(this.sig(this.u0.value), 0.0);
        return this.utop;
      },
      backward: function() {
        var s = this.sig(this.u0.value);
        this.u0.grad += (s * (1 - s)) * this.utop.grad;
      }
    }
    ```
  - 정방향 계산
    ```javascript
    // 정방향 계산
    var forwardNeuron = function() {
      ax = mulg0.forward(a, x); // a*x = -1
      by = mulg1.forward(b, y); // b*y = 6
      axpby = addg0.forward(ax, by); // a*x + b*y = 5
      axpbypc = addg1.forward(axpby, c); // a*x + b*y + c = 2
      s = sg0.forward(axpbypc); // sig(a*x + b*y + c) = 0.8808
    };
    forwardNeuron();
    ```
  - 역방향 계산
    ```javascript
    s.grad = 1.0;
    sg0.backward(); // axpbypc 에 기울기 저장
    addg1.backward(); // axpby 와 c 에 기울기 저장
    addg0.backward(); // ax 와 by 에 기울기 저장
    mulg1.backward(); // b 와 y 에 기울기 저장
    mulg0.backward(); // a 와 x 에 기울기 저장
    ```
    시작은 s.grad가 1이다. 마지막 게이트를 +1 의 힘으로 잡아 당기는 것처럼 보면 된다.
  - Gradient를 Value에 반영 (Step size)
    ```javascript
    var step_size = 0.01;
    a.value += step_size * a.grad; // a.grad 는 -0.105
    b.value += step_size * b.grad; // b.grad 는 0.315
    c.value += step_size * c.grad; // c.grad 는 0.105
    x.value += step_size * x.grad; // x.grad 는 0.105
    y.value += step_size * y.grad; // y.grad 는 0.210

    forwardNeuron();
    console.log('역전파 적용한 후 회로의 출력: ' + s.value); // 0.8825 출력
    ```
    - Step size의 곱으로 gradient를 value에 반영하면 최종값이 증가되는 지 볼 수 있다.
  - 마지막으로 변경된 value를 대입하여 역전파 알고리즘이 정확하게 계산된 것인지 확인
  - 이 sigmoid예제는 단일 뉴런에 대한 것이지만 이 코드는 어떤 수식(매우 복잡한 수식도)의 기울기도 계산할 수 있도록 일반화 시킬 수 있다.


# 신경망 설명
- 기존의 선형분류섹션에서는 s=Wx식의 계산이었으나, 신경망에서는 s=W2max(0,W1x)의 계산을 한다.
- W1은 100.3072의 행렬로서 이미지를 100차원짜리 중간단계로 벡터로 전환하는 정도로 생각해볼 수 있다. 0이하의 값은 0으로 막아버린다.
- 비선형성이 계산되어 결적이라는 점에 주목. 비선형성이 없다면 행렬들은 서로 겁해져서 하나의 행렬이 되고 예측 스코어도 역시 그냥 선형 함수가 된다.
- 이 비선형성이 wiggle을 찾을 수 있게 하고 W2,W1는 확률 그라디언트로 학습시키고 그라디언트들은 연쇄법칙과 backpropagation으로 계산하여 구한다.

# 뉴런 하나 모델링
- 생물학점 뉴런에서 그 의미를 빌려왔다.
- 86 billion neurons
- 10^14 - 10^15 synapses
- Coarse model.
- Single neuron as a linear classifier
