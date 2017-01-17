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

아래 3가지로 정의할 수 있다.
- rank (차원) dimension
- shape (행과 열의 길이) 예) 3x4
- type (데이터 형식)

#### Type의 종류
```python
x = tf.constant(3, name="x")
y = tf.Variable(x+9,name=y)
```

### Session
- session이 생성되어 실행되기 전까지 수행되지 않는다.
- print는 python 자료형을 출력하기 위한 method다. 연산에 대한 결과를 보려면 session을 만들어 수행하고 print해야 한다.
> 사전적 의미: 여러 개의 connection을 묶어서 부르는 말이다. connection 보다는 상위 개념이라는 정도로 알고 있자.

### Kernel
일반적인 의미와 달리 tensor에서는 '명령어를 구현한 것'이라고 쓰인다.
예를 들어 gpu kernel과 cpu kernel이 있다고 함.
