

## 검색, 다운로드, 확인
```bash
$ docker search tensorflow # 검색
$ docker pull gcr.io/tensorflow/tensorflow # 다운로드
$ docker images # image 확인
$ docker stop ubuntu # 정지
$ docker restart ubuntu # 재시작
$ docker rm ubuntu # 삭제
$ docker rm -f ubuntu # 삭제
$ docker kill ubuntu # 삭제
$ docker rmi ubuntu # 삭제
```

## 생성, 실행
생성

```bash
$ docker run --name=ubuntu ubuntu # 생성
$ docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow
$ docker run -itp 8888:8888 gcr.io/tensorflow/tensorflow
```

- ternsorflow container내부에서 notebook이 8888 port에 맞춰져 있다는 것. 만약 port 1234를 이용해서 붙이고 싶다면 1234:8888이 되어야 한다. 형식은 "호스트포트:컨테이너포트"
- '-i -t -p' --> '-itp' 순서가 변경되어도 OK
- 한번 run으로 실행하면 stop 될때까지 background에서 돌고 있는 것으로 보인다. kitematic이나 docker terminal을 사용하던지 stop할때까지 계속 돌고 있다. 'docker ps'로 확인할 수 있다. 적어도 tensorflow image의 경우는 그렇다.



실행중인 접속

```bash
$ docker exec -it  <container name> bash #접속
$ docker attach ubuntu # 접속만 함
$ docker ps 실행중인 container
```
- '-it'는 image와 terminal을 뜻하는 줄 알았지만 아님 i는 interactive를 의미하고 attach하지 않아도 STDIN를 유지하는 것 t는 tty 할당
- attach로만 붙으면 shell도 실행되지 않음.


폴더연결
```bash
$ docker run -v $HOSTDIR:$DOCKERDIR
$ docker run -d -p 3000:3000 -v /path/to/my/downloads:/downloads jpillora/cloud-torrent

```
'-v' 옵션을 사용한다.

복사와 출력
```bash
$ docker cp ./a.jpg tensor:/root/# 컨테이너 내의 파일을 호스트로 복사한다.
$ docker export # 컨테이너 파일 시스템을 tarball로 출력한다.
```

docker help
```bash
$ docker <command> --help
```

## shourcut
  설명  |  단축키
:--------:|:------:
container를 살리고 shell로 빠져나옴 | ctrl + p + q
