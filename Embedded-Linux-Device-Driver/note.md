## Buffer 전략
1. Bufferred I/O 방식: user와 kernel 영역에 독립된 메모리를 확보, 속도 느림 (copy_to_user() copy_from_user())
2. Direct I/O 방식 : kernel영역의 물리메모리를 user 가상주소로 매핑하여 접근, 고속전송가능, mmap() 사용
3. Neither I/O 방식 : user영역의 메모리를 직접접근, 권장하지 않는 방법, sysfs과 procfs 드라이버에서 사용됨

## 문자 드라이버 등록 함수 사용방법
  * register_chrdev() : 주번호/부번호를 고정하여 등록
  * misc_register() : 주번호/부번호 설정하지 않고 등록

## misc_register()함수의 동작 메카니즘
주번호는 10, 부번호는 동적으로 할당하여 커널에 문자드라이버를 등록후 sysfs에 기록후 uevent를 발생시킨다

결과물: /sys/dev/char/10:39/uevent

## Kernel Source
misc_register():misc.c-->device_create()-->device_create_vargs()-->device_register()-->device_add()--> 
device_create_file()(-->sysfs_create_file():sysfs생성),
kobject_uevent()-->kobject_uevent_env()-->netlink_broadcast_filtered():netlink socket송신

## ueventd
### netlink socket수신
1. do_coldboot()사용 :부팅시 생성된 장치를 검사하여
                       장치 파일을 생성
2. 부팅이후의 추가되는 장치의 이벤트를 폴링하여
            장치 파일을 생성
    ```
    mknod(path, mode, dev);  // mknod /dev/sample c 10 39
    rm /dev/sample
    ```
### netlink socket
: 커널에서 발생하는 장치의 이벤트를 ueventd 나 udevd 에게 전달하는 가상 socket struct uevent 형태로 전달

참조 예제: /system/core/init/uevent.c 

커널함수 : kobject_uevent()
 
## linux 의 VA 맵: 32bit
```
---------------0xFFFFFFFF
 kernel(1G)
---------------0xC0000000(x86)       0xBF000000(ARM)
                 0xBFFFFFFF               0xBEFFFFFF
  user(3G)

---------------0x0
```

## 드라이버 학습 개요(학습 단계)
1. 문자(character) 드라이버 : 응용프로그램과 드라이버간의
 호출 인터페이스를 구현,주번호/부번호, 장치 파일,시스템콜
  번호 관리의 문제점(번호 충돌) ==> misc driver사용

2. LDM(Linux Driver Model) : bus중심, 통합관리(PnP,Power)
    ```
       bus/device/driver 객체  ==> kobject
                         객체의 view : sysfs
       platform_bus----platform_device[platform_driver]
                         |__platform_device[platform_driver]
                         |__platform_device[platform_driver]
    ```
3. LDM : class 중심, class용 device객체

4. LDM : sysfs을 저수준으로 제어

        * 응용프로그램과 통신
        * netlink socket 사용: uevent를 송신
        * firmware event처리



## 문자(character) 드라이버의 동작구조
 : 응용프로그램이 드라이버를 호출할수 있는 방법

* 장치 파일 생성
 ```
 mknod /dev/mydrv c 240 0
 ```
  * struct inode: (디스크에 저장된 파일시스템정보를  갖는다)
    ```
    dev_t	i_rdev ;// [주번호(12bit) | 부번호(20bit) ]
                     :   240             : 0 
    ```
  * struct file: open()시 생성,close()시 소멸

* 드라이버 설치
    ```
    insmod mydrv.ko ==> 커널의 240번 테이블에 file_operations구조체를 등록
    ```
* 응용프로그램 실행
    ```
       ./test_mydrv
     int fd = open("/dev/mydrv",O_RDWR); // "swi 5"
      -->kernel mode(svc32):시스템콜 --> sys_open():
      file객체를 생성,주번호 240을 얻고 
      240번 file_operations을 얻어서  file객체에 저장
      -->mydrv_open()호출
    
      read(fd,buf_in,MAX_BUFFER);// "swi 3"
      -->kernel mode(svc32):시스템콜 --> sys_read()
      -->mydrv_read()호출
      
      write(fd,buf_out,MAX_BUFFER);// "swi 4"
      -->kernel mode(svc32):시스템콜 --> sys_write()
      -->mydrv_write()호출
      
      close(fd);// "swi 6"
      -->kernel mode(svc32):시스템콜 --> sys_close()
     -->mydrv_release()호출-->sys_close():file객체를소멸
    ```
