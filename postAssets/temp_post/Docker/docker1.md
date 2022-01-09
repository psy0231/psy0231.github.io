## Docker

- 컨테이너 기반의 가상화 도구.

### 가상화 
- 물리적인 하드웨어 자원을 논리적인 리소스로 재공하기 위한 기술

- 이때 하이퍼바이저가 필요한데  
호스트 컴퓨터에서 다수의 운영 체제를  
동시에 실행하기 위한 논리적 플랫폼을 말한다.

- 다시, 하이퍼 바이저는 두가지 방식으로 나뉜다.
  - Type 1

    운영 체제가 프로그램을 제어하듯이  
    하이퍼바이저가 해당 하드웨어에서 직접 실행되며  
    게스트 운영 체제는  
    하드웨어 위에서 2번째 수준으로 실행된다.

           |                  |         
           |        OS        |         
           |                  |         
           +------------------+         
           |                  |         
           |    HYPERVISOR    |         
           |                  |         
           +------------------+
           |                  |         
           |      H / W       |
           |                  |         
                 Type1

    Xen, XenServer, ESX Server, L4 마이크로커널, TRANGO, POWER 하이퍼바이저(PR/SM),  
    하이퍼-V, 패러럴서버, 썬의 로지컬 도메인 하이퍼바이저 등이 있다. 

  - Type 2

    일반 프로그램과 같이 호스트 운영 체제에서 실행되며  
    VM 내부에서 동작되는 게스트 운영 체제는  
    하드웨어에서 3번째 수준으로 실행된다. 

           |                  |         
           |        OS        |         
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
    
    VM의 대표적인 종류는 VMware Server, VMware Workstation, VMware Fusion, QEMU,  
    마이크로소프트의 버추얼 PC와 버추얼 서버, Oracle(SUN)의 버추얼박스,  
    SWsoft의 Parallels Workstation과 Parallels Desktop이 있다.

- 컨테이너(container)형
    
    컨테이너형은 좀 독특한 형태.  
    호스트 OS위의 Application layer 에서  
    개개의 Application 형태로 동작한다.  
    이 개개의 application을 컨테이너라고 하며    
    호스트 OS위에서 동작한다.
    
    개개의 컨테이너는 OS Kernel을 공유하여   
    동작하기 때문에 호스트 OS와 컨테이너 OS는 같다.
    
    하나의 컨테이너 내에서 환경변수를 설정했을 땐  
    다른 컨테이너에서 동일하게 먹히지 않는다.

## 침고

- https://www.joinc.co.kr/w/man/12/docker
- https://github.com/remotty/documents.docker.co.kr
- https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html


===
https://www.notion.so/b67ed727aea4467cbc3226bb0c8e8336
https://medium.com/@darkrasid/docker%EC%99%80-vm-d95d60e56fdd
https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html
https://github.com/remotty/documents.docker.co.kr
https://www.joinc.co.kr/w/man/12/docker
https://ko.wikipedia.org/wiki/Cgroups
https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81%ED%99%94
https://ko.wikipedia.org/wiki/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%B0%94%EC%9D%B4%EC%A0%80
https://ko.wikipedia.org/wiki/%EB%B0%98%EA%B0%80%EC%83%81%ED%99%94
https://ko.wikipedia.org/wiki/%EC%99%84%EC%A0%84_%EA%B0%80%EC%83%81%ED%99%94
https://renuevo.github.io/docker/docker-structure-windows10/
https://itholic.github.io/hypervisor/
https://blog.naver.com/alice_k106/220218878967
https://blog.naver.com/complusblog/220990379931
https://docs.microsoft.com/ko-kr/dotnet/architecture/microservices/container-docker-introduction/docker-defined
https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/manage-containers/hyperv-container
https://www.44bits.io/ko/post/why-should-i-use-docker-container
https://likefree.tistory.com/24