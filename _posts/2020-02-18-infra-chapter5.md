---
title: "[스터디]인프라 엔지니어의 교과서: 5장 스토리지"
categories:
  - study
tags:
  - 공부
  - Infra
---
아래 내용은 도서 [인프라 엔지니어의 교과서](http://www.yes24.com/Product/Goods/13486433) 중 5장 `스토리지`를 읽고 요약한 글입니다.
# 5장: 스토리지
## 스토리지
* 데이터를 저장하는 장치
* 서버 내부 저장 영역인 로컬 스토리지, 외부 저장 영역인 외부 스토리지가 있음
* 외부 스토리지는 서버에 직접 연결하는 DAS와 네트워크를 통해 연결하는 NAS, SAN이 있음
### 로컬 스토리지
* 서버 내부에 디스크를 설치해서 이용하는 저장 영역
* 외부 스토리지를 사용하지 않으므로 설치 공간을 절약할 수 있음
* 외부 스토리지에 비해 디스크 개수와 확장성이 부족
### 외부 스토리지
* 서버 외부에 준비한 스토리지 장비 혹은 영역
    ### DAS(Direct Attached Storage)
    * 서버에 직접 연결하는 스토리지 장비.
    * 로컬 스토리지만으로 용량이 부족할 때 필요한 만큼 디스크 용량을 늘릴 수 있음
    * 많은 디스크를 설치할 수 있어 스트라이핑 수가 많은 RAID로 구성하면 I/O 성능이 크게 증가됨
    * OS는 DAS에 생성된 논리 드라이브를 내장 디스크의 논리 드라이브로 인식함. 즉, OS단계에서 특별히 구분할 필요가 없음
    * RAID 컨트롤러 보드를 꽂거나 HBA(Host Bus Adaptor)보드를 꽂아서 사용. 전자는 보드가 구성을 관리하지만, 후자는 스토리지에 내장된 컨트롤러가 구성을 관리함
    * 실제 용량, 성능, 내장애성 및 확장성을 고려하여 선택해야 함
    * 특히, DAS에 몇 개의 디스크를 설치할 수 있는지는 매우 중요한 조건임
    * 2.5인치, 3.5인치 소켓이 있는데 3.5가 응답 속도가 빠르고 비용이 큼. 또한 직관적으로 2.5인치가 더 작으므로 같은 부피의 기기에 더 많은 소켓이 있을 수 있음
    * DAS 인클로저를 데이지체인으로 연결해 용량을 확장할 수 있는 형태의 제품도 있음(확장성을 고려하면 좋은 선택일 수 있음)
    >>다만, 매년 하드디스크가 대용량화되면서 랙 마운트형 서버에서 탑재할 수 있는 디스크 개수도 증가하고 있으므로, 굳이 DAS를 사용하지 않고 로컬 스토리지를 선택하는 것도 고려할만함
    ### NAS(Network Attached Storage)
    * 네트워크를 통해 여러 대의 서버가 액세스할 수 있는 스토리지
    * 서버와 NAS간에는 NFS, SMB, CIFS, AFP와 같은 프로토콜을 이용하여 통신
    * PC에서는 일종의 공유 폴더처럼 사용 가능
    * 여러 대의 서버에서 데이터를 공유하거나 여러 대의 서버에서 발생하는 백업 및 로그 파일을 한 군데에 모으는 용도로 사용할 수 있음
    ### SAN(Storage Area Network)
    * 블록 단위의 데이터 스토리지 전용 네트워크. 고속, 고품질 환경을 요구하는 환경에서 좋음
    * 하나의 스토리지를 논리적으로 나눈다는 점에서 Windows에서의 파티션과 비슷한 개념
    * FC-SAN, IP-SAN이 있음
    #### FC-SAN(Fibre-Channel SAN)
    - 파이버 채널 기반으로 구축된 고속, 고품질 스토리지 전용 네트워크
    - 일반적으로 기간계 DB 등 중요한 데이터를 다루는 환경에서 이용됨
    - 서버에서는 HBA 보드를 설치해 SAN 스위치를 통하거나 SAN 스토리지를 직접 연결
    #### IP-SAN(Internet Protocol SAN)
    - L2/L3 스위치 사이에서 이더넷을 통해 통신하는 네트워크
    - 고속, 고품질 통신을 가능하게 하지만 가격이 매우 비쌈
    - 통신 부분에 이더넷을 이용해서 SAN보다 저렴하게 구축 가능
    - iSCSI 등 IP 네트워크를 통해 SCSI 커맨드를 송수신할 수 있는 스토리지가 필요함
### RAID와 핫스페어
- 스토리지 장비는 인클로저 안에 디스크를 대량으로 탑재할 수 있게 설계되어 있음
- 여러 개의 디스크로 RAID를 구성해, 큰 스토리지 영역으로 사용하는 게 일반적이며, 이를 '볼륨'이라고 부름
- 볼륨은 디스크 하나가 고장나도 RAID로 이중화한 덕에 서비스가 금방은 영향을 받지 않음
- 고장 발생시 즉시 새 디스크로 교체하면 RAID가 리빌드되기에 무중단으로 스토리지 이용 가능
- 다만, 고장난 디스크가 방치되면 RAID 구성을 깨버릴수도 있음. 이를 대비해 핫스페어가 효과적인데, 핫스페어란 다른 다른 디스크가 망가졌을 때를 대비해 대기하는 스탠바이 디스크를 이야기함.
- 핫스페어가 활성화되어 RAID 그룹 안에 들어가면 망가진 디스크는 고장 상태로 처리, 새 디스크로 교체되면 핫스페어로 들어감
- 핫스페어 할당에 제한은 없으므로, 교체가 여의치 않은 경우 핫스페어를 넉넉하게 설정해 두는게 안전함

## 외부 스토리지 이용
4가지 이유로 외부 스토리지를 고려할 수 있음
1. 저장 영역을 많이 확보하고 싶다: 데이터가 많아 로컬로 충분하지 않으면 외부 스토리지에 맡길 수 있다.
2. 디스크 I/O 성능을 높인다: 로컬 디스크의 디스크 I/O 성능이 충분하지 않으면 외부 스토리지를 이용해 디스크 I/O 성능을 향상시킬 수 있다. 예를 들어 실용량 3TB를 3TB 스토리지 1개와 300GB 스토리지 10개의 RAID를 고려하면, 후자가 이론상 10배 정도의 디스크 I/O를 가진다.
3. 스토리지를 통합해서 집중 관리한다: 중요한 데이터가 서버별로 분산되어 있으면 관리가 어려워진다. 또한 여러 스토리지마다 불필요한 여분이 생긴다. 외부 스토리지는 이런 단점이 없어 운영 비용을 낮출 수 있다.
4. 복수의 서버에서 데이터 공유: 여러 서버에서 같은 데이터와 소스 코드를 읽고 쓰거나, DB 등을 같이 사용할 수 있다.

## 스토리지의 고급 기능
* 스토리지의 중요성이 높아지면서 스토리지에도 많은 변화가 있었다.
### 씬 프로비저닝(Thin Provisioning)
* 물리 스토리지 용량보다 많은 논리 볼륨을 할당하는 기능. 일반적으로 물리 스토리지 용량을 상한선으로 할당할 수 있다.
* 용량 부족으로 장애가 일어나지 않게 하기 위해 이용하는 경우가 많다.
* 논리 볼륨을 많이 만들면 각 논리 볼륨에서 여분으로 확보한 용량이 쌓여 낭비가 될 수 있다. 이럴 경우 씬 프로비저닝을 통해 할당한 용량만큼의 물리 스토리지를 준비하면 투자 비용을 절감할 수 있다.
* 가상 서버 환경처럼 게스트 OS마다 논리 볼륨을 만드는데 특히 효과적으로 작동한다.
### 자동 계층화
* 서로 다른 성능의 디스크를 조합해서 데이터의 이용 빈도에 따라 다른 장비에 저장하는 기능이다.
* 미리 특정 규칙을 명세하거나 각 파일의 이용 상황을 스토리지가 판단해서 자동으로 적절한 계층으로 이동시키는 방법 등 다양한 방법으로 구현된다.
### 디둡
* 스토리지를 백업할 때, 먼저 저장된 데이터가 있으면 그 데이터는 복사하지 않는 기능이다. '중복 제거 기능'이라고도 부른다.
* diffing algorithm을 이용하여 구현되는 경우가 많고, 스토리지 용량과 시간을 상당히 절약할 수 있다.
* 대부분의 최신 제품은 디둡과 더불어 데이터 압축 기능을 탑재하고 있다.
### 스냅샷
* 어떤 순간의 파일 시스템의 정지점을 순간적으로 복사한 뒤 보존해 두는 기능이다.
* 파일이 갱신될 때마다 갱신 이력과 갱신 전 파일을 스탭샷용 스토리지 공간에 기록하는 방법이 일반적이다.
* 갱신할 때만 생기므로 효율적으로 이력을 관리할 수 있다.