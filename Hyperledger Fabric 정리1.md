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

