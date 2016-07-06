* Python Anaconda를 통해 많이 사용하는 패키지를 모두 설치 가능
* Python 설치 대신 Python Anaconda만 설치해서 사용하는 경우도 많다.
* 그래프를 활용할 수 있음
  * import ​matplotlib.pyplot as ​mlp
* Xml file을 Data Frame으로 표현하기
  * idx = 1
  * df = pandas.DataFrame(columns=['title'​, 'singer'​])
  * for ​songElm in ​soup.findAll('song'​):
  * print(songElm.title.string)
  * print(songElm.singer.string)
  * row = dict(zip(['title'​, 'singer'​], [songElm.title.string, songElm.singer.string]))
  * rowSer = pandas.Series(row) # list로 row를 줘도 된다.
  * rowSer.name = idx
  * df = df.append(rowSer)
  * idx += 1
  * print(df)
  * Series로 만들어 넣어야 함.
* JSON의 parsing은 Dict로 변환해주는 것으로 끝난다. Dict를 사용하면 된다.
* run()을 직접 실행하면 그냥 함수 호출 start()로 호출하면 Thread로 호출
  * if ​__name__ == '__main__'​:
  * th = MyThread()
  * th.start() # vs th.run()
* thread.join() thread가 끝날 때까지 기다리는 wait 성격의 함수.
* thread 용 stack이 존재한다. 그렇지 않으면 함수처리가 동시성을 가지기 힘들다. 대신
heap을 공유한다. 이 공유되는 부분를 임계영역이라고 하고 Lock이 필요하게 된다.
  * class ​MyThread2(Thread):
  * def ​__init__(self):
  * super().__init__()
  *
  * def ​run(self): # 별도의 Stack 영역 시작
  * global ​mySum
  * for ​n in ​range(6, 11):
  * mySum += n
* Server는 Node.js가 좋다. 편리하고 강력하다.

* pyQT
  * 한번 배워두면 다른 언어의 UI도 개발할 수 있다.
  * Event는 Queue를 사용하며 Message화 된다.
  * Event를 등록해서 해당 Function이 호출 되도록 한다.
  ![Alt text](/python/pyQT.jpg)
* Event를 감시하기위한 Queue 바라보기가 시작됨. 이후 Line은 실행되지 않음.
  * app.exec() #loop queue memory
