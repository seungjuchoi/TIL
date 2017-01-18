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
