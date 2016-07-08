# QCT HVX Training
## Comment
* 성능이 좋은 데, Power 소모를 아직 측정하지 않음.
* filter가 10개 정도 됨. 예제는 Gaussian으로 할 예정
* Eclipse는 문제가 있음.
* Stream mode를 Target에서 가장 빨리 볼 수 있는 방법을 CTO에서는 가장 궁금해하고 있음.
* CTO에서는 Stream mode로 Video를 땡겨오는 것이 핵심이라고 생각
* CTO에서는 CL대비 얼마 Neon 대비 얼마의 성능을 가지고 있는지 알고 싶어함.
## Question
* Meaning of Local variable vs Gloval variable in HVX
	* Local Variable이 동작안해서 현재는 Global로 옮겨서 해야하는 상황이다.
* RGB2LAB
* filter를 5by5 filter한다는 의미
## PPT Contents
* Streaming : Computer vision test app : HQV framework
	* Streaming을 제외한 나머지 두가지는 현재 시연 가능하고 사용가능하다. 8992에서도 가능
	* Computer vision에서는 HAL이든 App이든 API호출하면 된다. 할일이 없다.
* HVX API를 java에서 바로 콜 가능 그러니까 YUV나 RGB를 가져다가 쓰자.
* HAL에서도 가져다가 콜가능하다.
* Processing은 10배 이상 증가될 것이라고 봄.
* High 다 돌아감, 8996과 8998는 되고 Middle은 점차 확산 
* 구성
	* HVX library를 만들어서 3rd app 에서 HVX API를 call
	* HVX를 통해 DSP에 던지고 ARM Core는 다른 일 처리, 병렬처리
	* DSP에 붙은 L2$에 블록 단위로 Memory를 가져다가 씀. -> Bus Traffic이 많이 줄어듬.  
	* SDK 1.2.2 + HVX Addon 1.0 조함
* 기본 용도
	* Filtering base다 전체적으로 동일한 filter로 가능
	* Face Recognition: T2T: Touch to .. 
	* Vidio Stabilization
* Simulator와 Target의 측정 결과가 거의 흡사하다 Bugpenalty를 75정도 주긴 한다.
* QCT에서도 하나씩 HVX로 Porting하고 있으면 Step by Step 중
* DSP clock이 떨어졌는데도 계산 시간이 1/5로 줄어듬
* ARM Neon은 2Ghz가 넘지만 DSP는 0.72Ghz정도 차이가 남 
* Power option: SVS, Nominal, Turbo. Turbo에서 
* 8996 V2.1에선 L2 prefetch Bug가 있음.
* Computer vision (측정 자료는 이게 전부)
	* Open CV
	* FastCV: QCT이 개선한 CV for ARM
			§ fcv란 단어를 앞에 붙여 함수 호출
	* Fast CV DSP: QCT이 개선한 CV for DSP or ARM (Filter마다 Bus load 있음) 
	* Open DSP: DSP 안에서 여러 filter를 한꺼번에 처리하고 Bus를 한번 이용해서 Memory Traffic을 줄임
	* DSP Kernel은 QXDM에서 측정한 것 Memory load를 뺀 것. (측정용)

## HexaoneDSP  Overview
* Sensor와 DSP를 같이 사용하지 않음.
* DSP가 DDR를 Acess하려면 (14Page 참고)
* HVX는 L2$만 전용으로 이용할 수 있음. (16Page 참고)
* HVX는 register 연산이 Vector로 되어 있음.
* 2pixel정도면 하는 걸 128개를 한꺼번에 계산해서 빠르다.
* Register를 Thread마다 별도로 하나씩 가지고 있음. 이유는 Rea time 때문ARM은 보통 Thread가 1개
* Clock이니까 각 Thread는 1/4밖에 못씀. 그래서 2개의 Thread를 모아서 씀. 800Mhz
	* (400Mhz -> 64B + 64B ) * 2
* HVX 전신은 Ti의 IMX다. 성공적인 Tech model이었다고 함. (Nokia <-> Ti)
* zzHDR을 구현중인데 HVX*2 Norminal로 사용중, Turbo라면 HVX 하나면 사용했을 걸, 소모전류 때문. 이경우 512kb를 모두 사용중 보통 반으로 나눠서 M2M과 Stream으로 나눠서 사용 
* Depth map을 만들려면 Streamer를 이용함.
* 하나의 line에서 모든 것을 끝내야하는 것이기 때문에 processing adventage가 높지 않을 수 없다.
* 16M이면 HVX를 2개 사용해야하고 Load도 걸리지 않을 수 없음. 
* 8998은 1Ghz 이상이 될 예정
* 현재로서는 HVX를 이용해 Stream을 하는 것이 효율적인가에 focus를 맞춰야한다고 QCT는 제안 

## Code Level
* HVX는 C complier를 사용할 수 없음.
* Interrinsics으로 coding 하고 assemble쪽으로 감.
* Stream은 L2 fetch만 있으면 된다. 
* L2_Prefetch()는 wait하지 않고 DMA로 가져옴. 그래서 먼저 1번 fetch 하고 2번 fetch 하고 동시에 1번 processing하는 것, 결국 fetch time에 processing과 fetch를 한꺼번에 함.
* 두개의 Vector pair가 하나의 Vector로 합쳐져서 Memory에 저장됨.
* 1 clock에 rounding saturation 등을 한꺼번에 처리
* shuffle은 vector를 돌려가면서 사용 
* Vector를 어떻게 사용하고, instruction은 무엇인지 Block Picture와 함께 설명하고 있음
* DSP function call을 하기 전에 clock borting이 필요.
* Multi threading
	* 보통 HVX 2개 wjdeh tkdyd
	* #1에는 1번 라인 #2에는 2번 라인 zig zag로 던저 처리
	* 얼마 단위로 던저줄지에 대한 것만 정해주면 알아서 처리해줌, 그래서 Local Variable로 셋팅해줘야 한다. Global이 안됨
	* 그런데 Local은 Debugging 안되니 참고
* Ship 안에 실행파일이 있음
* make tree V=android_release VERBOSE=1
* V가 debug_dynamic 이면 target용 debug image
* Gaussian7x7.c는 Stub For arm
* Gaussian7x7_imp.c, Gaussian7x7_C_intrinsics.c skel for DSP
* Intrinsic는 7개밖에 안쓰고 suffle로 Vector pair를 하나로 만듦
* make tree V=hexagon_debug_toolv72_v60 VERBOSE=1
* set HEXAGON_TOOLS_ROOT=.. 7.6.06
* 실행 lldb hexagon_v60/q6ss.cfg --dsp_clock 800
