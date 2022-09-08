---
title: Virtualization
author:
  name: owner
  link: https://github.com/psy0231
date: 2022-08-31 12:00:00  +0900
categories: [Grind, Stub]
tags: [virtualization, docker, hypervisor ]
---

## init

- docker를 하면서 궁금했던것들인데  
같이 쓰기에 좀 애매해서 뺌
- 찾은데가 많아서 대충 버무림
- 근데 찾은데를 안적어놓음…
- 여기 들어가는 내용들은  
docker, wsl과 관련있음.

## Virtualization

- 컴퓨터에서 컴퓨터 리소스의   
추상화를 일컫는 광범위한 용어  
- "물리적인 컴퓨터 리소스의 특징을   
다른 시스템, 응용 프로그램, 최종 사용자들이   
리소스와 상호 작용하는 방식으로부터 감추는 기술"  
- 이것은 다중 논리 리소스로서의 기능을 하는 것처럼 보이는  
서버, 운영 체제, 응용 프로그램, 또는 저장 장치와 같은  
하나의 단일 물리 리소스를 만들어 낸다.  
아니면 단일 논리 리소스처럼 보이는 저장 장치나 서버와 같은  
여러 개의 물리적 리소스를 만들어 낼 수 있다.  
- 크게 하드웨어 가상화, os수준 가상화,   
데스크톱 가상화, 응용프로그램 가상화,   
네트워크 가상화가 있음  
- 이 중 하드웨어 / os 수준 가상화만 씀.

## Hardware Virtualization (Hypervisor)

- Hypervisor는 호스트 컴퓨터에서 다수의 운영 체제를  
동시에 실행하기 위한 논리적 플랫폼을 말한다.  
- Virtualization는 개념적인거 Hypervisor는 구현체정도?  
- 아무튼, HW Virtualization방식에 따라 Hypervisor가 조금씩 다름  
- 크게 `Type 1`, `Type 2`, `전가상화`, `반가상화`로 분류함  

### Type 1

```
|                            |
|    OS   |   OS   |    OS   |
|                            |
+----------------------------+
|                            |
|         HYPERVISOR         |
|                            |
+----------------------------+
|                            |
|           H / W            |
|                            |
       
            Type1

```

- native, 혹은 bare-metal이라고도 부른다.
- Hypervisor가 하드웨어에서 직접 실행되는데  
위의 이름이 붙은 이유임.
- 게스트 운영 체제는  
하드웨어 위에서 2번째 수준으로 실행된다.  
- 이 방식으로는   
Adeos, CP/CMS, 하이퍼-V, KVM,  
LDoms / 오라클 VM 서버 포 SPAR,  
CLynxSecure, SIMMON, VM웨어 ESXi,  
VM웨어 인프라스트럭처, 젠, XtratuM, z/VM  
- HW가상화 Type1에는 가상화 방식에 따라   
전가상화 / 반가상화 두 가지 방식이 있다.  

### 전가상화(Full-Virtualization)

- Hypervisor는   
os마다 다른 명령체계를 해석하기위해  
DOM0라는 가상 머신을 실행하고   
이 DOM0가 모든 명령에 개입한다.  
- guest os는 DOM0으로 명령을 전달하고  
DOM0은 명력 해석 후 Hypervisor로 전달,  
Hypervisor는 하드웨어에 최종 명령을 전달하는식.  
- 이 방식은 하드웨어의 완전한 가상화로  
guest os의 수정이 필요없지만  
DOM0가 할일이 많아져 반가상화에 비해 느림.  
- 위 그림에서 보면    
os1이 한국어, os2가 일본어, os3이 중국어를    
각각 쓴다고 했을때    
 Hypervisor는 영어를 쓰는 HW로    
모든 언어를 영어로 번역하는 역할  

### 반가상화(Para-Virtualization)

- 전가상화의 DOM0의 역할을   
각 guest os에서 직접 처리하는방식.  
- Hyper Call이라는 인터페이스를 이용해  
Hypervisor와 직접 연결.  
- 이때 guest os에는   
Hyper Call에 대한 정보가 없으므로  
geust os에 구현해야함.  
- 따라서, 전가상화보다 성능이 좋지만    
guest os의 커널을   
직접 수정해야 하는 경우가 생기며   
이가 불가능한 os도 있음.  
- 아무튼 ”쉽지않음”  
- 반가상화는    
HW, Hypervisor가 여전히 영어를 쓰고있을떄   
각각 os가 영어를 배우고 직접 번역.  

