* package load 만으로 하위 py를 import 하고 싶다면
  * __init__.py안에 __all__ = ['com11', 'com22'] 를 적어줌
* python의 class
  * 선언: java는 new CC(), python은 new 빼고 CC()
  * 모두 public임.
  * 멤버 함수에는 C++의 this처럼 필요함.
  * Overloading이 없다. Overridiing은 있다!
* 왜 self가 필요한가?
  * 클래스 함수 내에서 클래스 직속 변수나 함수에 접근할 수 있는 방법이 없기
때문에 self.xxx의 구조가 필요하다.
  * interpreter가 클래스의 함수를 호출할 때 self를 암묵적으로 넣어줌
class.method(self,a,b)
  * python의 visual basic만 self를 사용하고 나머지는 this다. 개념은 동일
* 별도의 선언없이 메소드 안에서 바로 self.a 를 사용가능.
* 일반 함수 안에서 생성된 클래스 객체는 스택의 일반 변수에 주소값을 저장하게 되고
함수가 끝나자마자 일반 변수가 소멸되면서 클래스 객체를 가르키는 Reference
Count가 Zero되면서 소멸된다. 그렇게 하지 않으려면 적어도 Return이라도 해줘야
한다. (Reference Count가 Zero면 언제든지 소멸된다.)​class의 소멸자로 소멸시점을
바로 알수 있다.
* 클래스 메소드의 내용을 Pass로 만들어 추상함수화 할 수 있다. 인터페이스와 같은
의미로...
  * class Animal: # 인터페이스
def Live(self): # 추상함수
pass
delf die(self):
pass
* 상위 클래스를 상속했을 때 상위 클래스의 __init__함수는 불리지 않는다. 생성자니까
그 클래그의 객체가 생성될 때 불린다.
* python ­m unittest a.py 바로 단위테스트 클래스의 main()를 호출 하도록 하는 커맨드
* C Module 부착
  * “extest”와 file명을 맞춰줘야 함.
  * PyMODINIT_FUNC
  * PyInit_extest(void)
  * {
  * return PyModule_Create(&extestmodule);
  * }
* C module에서 Input과 Output을 다양한 형태로 사용가능하다. (list나 tuple로 return
가능)


* scons: Python을 활용한 Make Build 환경을 만드는 Tool.
  * 2.7에 최적화 되어 있다.
  * Build 대상은 Java나 C다. Python이 아니다.
  * Build 구조도
![Alt text](/python/SConstruct.jpg)
* SConstruct 파일은 Python Source File Type이다. print(“red”)를 추가로 작성해도 scons 명령어 수행시 동작함을 알 수 있었다.
* easy_install로 설치한 package의 경우 egg 파일이 site­packages에 설치되며 별도의
Path설정(파일명까지)을 해야 사용 가능한 불편함이 있다.
* sqllite
  * 사용시 마다 connection을 유지하지 않고 사용후 바로 close하는 것을
추천하고 있음.
  * q="insert into student(name,age,birth) values(?,?,?)"
* MySQL
  * q="insert into student(name,age,birth) values(%s,%s,%s)"
  * %s는 sqllite의 물음표와 대응된다. 자료형은 따지지 않는다.(알아서해주는 듯)
