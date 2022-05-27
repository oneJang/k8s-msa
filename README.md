<클라우드 융합 이론 정리>
===

## [중간고사]

1. 가상화: 컴퓨터의 물리적인 자원들을 추상화 하는 기술
2. 하이퍼바이저: 가상화를 해주는 소프트웨어
2-1) kvm: 리눅스에 포함되어있는 커널 모듈, 실제 cpu의 부분을 가상화된 cpu와 매핑
2-2) QEMU: 하드웨어를 가상화 해주는 하이퍼바이저중 하나
3. Livbert: 하이퍼바이저들을 관리하는 오픈소스 라이브러리

VM vs 컨테이너
VM: 하이퍼바이저로 하드웨어를 가상화하여 만든 공간
컨테이너: 컨테이너 엔진으로 os를 가상화하여 만든 별도 공간
VM은 공간 하나당 guest os를 설치해야하기 때문에 무거움
반면 컨테이너는 os를 공유하기 때문에 응용프로그램과 라이브러리만 있으면 됨 -> 가벼움

수많은 서버가 모여 있는 센터: 데이터 센터
수많은 가상머신과 컨테이너가 모여있는 센터: 클라우드
서버들 속의 하이퍼바이저, 컨테이너 엔진이 만드는 가상머신과 컨테이너를 운영, 관리하는 기술을 공부하는 것 -> 클라우드 기술을 학습하는 것

http(hypertext transfer protocol): 웹 정보를 주고받는데 사용되는 프로토콜
주요특성: stateless -> 서버는 이전에 클라이언트가 request했던 것에 대한 정보를 기억하지 않는다
http 메세지: request, reponse
request: ex) GET, POST, PUT, DELETE ...
reponse: ex) 200 ok

api: 서버와 클라이언트 사이의 인터페이스
ex) 음식점
클라이언트: 주문하는 사람
api: 직원
서버: 주방장

restful api: http의 방식으로 api를 사용하는 것
각 자원을 주고받을 때 URI로 식별, http의 모든 특성을 갖추고 있음

openstack: restful api를 이용한 virtual machine 관리용 클라우드 운영 시스템, 수많은 오픈소스 프로젝트들이 모여 구성됨
1) nova
- 하이퍼바이저를 관리하는 api제공
- vm을 생성하고 관리
- 제어기능이 contol Node에 위치하여 compute node관리(compute node: vm이 만들어지는 서버)
2) storage
- ceph: 오픈스택과는 관련이 없고 저장소에 집중한 프로젝트
3) glance
- cd rom같은 기능으로 생각하면 편함
- vm위에 올릴 os의 데이터를 가지고 있음
4) keystone
- 사용자 인증을 통하여 물리 서버 내의 자원을 사용할 수 있도록 관리
5) horizon
- 오픈스택을 쉽게 이용할 수 있게 해주는 gui서비스
6) neutron(제일 어려움)
- 네트워크 관련 작업을 해줌

## [기말고사]

컨테이너: 하나의 host os위에서 여러 개의 프로세스가 '고립된 공간'에서 동작하는 구조
진짜 컨테이너 박스를 생각하면 이해하기 쉬움

초기의 컨테이너: 리눅스 컨테이너
리눅스 컨테이너에서 격리에 사용되는 개념)
namespace: 사용되는 다양한 변수 등등을 name으로 구분
Cgroup: cpu, 메모리 같은 자원을 격리

도커: 가장 대표적인 컨테이너화 기술, 관리데몬(containerd)와 런타임(runc)을 사용하여 격리

vm vs 컨테이너<br>
| 항목 | 컨테이너 | 가상머신  
| ------ | ---- | ---- |
| Guest OS | X | O |
| 커널자원 분리 | X | O |
| 시작 및 종료시간 | 빠름 | 느림 |
| 자원 효율성 | 높음 | 낮음 |

컨테이너의 장점
1) 애플리케이션의 빠른 배포
- 컨테이너 형식이 표준화됨
- 개발 테스트와 수정이 빠름
2) 설치와 확장이 쉽고, 이식성이 좋음
- 다양한 인프라에 설치가능
- 컨테이너 규모 확장 및 축소가 쉽고 빠름
3) 오버헤드가 낮음
- 하이퍼바이저 x
- 서버 자원을 더 효과적으로 사용
4) 관리가 쉬움
- 수정된 부분만 적용

수많은 컨테이너들을 관리할 툴이 필요(컨테이너 오케스트레이션이 필요)<br>
컨테이너 오케스트레이션 대표주자: 쿠버네티스<br>
쿠버네티스의 기본단위: 클러스터<br>
클러스터 - 마스터 노드(control-plane)1개이상, 여러개의 worker node로 구성되어있음<br>
클러스터의 가장 작은 단위: pod<br>
마스터 노드)
- api server: 클러스터에서 일어나는 모든 일들을 받아들이고 전달하는 통로
- etcd: 워커노드의 모든 상태 정보들을 키:값 형태로 저장함, 칠판과 같은 역할
- 스케줄러: 요청된 작업을 어느 노드에 배치시킬지 결정
- controller manager: 다양한 컨트롤러 관리(ex. pod생성/복제/배포...)
- coredns: pod의 ip를 도메인 이름으로 바꿈
워커 노드)
- kubelet: api server와 통신, pod의 상태 감시
- kube proxy: 특정 컨테이너와 통신하고자 할 경우 프록시의 iptables을 확인하여 접속하게 함
- 컨테이너 엔진: 컨테이너를 관리
pod)
- 컨테이너: 실제 기능을 하는 컨테이너가 한개(or 한개 이상) 있음
- 스토리지: 컨테이너의 정보를 저장
- 네트워크: 컨테이너와의 통신을 담당

쿠버네티스의 대표적 특징
1) 선언적
- 명령이 아니라 선언을 함 -> yaml
- 현재 상태(current status)를 그 선언한 형태(desired status)로 변경
2) 마스터 노드는 명령을 하지 않음
- 모든 노드들은 api server를 항상 주시(watch)하고 필요한 사항을 적용
3) 기존 응용프로그램을 수정없이 돌릴 수 있게 함
- 응용프로그램의 민감한 정보는 secret형태로 api server에서 관리하는데 기존 응용프로그램에 api server로 
가서 민감한 정보를 가져오라는 코드를 추가해야함 -> 기존 코드를 수정
pod에 민감한 정보를 저장하는 remote storage와 프로그램과 연결하면 수정할 필요없음
4) pod의 이식성을 강화
- 스토리지를 어떻게 쓸지 정의하면 어디서든 portable하게 사용가능