### Type 2

- 일반 프로그램과 같이 호스트 운영 체제에서 실행  
- VM 내부에서 동작되는 게스트 운영 체제는  
하드웨어에서 3번째 수준으로 실행된다.
    
    ```
    |                  |
    |    OS   |   OS   |
    |                  |
    +------------------+
    |                  |
    |    HYPERVISOR    |
    |                  |
    +------------------+
    |                  |
    |        OS        |
    |                  |
    +------------------+
    |                  |
    |      H / W       |
    |                  |
            Type2
    
    ```
    
- Host형 가상화라고도 함.  
- 일반적으로 쓰는 vm이 이 형태  
- 애초에 os가 먼저 설치되어  
이것저것 귀찮음이 없어 접근성이 좋다.  
- 다만, 무겁다.  
- VMware Server, VMware Workstation,  
VMware Fusion, QEMU,   
마이크로소프트의 버추얼 PC와 버추얼 서버,  
Oracle(SUN)의 버추얼박스,   
SWsoft의 Parallels Workstation과  
Parallels Desktop이 있다.  

## OS 수준 가상화(operating system level virtualization)

- 운영 체제의 커널이   
하나의 사용자 공간 인스턴스가 아닌,  
여러 개의 격리된 사용자 공간 인스턴스를  
갖출 수 있도록 하는 가상화 방식.  
- 이러한 인스턴스들은   
container, software container, Virtual Engine,   
jail(FreeBSD jail, chroot jail)등 으로 부르며  
소유자와 사용자의 관점에서    
실제 서버인 것처럼 보이게 한다.  
- docker는 이 기반으로 만들어졌다.  
- 호스트 OS위의 Application layer 에서  
개개의 Application 형태로 동작한다.  
이 개개의 application을 컨테이너라고 하며  
호스트 OS위에서 동작한다.  
- 프로그래밍 언어에서 볼 수 있는  
namespace, class instance 같은 느낌.  
또는 게임에서 보는 서버안에 개별 채널같은?  
- HW Vitualization 이랑 확연히 다른점은  
application layer에서 동작한다는점으로  
OS를 통으로 구동하는게 아닌  
그 환경을 기반으로 구동되는  
application을 실행한다는점  
- 그래서 이 전에 vm을 쓰던걸 생각해서  
linux image받고 실행해보면  
바로 끝나는게 이런 차이때문  
- application 처럼 실행하기때문에  
container의 생성, 삭제가 쉽고  
기능상으로 갈갈이 찢기 좋게된듯  
- 개개의 컨테이너는 OS Kernel을 공유하여  
동작하기 때문에 호스트 OS와 컨테이너 OS는 같다.  
    - 컨테이너 os가 같은게 싫거나  
    커널공유로 인한 보안상의 이유로  
    위 type2형 vm위에서 구동하는 경우도 있나봄.  
- 하나의 컨테이너 내에서 환경변수를 설정했을 땐  
다른 컨테이너에서 동일하게 먹히지 않는다.  
서로 격리된 환경이기때문에.


## Docker
- 컨테이너 기반의 가상화 도구.
- 가상화 설명으로 서론이 길었던건 위 한줄 이해를 위해..  
- 그림으로 보면 `type2`랑 비슷한 모양일것같은데  
hypervisor가 docker로, 최상층 os가 container로.  
    
    ```
    |                       |
    | container | container |
    |                       |
    +-----------------------+
    |                       |
    |         docker        |
    |                       |
    +-----------------------+
    |                       |
    |           OS          |
    |                       |
    +-----------------------+
    |                       |
    |          H / W        |
    |                       |
            docker
    ```
    이렇지 않을까 싶다.
- docker for windows를 설치할 떄  
두가지 설치 가능한방법이 있는데  
wsl2를 이용한 방법과 Hyper-V를 이용한 방법.  
wsl2를 추천한다고 써있음. 

## etc
> 뇌피셜 공간.  
> 및 풀리지 않은 의문점들.

