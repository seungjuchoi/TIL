## 대표 Framework
- Tensor Flow
- Torch
- Caffe
- Theano 테아노

## Site
- https://tensorflow.blog/1장-실수치-회로-해커가-알려주는-뉴럴-네트워크/
- http://hunkim.github.io/ml/
- https://github.com/HFTrader/DeepLearningBook
- http://www.aistudy.co.kr/
- https://tensorflow.blog/1장-실수치-회로-해커가-알려주는-뉴럴-네트워크/
- https://tensorflow.blog/2016/05/04/stanford-cs231n-%EA%B0%95%EC%A2%8C%EA%B0%80-%EB%8B%AB%ED%98%94%EC%8A%B5%EB%8B%88%EB%8B%A4/#comment-128
- http://aikorea.org/
- http://aikorea.org/awesome-rl/
- http://hackaday.com/2016/12/21/practical-deep-learning/

## Book
- Rainforcement Learning https://www.gitbook.com/book/dnddnjs/rl/details

## Lecture
- 제프리 힌튼(Geoffrey Hinton) 교수의 코세라 강좌인 Neural Netwoks for Machine Learning
- 다프니 콜러(Daphne Koller)의 Probabilistic Graphical Models

## Example code
- 텐서플로우(TensorFlow)를 이용한 ImageNet 이미지 인식(추론) 프로그램 만들기 https://solarisailab.com/archives/346

## Framework
참고 링크: http://aikorea.org/blog/dl-libraries/ (Tensorflow release 이전 투표 결과)
![](https://raw.githubusercontent.com/aikorea/aikorea.github.io/9d063d4221aaf88a7d64c71340f3962bdd6f31ef/images/DL_lib_vote.PNG)
- Theano: 수식 및 행렬 연산을 쉽게 만들어주는 파이썬 라이브러리. 딥러닝 알고리즘을 파이썬으로 쉽게 구현할 수 있도록 해주는데, Theano 기반 위에 얹어서 더 사용하기 쉽게 구현된 여러 라이브러리가 있다.
  - Keras: Theano 기반이지만 Torch처럼 모듈화가 잘 되어 있어서 사용하기 쉽고 최근에도 계속 업데이트되며 빠른 속도로 발전하고 있는 라이브러리.
  - Pylearn2: Theano를 유지, 보수하고 있는 Montreal 대학의 Yoshua Bengio 그룹에서 개발한 Machine Learning 연구용 라이브러리
  - Lasagne: 가볍고 모듈화가 잘 되어 있어서 사용하기 편리함
  - Blocks: 위 라이브러리와 비슷하게 역시 Theano 기반으로 손쉽게 신경망 구조를 구현할 수 있도록 해주는 라이브러리
- Chainer: 거의 모든 딥러닝 알고리즘을 직관적인 Python 코드로 구현할 수 있고, 자유도가 매우 높음. 대다수의 다른 라이브러리들과는 다르게 "Define-by-Run" 형태로 구현되어 있어서, forward 함수만 정의해주면 네트워크 구조가 자동으로 정해진다는 점 이 특이하다.
- nolearn: scikit-learn과 연동되며 기계학습에 유용한 여러 함수를 담고 있음.
- Gensim: 큰 스케일의 텍스트 데이터를 효율적으로 다루는 것을 목표로 한 Python 기반 딥러닝 툴킷
- deepnet: cudamat과 cuda-convnet 기반의 딥러닝 라이브러리
- CXXNET: MShadow 라이브러리 기반으로 멀티 GPU까지 지원하며, Python 및 Matlab 인터페이스 제공
- DeepPy: NumPy 기반의 라이브러리
- Neon: Nervana에서 사용하는 딥러닝 프레임워크

## Paper
- Michael Nielsen’s Neural Networks and Deep Learning http://neuralnetworksanddeeplearning.com/index.html
- http://www.iro.umontreal.ca/~pift6266/H10/notes/deepintro.html
- http://www.iro.umontreal.ca/~pift6266/H10/notes/mlintro.html
- Googlenet https://www.cs.unc.edu/~wliu/papers/GoogLeNet.pdf
