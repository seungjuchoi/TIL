# Tensorflow
## Intro
- Deepmind의 DQN과 Google의 Tensorflow는 다르다.
- Opensource이지만 다 공개하진 않았다. 학습데이터는 공개도 되어 있지 않음. 즉 돈되는 수준의 공개는 아직 아니다.
- Python, C, C++ 중에 하나 선택, 그러나 사실 Python.

## Deep learning
별거없고 Machine learning + Neural Network 정도. 사전적으로는 hidden layer가 2개이상인 learning 방법

## Machine learning
- 통계학적으로는 추측 통계학

### 지도 학습
- Training set, Desired output with **labeled data**
- 비용 함수 (Cost Function)
  - 표준 편자 계산
  - 주로 회귀 (Regression)
  - 학습에 사용하지 않는 검증 데이터 셋에 대해 오류 발생
- 비유: 학교에서 배운 내용들로만 사회를 살아갈 수 있을까?
- 개발자: 할일이 있다. Modeling과 Data 준비 등

### 비지도 학습
- Learning without labeled data
- 군집화 (Aggregation) / 분류 (Classification)
- 예를 들어 적절한 분류는 사용자에게 상품 추천을 가능하게 한다.
- 개발자: 할일이 없다. 단순한 learning 상태 확인정도, 학교에서 단순한 Logic을 배워야하는 시대는 끝날지도 모름.

## Neural Network
- 비선형 구조를 해결할 수 있다.

## Deep learning
- ML의 한 분야
- NN,CNN,Deep brief 등등

## Tensorflow Code 구현
### 자료형
tensor는 tensorflow의 자료형이고 연산을 위해서는 이것으로 wrapping이 필요하다. 즉, python 자료형을 Tensorflow 자료형으로 바꾸는 작업이다.

아래 3가지로 정의할 수 있다. 사실 numpy의 array가 가지는 속성이다.
- rank (차원) dimension
- shape (행과 열의 길이) 예) 3x4
- type (데이터 형식)

#### Type의 종류
```python
x = tf.constant(3, name="x")
y = tf.Variable(x+9,name=y)
ph = tf.placeholder("uint8", [None,None,3])
ph = tf.placeholder("int32")
```
tf.placeholder는 변수를 저장하는 공간을 의미하고 자료형만 명시하고 data를 넣지 않을 수도 있다.  

### Session
- Client 프로그램(ex. python environment)이 Tensorflow 런타임 시스템과 통신하기 위해서 세션이 생성되어야 한다.
- session이 생성되어 실행되기 전까지 수행되지 않는다.
- print는 python 자료형을 출력하기 위한 method다. 연산에 대한 결과를 보려면 session을 만들어 수행하고 print해야 한다.
> 사전적 의미: 여러 개의 connection을 묶어서 부르는 말이다. connection 보다는 상위 개념이라는 정도로 알고 있자.

### Kernel
일반적인 의미와 달리 tensor에서는 '명령어를 구현한 것'이라고 쓰인다.
예를 들어 gpu kernel과 cpu kernel이 있다고 함.

### Dataflow graph
- Node는 산술 연산자 (+,-,/,\*)
- Edge는 Tensor라고 명명된 데이터 집합을 의미하며 피연산자
- Dataflow를 실행 관점(GPU, CPU)으로 나눠서 설계해야 한다. Multi-thread가 프로그램에 영향을 미치는 것 때문에 동시성 문제가 발생하고 semaphore등을 사용해야하는 것처럼 Tensorflow에서도 core에 대한 부분을 Model 설계시 고려해야한다는 의미이다.
- Server가 한대인 경우에는 Thread 설계로 가능하나 Server가 여러대인경우에는 Scalable를 고려해야 함. (병렬 computing: 하둡, Tensorflow)

#### Edge 종류
- 일반 Edge
  - 입력값이 Tensor이고 하나의 명령어(Node)에 대한 출력값은 다른 명령어의 입력값이 된다. Tensor의 전달 정도로 생각하자.
