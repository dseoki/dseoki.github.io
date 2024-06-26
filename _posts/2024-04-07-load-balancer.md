---
title: 로드 밸런서(Load Balancer)
summary: 로드 밸런서(Load Balancer)에 대해서 알아보자
categories: [dev]
comments: true
---

## 로드 밸런서(Load Balancer)
애플리케이션 가용성을 최적화하고 긍정적인 최종 사용자 경험을 제공하기 위해 **여러 서버에 네트워크 트래픽을 효율적으로 분산하는 프로세스**

클라이언트와 서버풀(분산네트워크를 구성하는 서버들의 그룹) 사이에 위치해서 한 서버에 부하가 집중되지 않도록 한다.

## 로드 밸러서 기능
### 1. Health Check (상태 확인)
서버들에 대한 주기적인 Health Check를 통해 서버들의 장애 여부를 판단하여, 정상 동작 중인 서버로만 트래픽을 보낸다.
- L3: **ICMP**를 이용하여 서버의 IP 주소가 통신 가능한 상태인지를 확인 한다.
- L4: TCP는 3 Way-Handshaking (전송 - 확인/전송 - 확인)를 기반으로 통신하는데, 이러한 TCP의 특성을 바탕으로 각 포트 상태를 체크하는 방식
- L7: 어플리케이션 계층에서 체크를 수행. 실제 웹페이지에 통신을 시도하여 이상 유무를 파악

> ### 💡 ICMP
> 한 개의 서버에 과부하가 걸리지 않도록 온프레미스 또는 서버 팜이나 클라우드 데이터 센터에 호스팅된 사용할 수 있는 서버 수에 따라 요청을 라우팅한다.

### 2. Tunneling (터널링)
**데이터 스트림을 인터넷 상에서 가상의 파이프를 통해 전달하는 기술**로, 패킷 내에 터널링할 대상을 캡슐화시켜 목적지까지 전송하는 기술

연결된 상호 간에만 캡슐화된 패킷을 구별해 캡슐화를 해제하게 한다.

### 3. NAT (Network Address Translation)
내부 네트워크에서 사용하는 사설 IP 주소와 로드밸런서 외부의 공인 IP 주소 간의 변환 역할을 한다.

로드밸런싱 관점에서 여러개의 호스트가 하나의 공인 IP 주소(VLAN or VIP)를 통해 접속하는 것이 주 목적이다.

- SNAT(Source Network Address Translation): 내부에서 외부로 트래픽이 나가는 경우
- DNAT(Destination Network Address Translation): 외부에서 내부로 트래픽이 들어오는 경우

### 4. DSR (Direct Server Return)
서버에서 클라이언트로 트래픽이 되돌아가는 경우, 목적지를 클라이언트로 설정한 다음, 네트워크 장비나 로드밸런서를 거치지 않고 바로 클라이언트를 찾아가는 방식이다.

이 기능을 통해 로드밸런서의 부하를 줄여줄 수 있다.

<br>

## 로드밸런서 알고리즘
### 라운드 로빈 (Round Robin)
서버에 들어온 요청을 순서대로 돌아가며 배정하는 방식\
클라이언트의 요청을 순서대로 분배하기 때문에 서버들이 동일한 스펙을 갖고 있고, 서버와의 연결(세션)이 오래 지속되지 않는 경우에 활용하기 적합하다.

### 가중 라운드 로빈 (Weighted Round Robin)
각각의 서버마다 가중치를 매기고 가중치가 높은 서버에 클라이언트 요청을 우선적으로 배분.\
주로 서버의 트래픽 처리 능력이 상이한 경우 사용되는 방식

### IP 해시 (IP Hash)
클라이언트의 IP 주소를 특정 서버로 매핑하여 요청을 처리하는 방식\
사용자의 IP를 해싱하여 부하를 분산하기 때문에 사용자가 항상 동일한 서버로 연결되는 것을 보장.\
경로가 보장되며, 접속자 수가 많을수록 분산 및 효율이 뛰어나다.

### 최소 연결 (Least Connection)
Request가 들어온 시점에 가장 적은 연결(세션) 상태를 보이는 서버에 우선적으로 트래픽을 할당\
자주 세션이 길어지거나, 서버에 분배된 트래픽들이 일정하지 않은 경우에 적합

