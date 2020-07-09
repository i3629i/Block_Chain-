# Hyperledger Fabric

## 1. 비트, 이더 vs Fabric의 차이
1. 비트와 이더는 Public 개인키 기반, Fabirc는 Private 인증서 기반으로 //
2. 비트와 이더는 모든 역할을 한 프로그램이 진행
3. Fabirc같은 경우는 Orderer(블록을 생성하는), Peer(블록체인 보관) 역할을 나누어서 수행
4. Fabric은 중간자 CA라고 하는 기관이 존재
5. Fabric에선 이전 해시 값을 더해서 이후 블록을 연결하는 방식이 아니다.

## 2. Fabric 구조
1. Peer - 블록체인 보관, 거래 트랜잭션을 시뮬레이팅
2. Orderer - 블록 생성(찍어내기만 함) 
3. App(유저) - 

![image](https://user-images.githubusercontent.com/50629716/86989142-c032be80-c1d4-11ea-825d-38974a391cc6.png)
1. User가 Fabric에서 거래를 만듬
2. 만든 거래를 Peer에 보내 허락을 받아야함, User -> Peer 제안을함
3. Peer -> User 거래가 좋다고 판단시 Ok // 투표방식 3명중 2명이 OK면 Orderer에 블록생성을 할 수있는 허락을 받음.
4. Orderer는 허락받은 거래를 모아 블록생성 // 안에든 내용은 보지 않고 진행
5. Block생성후 Peer에게 보냄

## 3. Ledger
1. 블록체인
2. World Stat(Peer보관) // Peer만 소유하고 있음. 

## 4. 구조
![image](https://user-images.githubusercontent.com/50629716/86989841-3daafe80-c1d6-11ea-9c3c-cb083556f7d6.png)


## 5. Lefger 방식 Raad Set, Write Set
* 한번 Wirte 한 키에 대해서 Read하면 Fail처리함.
* Peer가 블럭을 만들지 않다보니 블럭의 해시값이 중첩이 될 수 있음. // 음.. 블럭 생성자
* 예를 들어서 거래에 대해 PeerA PeerB등이 수락을 완료하고 Orderer에게 검증 값을 보낼 때 트랜잭션의 주소(?), 키(?) 값이 중첩이 될 수 있음, 그래서 먼저 들어온 키값에 대해 Write를 했을 때 다음 트랜잭션의 키값이 같아 버리게 되는 경우가 생김


## 6. Fabric Network setting
* orderer, App, Peer
1. Ledger - 블록체인
2. Chain Code - 스마트 컨트랙트 별로 각자 다른 도커 인스턴스를 가진다..? , Peer소유
3. CA, Fabric-CA

4. Organization R
5 CC : Channl Configuration
6. Nc : Network Config
7. Channel
8. Fabric-CA, CA - App, Peer의 인증서등을 발급, 재발급, 폐기, 갱신등 해주는 곳
![image](https://user-images.githubusercontent.com/50629716/86998545-93d66c80-c1eb-11ea-947c-f9a495784eb8.png)
* Channel에 가입되어 있는 Peer들끼리 공유가 가능 // 같은 스마트 컨트랙트를 소유해야함
* Organization - 네트워크의 한 그룹

### Network 작업 순서
![image](https://user-images.githubusercontent.com/50629716/87000506-2973fb00-c1f0-11ea-884d-cabcbf1ee44c.png)

## 7. Fabric Identity

### PKI(Public Key Interface)
* 공개키 암호화 
  * 개인키 + 공개키 // 공개키로 암호화하고, 개인키로 복호화
  * 개인키는 소유자만 비밀로 보관하고 있어야함. 
* 전자서명
  * 데이터가 위/변조 되지 않았다는 것을 PKI시스템에 알려주기 위한 시스템
  * A의 개인키로 암호화한 문서를 받았을 때 B가 공개키로 복호화를 진행했을 때 결괏값이 같게 나온다면 A는 문서에 대한 부인을 할 수가 없음.
![image](https://user-images.githubusercontent.com/50629716/87001881-7ad1b980-c1f3-11ea-83d6-4162a5ff824f.png)
  * 단, 전자서명은 전자서명에 사용된 공개키가 송신자의 공개키인지 증명할 방법이 없음. 그래서 CA(Certification Authority)라는 인증 기관을 이용해 신뢰할 수 있는 안전한 공개키를 제공
  
* 인증서(X.509) 표준
  * 공개키 인증서란? 사용자의 공개키 + 사용자의 식별정보를 추가해 만드는 일종의, 전자 신분증 같은 역할.
  * X.509 인증서에 저장된 정보
    * 표준 
    * 일련번호 - 발급한 인증 기관내의 일련번호
    * 서명 알고리즘 - 발급할때 사용한 알고리즘 Ex. SHA1, SHA2, RSA
    * 발급자 - 발급한 인증 기관의 이름
    * 유효 기간 - 인증서 시작일 만료일
    * 주체 - 인증서 소유자의 DN이름
    * 공개키 - 인증서의 모든 영억을 해시해 인증 기관의 개인키로 서명한 값
* Fabric 인증서

* MSP(Membership Service Provider)
  * Fabric에서 노드/유저/채널 등등, 서로간의 인증, 권한 관리를 해주는 기본적 개념
  * 실제로 노드(Peer, Orderer, User)들은 MSP안에 인증서를 가지고 인증하고 검증함.
  * 채널/네트워크 ->다른 여러가지 방법을 통한 멤버쉽 관리
  