- 특수 Edge
  - 두 노드 간의 제어 의존성을 정의함
  - 연산의 우선 순위를 표현


## Tensorflow code
1차원 텐서
```python
import numpy as np
tensor_1d = np.array([1.4,1,3.0,23.99])
print(tensor_1d.ndim)
print(tensor_1d.shape)
print(tensor_1d.dtype)
tf_tensor = tf.convert_to_tensor(tensor_1d,dtype=tf.float64)
with tf.Session() as sess:
    print(sess.run(tf_tensor))
    print(sess.run(tf_tensor[1]))
    print(sess.run(tf_tensor[2]))
```
numpy array를 바로 tensor로 wrapping할 수 있다.

2차원 텐서
```python
mat1 = np.array([(1,2,3),(4,5,6),(4,5,6)],dtype='int32')
mat2 = np.array([(5,6,7),(7,8,9),(4,5,6)],dtype='int32')
mat1 = tf.constant(mat1)
mat2 = tf.constant(mat2)
mat_mul = tf.matmul(mat1,mat2)
mat_sum = tf.add(mat1,mat2)
with tf.Session() as sess:
    print(sess.run(mat_mul))
```
- 참고로 행렬의 곱을 할 수 없는 행렬인 경우 Error가 발생한다.
- dtype을 int32로 생성하는 이유는 default가 int64이고 이는 지원하는 dtype이 아니기 때문이다. float16, float32, float64, int32, complex64, complex128 중에 하나여야 한다.

### bmp를 활용한 3차원 image crop 예제
```python
import tensorflow as tf
import matplotlib.image as mp_image
import matplotlib.pyplot as plt

filename = 'packt.jpeg'
input_image = mp_image.imread(filename)

my_image = tf.placeholder("uint8",[None,None,3])

slice = tf.slice(my_image,[10,0,0],[16,-1,-1])

with tf.Session() as sess:
    result = sess.run(slice,feed_dict={my_image:input_image})
    print(result.shape)
plt.imshow(result)
plt.show()
```
- pilow package가 필요했다.
- plt는 show함수가 호출되어야 출력된다.
- feed_dict를 이용해서 runtime시 parameter를 전달할 수 있다.

### 복소수 표현
```python
import tensorflow as tf
x = 5. + 4j
print (x)

x = complex(5,4)
print (x)
print (x.imag)
print (x.real)
```
complex를 이용하거나 더하기 j방법을 이용하거나

### 데이터 모델 함수 예제
```python
import tensorflow as tf
import numpy as np

number_of_points = 1000

x_point = []
y_point = []

a = 0.22
b = 0.78

for i in range(number_of_points):
    x = np.random.normal(0.0,0.5)
    y = a*x + b + np.random.normal(0.0,0.1)
    x_point.append([x])
    y_point.append([y])

plt.plot(x_point,y_point, 'x', label='Input Data')
plt.legend()
plt.show()
```
![](/images/2017/01/normal_distribution.png)
- plt.plot의 사용법에 주목하자.


## 인공 신경망
각 노드는 인공 뉴런이라고 부르고 매우 간단한 기능을 가지고 있다. 시그널의 총 양이 활성화 함수에 정의된 활성화 임계치를 초과하면 활성화 상태가 된다. 활성화가 되면 시그널을 전달 경로에 따라 연결된 다른 유닛으로 전달한다.

### 신경망 구조
- 각뉴런은 다음계청의 모든 뉴런들과 연결된다.
- 같은 계층에 속한 뉴런끼리는 연결되지 않는다.
- 해결하고자 하는 문제에 따라 계층 수와 한 계층의 뉴런 수가 달라진다.

### 단일 계층 Perceptron
첫 신경망 도멜로 1958년에 프랭크 로젠블랫에 의해 제안됐다. 가중치의 벡터(W)로 구성이되는데 계산과정은 입력 벡터와 가중치 벡터의 개체와 곱한 것을 모두 합하는 것이다. 그리고 입력 벡터를 두 부류로 구분한다.
![](/images/2017/01/perceptron.png)  
f는 활성화 함수가 된다. 비선형 함수를 선호하는 경향이 있다.

