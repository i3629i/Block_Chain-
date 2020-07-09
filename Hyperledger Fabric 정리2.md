# Hyperledger Fabric

## 1. Channel
 
### 네트워크 채널
* 특정 멤버들(Peer, User, Organization)끼리만 블록체인과 스마트 컨트랙트를 공유가능.
* 예를 들어 채널 "A" 에있는 멤버들은 "B"에 있는 채널을 접근 할수없음, 공유불가능
  * 그러나, 예를들어 Peer1이 채널 A,B 둘다 가입 되어 있을 수 있음, 둘 다 접근이 가능함.
<br>

* 채널은 MSP, Identity에 의해 식별이 가능.
* 채널은 각 채널 별로 블록체인이 따로 있음
  * 채널을 만들 때 Genesis Block에 채널의 정보가 있는 트랜잭션으로 시작.
  * 처음에 각 Peer들이 Genesis Block의 정보를 보고 채널에 접근
 <br>
 
* 채널에 피어들이 블록을 공유할 때
  * 리더피어 - 가장 먼저 orderer와 연결해 블록을 가져옴  //리더피어가 가장먼저 블록의 값을 받아 다른 피어에게 브로드캐스트함
  * 앵커링 피어 - 네트워크 에서 서로다른 채널에 존재하는 피어들의 정보같은 것을 공유하게 됨. // 네트워크 정보들을 공유
  
 
## 2. Gossip Protocol

### Peer 역할 종류
* Committer
  * 모든Peer는 Committer
  * 블록체인을 가지고 있고 , Excute, Validate하는 역할
 
* Anker Peer
  * 자신의 Organization의 네트워크 정보를 가짐, 이를 전달해 채널에 존재하는 피어들을 알 수 있음.
 
* Leader
  * Orderer로부터 블록을 받아와 다른 피어들에게 브로드캐스트 //Admin이 정하거나 Peer들끼리 Leader를 정할 수 있음.
  
### 앵커 피어(?)
* 채널을 만들 오더러 옵션을 통해 알림
* 부트스트랩 피어 설정으로 인해 시작시 특정 피어랑 연결하라고 설정

### 리더 피어
* 환경변수에서 원하는 피어를 설정
* 다이나믹하게 선언 - ID가 높은 Peer가 리더가 된다.

### 오더러 연결
* Config를 통해서 연결(genesis.block) 흠..

## 3. Event
* SDK를 이용해 Peer로 이벤트를 수신, grpc를 통해 이벤트를 받음
![image](https://user-images.githubusercontent.com/50629716/87010584-b96e7080-c201-11ea-93a4-c3d1cd34c9f9.png)
* 서버에서 받아서 이벤트를 진행함

## Fabric event 종류
* Block 새로 생성될 때마다 이벤트를 가져오는
  * Full 블럭 가져오기
  * 원하는 블럭만 가져오기
 * Transaction Event
   * User가 거래내역을 제출한후 커밋이 진행 되었는지 확인해 봐야하기 때문에 이벤트 존재
 * Chaincode Event
   * Setevent(eventname, Payload) (?)
 
## 4. User관리

### 유저 생성
* 비트코인 혹은 이더리움
  * 유저가 직접 개인기를 생성 할 수 있음
* Fabric
  * CA를 통해서 개인키와 인증서를 발급 받아야함.

### 유저관리 
1. Delegated
* Balance-Transfer 모델
* **서버가 모든 유저의 키를 관리**
* 회원 관리하는 서버가 유저ID와 인증서를 매칭해서 트랜잭션 생성
![image](https://user-images.githubusercontent.com/50629716/87015086-e6258680-c207-11ea-9f79-7b60cb527cb3.png)

* 아무래도 서버가 존재하다 보니 Fabric에서 직접적으로 연결되어있는 것보다 일처리가 빠르게 가능함 // 서로 간의 통신이 잦아서 직접적이면 느려진다.

2. Decentralized
![image](https://user-images.githubusercontent.com/50629716/87015814-e07c7080-c208-11ea-9b62-ba846c5c2592.png)
* Dapp모델
* 모바일에서 키를 관리한다. 안드로이드에서 키를 잘 관리해야함
* Fabric CA를 통해 유저 생성

  * 직접적으로 안드로이드로 통신하다보니 관리가 안되고 보안 같은것도 위험함 Peer, Orderer등과 통신하기에 에러걱정도 많고 사실상 어려움.


3. Chaincode Level User
* 체인코드 레벨에서 PKI 공개키 암호화를 도입해서 이용
* 크립토 연산이 중복
