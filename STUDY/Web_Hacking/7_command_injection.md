---
sort: 7
---

# OS Command Injection

<center><img src = "https://user-images.githubusercontent.com/76420201/106404668-cc36e400-6476-11eb-9695-c38bd8098047.png" width = "80%"></center>
<br>

## OS Command Injection (운영체제 명령어 삽입)
<u>OS Command Injection</u>은 공격자가 웹 어플리케이션 파라미터(입력값)에 시스템 명령어를 입력 후 실행시켜 서버정보 확인, 권한 상승, 다른 취약점과 병행하여 비밀 파일 등에 접근 및 서버를 제어할 수 있는 취약점 이다.

<br>

## 취약점이 발생하는 이유

- 외부 입력값을 검증, **제한**하지 않고 운영체제 명령어 또는 운영체제 명령어의 일부로 사용하는 경우 발생한다.

- 운영체제 명령어가 변조되어 의도하지 않는 명령어 또는 추가 명령어가 실행

<br>

## 공격방법

OS명령어를 처리하는 페이지에 제공되는 명령어와 ', &#124;, &&, &, ; 등 명령 삽입 특수문자를 입력하고, 이어서 이용하고자 하는 명령어를 입력하면 명령삽입 특수문자 앞에 있는 명령어의 결과를 출력하고 뒤이어 두번째 명령어를 실행하여 결과를 출력합니다.

- 메타캐릭터

|-|-|-|
|`|`|파이프라인 | 앞에서 실행한 명령어 출력 결과를 뒤에서 실행하는 명령어 입력값으로 처리함|
|`;`|세미콜론|한개의 라인에 여러개 명령어를 수행해줌|
|`&&`|더블 엠퍼센트|첫 번째 명령어가 성공되면 다음 명령어를 수행함(단, 실패시 다음 명령어 수행X)|
|`||`|더블 버티컬바|첫 번째 명령어가 실패해도 다음 명령어를 수행함(단, 성공시 다음명령어 수행 X)|


- 개발자가 의도한 형태의 입력
http://.../help.jsp?doc_page=`sqli.html` ⇒ type c:\data\help_file\sqli.html 명령어의 실행 결과를 사용자에게 반환
                                              
- 개발자가 의도하지 않은 형태의 입력
http://.../help.jsp?doc_page=`sqli.html %26 ipconfig` ⇒ 의도하지 않은 ipconfig 명령어의 추가 실행 결과를 사용자에게 반환

**방법**

1. 사용자 화면에서 서버로 전달되는 값을 조작 (브라우저의 개발자 도구를 이용)

2. 프록시를 이용해서 서버로 전달되는 값을 조작

<br>
    
## 방어방법

OS Command Injection 취약점을 막기 위해서는 사용 가능한 명령어를 지정하여 외부로부터 부적절한 명령어 입력 사용을 막아야 합니다.

- 화이트 리스트 방식 = 허용 목록 : 사용할 내용을 목록에 저장하고, 저장된 범위 내의 값만 사용하도록 제한
새로운 유형의 입력값에 대해서도 동일한 보안성을 제공하는 장점이 있음

- 블랙 리스트 방식 = 제한 목록 : 사용하지 않을 내용을 목록에 저장하고, 저장된 범위 밖의 값만 사용하도록 제한
모집합이 크거나 변경이 심한 경우에 적용 (예: 인천공항)