- 시작에 docker, wsl과 연관있다 했는데  
정확한건 잘 모르겠고 찾을때  
이렇다더라 한게 있었는데  
굳이 필요한 내용은 아닌것같음.
### wsl1 - wsl2
  - 좋은 비유는 아니지만 굳이 해보자면  
  wsl1은 type2같은 형태,  
  wsl2는 type1같은 형태의 구조.  
  
    좋은 비유가 아니라고 한건 wsl1로  
    사실은 api 방식이었나 했는데 컷.

  - wsl2에서 아래깔린 hypervisor는  
  hyper-v로 윈도우에서 type1로  
  제공되는 hypervisor라고 한다.
  - 아무튼 wsl, wsl2를 써보면  
  성능은 wsl2가 좋다고 하는데  
  체감상은 vmware쓰다가 이거쓰면  
  그저 신세계임.
  - 다만 확연히 느낄 수 있는점은  
  wsl1에서는 network, file등  
  신경쓸게 딱히 없었는데  
  내 기억속엔 그럼.  
  다시 보기 귀찮으니까 컷  
  wsl2는 network가 내부망처럼 동작하고  
  file을 직접 까볼수 없게된것 등  
  ($wsl로 보면 되는데 컷.)

    암튼 미묘하게 다른데   
    wsl2은 커널을 공유하게되는 형태로 되면서  
    이런 구조를 갖게되는경우  
    보안상의 이유로 가상환경과  
    실제 host환경사이 벽이 쳐지는것같음.  
    docker도 그렇고.

### in windows
- 윈도우에서 기능 켜기/끄기를 보면  
관련되어 보이는게 몇개 보임.
  - Hyper-V
  - linux용 windows 하위 시스템
  - 가상 머신 플렛폼
  - windows 하이퍼바이저 플렛폼
  - windows 샌드박스
- 이 중에 사용중인 기능은  
`linux용 windows 하위 시스템`,  
`windows 샌드박스`, `가상 머신 플렛폼`
- 여기서부터 잘 모르겠는데  
내가 실제 쓰는건 wsl, win sandbox,  
win docker가 있음.  
그러면 hyper-v가 사용중일줄 알았는데  
막상보니 그렇지 않음.  
또 가상 머신 플렛폼이 껴있음..
- 생각해보면 wsl2설치 과정에서 명령어가  
  ```
  dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
  dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  ```
  이 두개였음 .  
  애초에 hyper-v는 필요 없었나?
- win sandbox는 어떤걸 쓰는거임?

- **hyper-v** : 

  Microsoft의 hypervisor

- **virtual machine platform** : 

  hypervisor를 계속 지원하는  
  덜 강력한(?) Hyper-V.  
  VMP는 WSL2에 대한 요구사항.  
  App-V 또는 MSI를위한 MSX 애플리케이션  
  패키지를 생성하는 데 사용될 수 있음.

- **Windows Hypervisor Platform** :

  Windows hypervisor에서  
  가상화 소프트웨어가 실행되도록 설정.  
  
  전체 Hyper-V 없이도  
  가상화된 애플리케이션(App-V 같은)을  
  windows에서 실행할 수 있다.  
  
  타사 개발자가 Hyper-V를 사용하기 위해  
  사용할 수 있는 API스택.  
  
  타사 가상화 스택 및 애플리케이션을 위한  
  확장 사용자 모드 API를 추가하여  
  hypervisor 수준에서  
  파티션을 생성, 관리하고  
  파티션에 대한 메모리 매핑을 구성하며,  
  가상 프로세서의 실행을 생성하고 제어.
  
  Oracle VirtualBox, Docker 및  
  QEMU가 이러한 프로젝트의 예.

- **Windows Subsystem For Linux** : 

  Windows 내에서 Linux 명령을 실행.

- 여기저기서 적당히 짜집어다가 적음  
- 관련 문서는 ms 가상화 문서에서.  
- 또 찾다보니까 나오던데  
hyper-v를 이용해 linux를 돌리면  
내가 지금 쓰는 wsl이랑 많이 다름.  
wsl보다 그게 진짜 os통으로 올라가는 느낌?  
  
  hyper-v를 쓰면 vm류를 못쓴다하던데  
  난 잘 쓰고있음.  
  이 외에도 몇가지 불편한게 있다던데    
  난 그런게 없는거 보니  
  wsl이 hyper-v에서 쓰던걸  
  vmp로 바꾸고 지금 처럼 된게 아닌가 싶음.

- 암튼 여긴 정리할건 아니고  
정리 될것같지도 않고  
쓴것도 애매한것도 많고  
다시 볼일이 있겠나 싶음.

## 참고
- [가상화 문서](https://docs.microsoft.com/en-us/virtualization/)