### 최소 응답시간 (Least Response Time)
서버의 현재 연결 상태와 응답시간을 모두 고려하여, 가장 짧은 응답 시간을 보내는 서버로 트래픽을 할당하는 방식\
각 서버들의 가용한 리소스와 성능, 처리중인 데이터 양 등이 상이할 경우 적합

### 대역폭 (Bandwidth)
서버들과의 대역폭을 고려하여 서버에 트래픽을 할당



## L4와 L7
부하 분산에는 L4 로드밸런서와 L7 로드밸런서가 가장 많이 활용된다. 그 이유는 L4부터 포트(port)정보를 바탕으로 로드를 분산하는 것이 가능하기 때문이다.\
한 대의 서버에 각기 다른 포트 번호를 부여하여 다수의 서버 프로그램을 운영하는 경우라면 최소 L4 로드밸런서 이상을 사용해야 한다.

### L4 로드 밸런서
4계층 - 네트워크 계층(IP, IPX)이나 3계층 - 전송 계층(TCP, UDP)의 정보를 바탕으로 로드를 분산한다.\
즉, IP 주소, 포트번호, MAC 주소, 전송 프로토콜 등에 따라 트래픽을 분산하는 것이 가능하다.

![L4 load balancer](https://github.com/dseoki/dseoki.github.io/assets/32925806/8e483528-028f-4f7c-ae25-ed714920edf0)
*L4 로드밸런싱*

#### 장점
* 패킷의 내용을 확인하지 않고 로드를 분산하므로 속도가 빠르고 효율이 높다.
* 데이터의 내용을 복호화할 필요가 없기에 안전하다.
* L7 로드밸런서보다 가격이 저렴하다.

#### 단점
* 패킷의 내용을 살펴볼 수 없으므로, 섬세한 라우팅 불가하다.
* 사용자의 IP가 수시로 바뀌는 경우라면, 연속적인 서비스를 제공하기 어렵다.


### L7 로드 밸런서
7계층 - 어플리케이션 계층(HTTP, FTP, SMTP 등)에서 로드를 분산하기 때문에, HTTP 헤더, 쿠키 등과 같은 사용자 요청을 기준으로 특정 서버에 트래픽을 분산하는 것이 가능하다.

![L7 load balancer](https://github.com/dseoki/dseoki.github.io/assets/32925806/a22a62c7-72a5-4dd3-891f-c25f9a492689)
*L7 로드밸런싱*

L4의 기능에 더하여, 패킷의 내용을 확인하고 그 내용에 따라 로드를 특정 서버에 분배하는 것이 가능하다.

특정한 패턴을 지닌 바이러스를 감지해 네트워크를 보호할 수 있다.

Dos/DDos와 같은 비정상적인 트래픽 필터링이 가능하다.

* URL 스위칭 (URL Switching): 특정 하위 URL들은 특정 서버로 처리하는 방식
* 컨텍스트 스위칭 (Context Switching): 클라이언트가 요청한 특정 리소스에 대해 특정 서버로 연결 가능
* 쿠키 지속성 (Persistence with Cookies): 쿠키 정보를 바탕으로 클라이언트가 연결했었던 동일한 서버에 계속 할당해 주는 방식.\
특히, 사설 네트워크에 있던 클라이언트의 IP 주소가 공인 IP 주소로 치환되어 전송하는 방식을 지원

#### 장점
* 상위 계층에서 로드를 분산하기 때문에 훨씬 더 섬세한 라우팅이 가능하다.
* 캐싱(Cashing) 기능을 제공한다.
* 비정상적인 트래픽을 사전에 필터링 할 수 있어 서비스 안정성이 높다.

#### 단점
* L4에 비해 비싸다.
* 패킷의 내용을 복호화 하므로 더 높은 비용을 지불해야 한다.
* 클라이언트가 로드밸런서와 인증서를 공유해야 하기 때문에, 공격자가 로드밸런서를 통해 클라이언트의 데이터에 접근할 수 있는 보안상의 위험성이 존재한다.