### 로지스틱 (Sigmoid) 회귀
일반적인 선형 회귀로는 해결할 수 없는 분류 문제를 해결하려는 알고리즘을 말한다. Logistic 혹은 Sigmoid 함수를 사용하는데 Sigmoid는 아래와 같은 패턴을 가진다. (동의어: Binary Classification) [참고링크](https://github.com/seungjuchoi/TIL/blob/master/DL-ML/cs231n%EC%A0%95%EB%A6%AC.md#sigmoid-함수-예제)  
![](/images/2017/01/Logistic-curve.svg.png)  
로지스틱 회쉬는 특정 개체가 어떤 클래스에 속할 확률을 알려준다. 신경망으로 학습한 지도 학습은 가중치를 최적화 하는 반복 프로세로 구성된다.
> 참고로 Softmax는 두 개 이상의 그룹을 나누기 위해서 binary classification를 확장(사실 반복)한 알고리즘이다.

#### 학습의 기본적인 단계
1. 가중치는 학습 시작시에 랜덤하게 가져온다.
2. 학습 데이터 집합의 각 객체 마나 원하는 결과값과 실제 결과값에 대한 계산을 한다. 이것을 Error라고 하고 이것은 다시 Weight값을 조절하는 데 반영된다.
3. 모든 학습 데이터 집합을 랜덤한 순서로 모델에 입력하고 에러가 임계치의 미만이거나 미리 정의된 최대 반복 수를 넘어가면 정지시킨다.

### 다중 계층 Perceptron
단일 Perceptron를 여러개 연결한 구조로 Input 계층이 바로 Output 계층과 연결되지 않고 Hidden계층을 거친다. (Hidden layer가 두개 이상인 모델을 Deep learning이라고 한다.)   
![](images/2017/01/multi-perceptron.png)  
이 MLP에 대한 알고리즘은 일반적으로 역전파를 사용한다. (Backpropergation)

> 역전파 알고리즘은 신경망에 대한 알고리즘으로 계산된 차이에 기초하여 가중치를 수정하고 출력값들을 단계적으로 수렴시킨다.

MLP에서 주목해야하는 것은 은닉계층의 뉴런의 기대 결과값을 모르더라도 경사 하강법 기술을 사용해 에러 함수를 최소화하는 것을 토대로 항상 지도 학습 방법을 적용할 수 있다는 점이다.
또한, 입력값과 출력값을 정의되어 있지만 은닉 계층의 수와 계층별 은닉 뉴런의 수를 결정되어 있지 않다. 유사 예제를 통해 경험으로 판단할 수 밖에 없다.  아래 두가지를 고려해야 한다.
- 은닉 계층의 수가 증가되면 학습시 연결되어야 하는 노드 수가 증가되어야 하고 학습 데이터 집합의 크기도 증가되어야 한다. 이는 학습 속도를 떨어뜨린다.
- 은닉 계층에서의 노드가 너무 많다면 학습 데이터 집합에만 국한되어 학습되기 때문에 일반화가 어렵고 너무 적다면 학습이 아예 안될 수 있다.

### 다중 계층 Perceptron 함수 추정
학습 단계에서 x와 f(x)로 구성된 집합을 이용한 신경망을 학습시키고 x가 주어졌을 때 f(x)를 추정하도록 해보자. 하나의 은닉 계층으로도 만들 수 있다.

## 딥 러닝

### CNN을 통한 image 인식
Convolution 계층과 Neural network 계층의 합을 CNN이라고 하고 Convolution 계층에서는 Feature extraction이 수행된다. 이 과정은 NN로 들어가기위한 전처리 과정인데 데이터 량을 줄이고 필요한 부분만 강조할 수 있다는 장점이 있다.

#### 구조
지역 수용 영역(local receptive fields), 합성곱, 풀링 의 세단계로 구성되어 있다.  
데이터의 일부분이 다른 부분과 연관성이 있다고 가정하고 있다. 예를 들어 은닉 뉴런에 들어가는 입력 뉴런의 크기는 한정되어 있다는 것이다. (예를 들어 100x100 image에서 5x5만 다음 은닉 뉴런에 들어감). 이렇게 하는 이유는 입력데이터의 이미지는 특정이 한데 모여있고 연과성이 강하기 때문에 데이터에서 특이한 그룹을 쉽게 추출할 수 있기 때문이다. 이런 과정을 합성곱이라고 한다.  
합성곱을 통해 100x100은 5x5 영역으로 나누면 96x96이된다. 각각의 뉴런은 편향 bias과 영역과 연결된 5x5 가중치 weight를 갖고 있다. 이 의미는 입력 데이터마다 대상 사물의 위치가 변경돼도 첫번째 은닉 레이어의 모든 뉴런에서 그 특징을 잘 인식할 수 있다는 것을 의미한다. 이런 것을 서로 공유한다는 이유 때문에 공유 가중치 공유 편향이라고 불리기도 한다. 

#### 패턴
![](http://cfile3.uf.tistory.com/image/2723C841583ED6AC2372AD)  
왼쪽이 원본 Image, 오른쪽은 filter이다. 두 Image를 행렬화 시키고 곱하면 filter와 패턴이 비슷한지 알 수 있다. 입력값은 여러가지 패턴을 가질 수 있기 때문에 다중 필터를 적용하여 어떤 패턴을 가지고 있는 지 알아낸다.  
![](http://cfile22.uf.tistory.com/image/2739D541583ED6AD2BF6AD)  
가로 세로 필터를 적용해보는 모습  
그런데 필터는 어떻게 만들어지는 것일까? 필터는 데이터를 넣고 학습을 시키면, 자동으로 학습 데이타에서 학습을 통해서 특징을 인식하고 필터를 만들어 낸다. CNN의 신기한 기능.   
![](http://cfile30.uf.tistory.com/image/210B0A39583EDBBB052E78)  
필터의 행렬곱 계산은 위처럼 진행한다. stride법이라고 하고 이렇게 얻어낸 결과를 Activation map, Feature map 이라고 한다. 얻어진 데이터 행렬은 필터의 크기에 맞춰 줄어들게 되는데 Feature 추출을 반복할 때마다 크기가 작아지면 Feature가 유실될 수 있기 때문에 padding 기법이 필요하다.  
![](http://cfile1.uf.tistory.com/image/23083C43583ED7621D1061)  
input 계층 주위로 0을 둘러싸서, 결과 값이 작아지고 Feature가 유실되는 것을 막는다.

#### Activation 함수
생성된 Activation map(Feature map)에 Activation function을 적용해야 한다. 해당 filter의 패턴이 있는지 없는 지 확인하는 단계로 이 함수로 로지스틱 회귀에서 배웠던 sigmoid 함수를 사용할 수 있다. 간단히 설명하면 Output이 True이면 0.5~1의 값이 return되고 False이면 0~0.5의 값이 return 된다.
![](http://cfile8.uf.tistory.com/image/243EA743583ED7B1256C1F)  
그러나 Nueral network에서는 이 sigmoid 함수보다는 ReLu 함수를 Activation 함수로 대부분 사용한다.

### RNN 순환 신경망
CNN과는 달리 Node별로 같은 값인 U,V,W을 사용하는 신경망으로 순서가 있는 정보를 입력 데이터로 가진다. 일반적인 신경망은 서로 독립적이라 가정하는데 자연어 처리, 음성인식과 같은 앞뒤 순서가 중요한 상황에서는 적합하지 않다. 그래서 RNN은 앞뒤 문맥의 이해가 필요한 모델에 강하다. 구조는 아래와 같다.  
![ ](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/09/rnn.jpg)
