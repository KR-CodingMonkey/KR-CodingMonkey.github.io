---
sort: 5
---

# Port scanning

## 포트 스캐닝(Port Scaning)

- 어떤 포트가 열려 있는지 확인하는 것으로 **침입 전 취약점을 분석**하기 위한 사전작업 중 하나

- nmap을 주로 사용 (오픈 소스 기반의 포트 스캔 도구)
  - `nmap [scan_type] [options] [target]`
  - scan_type 옵션

  | -sS | TCP SYN(Half - Open) Scan|
  | -sT | TCP Connect(Open) Scan |
  | -sU| UDP Scan|
  | -sF| TCP FIN Scan|
  | -sX| TCP Xmas Scan|
  | -sN| TCP NULL Scan|
  | -sA| TCP Ack Scan|
  | -sP| Ping(icmp/icmp echo) Scan|
  | -D| Decoy Scan|

<br>

## 동작 방식

<details markdown="1">
<summary><b>TCP SYN(Half-Open)스캔 (스텔스 스캔)</b></summary>

<br>   
- 완전한 연결을 수행하지 않기 때문에 로그가 남지 않는다.
- SYN -> SYN + ACK -> RST<br>
```
서비스 포트가 동작하는 경우 :  SYN → SYN/ACK
서비스 포트가 동작하지 않는 경우 : SYN → RST
```

- 열려있는 경우

```javascript
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sS -p 80 192.168.94.133
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-25 21:19 EST
Nmap scan report for 192.168.94.133
Host is up (0.00077s latency).

PORT   STATE SERVICE
80/tcp "open"  http
MAC Address: 00:50:56:30:D5:73 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.46 seconds
```                                                                                                            

- 닫혀있는 경우

```javascript
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sS -p 8080 192.168.94.133 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-25 21:19 EST
Nmap scan report for 192.168.94.133
Host is up (0.0013s latency).

PORT     STATE  SERVICE
8080/tcp "closed" http-proxy
MAC Address: 00:50:56:30:D5:73 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.50 seconds
```

</details>

---

<details markdown="1">
<summary><b>TCP Connection(Open)스캔</b></summary>

<br>   
- 결과가 가장 정확하지만 로그가 남는다.
- 방화벽 존재시, DROP(해당 패킷 폐기) / REJECT(해당 패킷 폐기 후 ICMP 메시지 전송) 2방법 존재<br> 
```
서비스 포트가 동작하는 경우 :  SYN → SYN/ACK
서비스 포트가 동작하지 않는 경우 : SYN → RST 
```

- 열려있는 경우

```javascript
┌──(kali㉿kali)-[~]
└─$ nmap -sT -p 80 192.168.94.133 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-25 20:56 EST
Nmap scan report for 192.168.94.133
Host is up (0.0013s latency).

PORT   STATE SERVICE
80/tcp "open"  http

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

- 닫혀있는 경우

```javascript               
┌──(kali㉿kali)-[~]
└─$ nmap -sT -p 8080 192.168.94.133 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-25 20:56 EST
Nmap scan report for 192.168.94.133
Host is up (0.0014s latency).

PORT     STATE  SERVICE
8080/tcp "closed" http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
```

</details>

---

<details markdown="1">
<summary><b>TCP FIN/NULL/Xmas 스캔 (스텔스 스캔)</b></summary>

<br>
- TCP Header의 제어비트를 비정상적으로 설정해서 스캔하는 방식
- 포트 상태가 Closed라면 요청 세그먼트에 대한 응답으로 RST를 받게 된다.
- RST가 오지 않으면 포트가 열려있거나 방화벽에서 filtered 된 상황<br>
```
서비스 포트가 동작하는 경우 :  FIN → ??? (무응답)
서비스 포트가 동작하지 않는 경우 : FIN → RST
```   

</details>

---

<details markdown="1">
<summary><b>TCP ACK 스캔</b></summary>

<br>
- 포트 오픈 여부를 판단하는 것이 아니라 방화벽의 룰셋(필터링 룰셋)을 테스트하기 위한 스캔
- 대상 방화벽이 상태 기반(stateful)인지 여부(TCP연결 상태 추적)
- 대상 포트가 방화벽에 의해 필터링 되고 있는지 여부<br>  

</details>

---

**UDP 스캔**
- ICMP Unreachable 메시지를 이용하여 UDP포트의 Open 여부를 확인하기 위한 스캐닝 방식<br>

---

**Decoy 스캔**
- 실제 스캐너 주소 외에 다양한 위조된 주소로 스캔하는 방식<br>
    
