* String은 수정이 안됨 (immutable 속성)
  * Network용도로 사용하려면 Byte로 변환해야 함.
  * b’abc’
  * ba = ‘abc’.encode()
  * ‘abc’.decode()
  * ba[0] = ord(‘A’) #ord는 아스키 코드로 바꿔주는 메소드임.
* Python은 Memeory 관리는 reference count를 통해 한다. 이것이 0이 되면 자동으로
소멸 시킨다.
  * sys.getrefcount(myList)
* 하나의 원소를 가지는 Tuple은 (10,) 콤마를 줘야 한다.
* Dictionary는 순서없는 데이터 타입으로 Index를 사용할 수 없다. Tree 구조를
사용하는 듯
* 삼항 연산자
  * n > 0? 100:200 (C형태)
  * 100 if n>0 else 200 (Python if이용) (else가 꼭필요함)
  * result = (‘a’,’b’)[n>0] (Python Turple이용)
  * result = {True:‘a’,False:’b’}[n>0] (Python Dictionary이용)
* [n for n in range(4)]
* {k:v for k,v in dict()}
* for n in list:
print(n)
if n==5:
break;
else:
print (‘else’) # break로 빠졌는지 for횟수를 모두 채워 빠져나왔는지 알 수 있음
* enumerate(list): Dictionary로 만들어줌.
* ­­와 ++는 없다. ++4 == +(+4) 일뿐
* if 0<n<10 연산자 가능 C에서는 (0<n) 후에 1<10을 연산 하기 때문에 안됐음.
* // 연산자는 몫 연산자이다. 그냥 / 연산자는 몫이 아니라 실수가 나온다.
* Dictionary format
  * Key가 반드시 문자열이어야 함.
  * print(“이름:%(name)s 나이:%(age)d” % mydic)
* format
  * 자료형을 맞춰주지 않아도 알아서 들어감.
  * a, b = 4,’abc’
  * s = “a={0} b={1}”.format(a,b)
  * s = “a={0:10} b={1}”.format(a,b) 열칸 잡고
    * a= 4 b=abc
  * s = “a={0:10} b={1:>10}”.format(a,b) b를 오른쪽으로 정렬 시키면서 10칸
    * a= 4 b= abc
  * string은 기본이 왼쪽 정렬이니까.
* function이 list를 받으면 reference 연산으로 값을 바꾸면 함수를 빠져나와도 값
바뀐다. 그러니까 값복사가 아니라 Reference 복사라는 것.
  * def fn(b):
b[0] = 100 # 진짜 값이 바뀜
b = [1,2,3]
fn(b)
print(b) # [100,2,3]
* 함수 내에서 전역 변수를 바꾸려면 함수내에서 global로 선언
* global이 builtin 심볼을 Overwrite하지 않도록 해야함.
  * global이 builtin 보다 더 우위에 있음
  * str = ‘abc’
  * 이후부터는 str() 함수를 사용 못함. (전역이면 못쓰고, 지역이면 그 지역
범위에서만 못씀)
* 인자를 모두 Turple로 받을 수 있다.
  * def fn(*arg): # arg=(10,20,30) 이렇게 됨.
  * fn(10,20,30)
* 인자를 모두 Dictionary로 받을 수 있다.
  * def fn(**arg): # arg=(name=”aa”,age=32) 이렇게 됨.
  * fn(name=”aa”,age=32)
  * fn(**myDic) #이렇게 넘길 수도 있음.
* 가변 인자와 Dictionary 인자는 뒤로 넘겨야 한다. 그래야 아다리가 맞는다.
  * def fn(a,*b,**c):
  * fn(10,3,4,’a’=’10’,’b’=’str’) #a=10, b=(3,4) c={’a’=’10’,’b’=’str’}
* switch­case 문 쓰기
  * myDic ={1:fn, 2:fn1}
  * myDic[1]() # fn()과 같음.
* 라이브러리의 종류
  * 내장 라이브러리(함수, 클래스, 전역변수) import 필요없음
  * 내장 모듈(import) os, sys
  * lib폴더에 존재하는 library (Python에서 제공)
  * 내가 만든 Library, 3rd party
* 미리 컴파일 해두는 방법
  * python ­m py_compile a.py
