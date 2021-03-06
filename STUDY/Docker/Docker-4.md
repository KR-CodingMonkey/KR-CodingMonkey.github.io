# Docker Container
  

## 컨테이너 실행
도커를 실행하는 명령어이다. container는 생략 가능하다.

`docker container run [option] [image:tag] [command]`<br/>
`docker run [option] [image:tag] [command]`

| Option  | Description |
| ------- | -------- |
| --name | 컨테이너 이름 부여 |
| -d, ---detach | detach mode, 백그라우드에서 실행한다|
| -p [Host Port:Container Port], ---publish | Port forwarding, 호스트와 컨테이너의 포트를 연결 |
| -e | 환경변수 설정 |
| -rm | 컨테이너 종료시 컨테이너 자동 삭제 |
| -it | 콘솔에 결과를 출력하는 옵션, -i -> 컨테이너 표준출력을 연다 -t -> tty(디바이스)를 확보한다 |
| exec | 외부에서 실행되고 있는 컨테이너 안의 명령을 내릴 수 있다 | 

<br/>
`docker run [image]` 라는 명령어는 이미지를 다운받고`docker pull [image]` 컨테이너를 생성하고`docker container create` 컨테이너를 실행하는`docker container start` 3가지 명령어를 동시에 처리할 수 있다. 이미지가 이미 존재한다면 다운로드`pull`하지 않는다. 

![docker_run](https://user-images.githubusercontent.com/76420201/104117172-9795a800-5362-11eb-907a-9e471f31c88b.GIF)

이미지 이름 뒤에 tag를 붙이지 않으면 자동으로 latest버전을 다운로드 하게 된다.
`docker run ubuntu` = `docker run ubuntu:latest` 하지만 특정한 버전을 다운로드 하고 싶을때는 tag를 원하는 버전에 맞추어 입력해야 한다. ex) `docker run ubuntu:16.04`

컨테이너는 정상적으로 실행됐지만 뭘 하라고 명령어를 전달하지 않았기 때문에 컨테이너는 생성되자마자 종료된다. **컨테이너는 프로세스**이기 때문에 실행중인 프로세스가 없으면 컨테이너는 종료된다.

<br/>

## 컨테이너 리스트

`docker container ls`<br/>
`docker ps`

`docker run`명령어를 입력하면 컨테이너 생성되고 실행된다. `docker ps`명령어를 통해서 현재 가동중인 컨테이너 목록을 확인할 수 있다. `docker ps -a`와 같이 `-a`옵션을 부여하게 되면 실행 중/정지 중인 모든 컨테이너를 표시한다.<br/>
STATUS UP은 실행중 Exited는 정지 상태를 의미한다.

![docker_ps_a](https://user-images.githubusercontent.com/76420201/104118439-86519900-536c-11eb-8412-40aaa86f368c.GIF)

이와 같이 하나의 이미지로 여러개의 컨테이너를 실행할 수 있다. 주의해야 할 점은 서로 다른이름을 부여해줘야 한다는 것이다. 이름이 중복되면 에러메시지가 출력된다.

<br/>

## 컨테이너 삭제 
`docker container rm [Container ID]`<br/>
`docker rm [Container ID]`

정지 상태인 컨테이너를 삭제할 때 `docker rm [Container ID]`명령어를 입력하면 된다. 
![docker_rm](https://user-images.githubusercontent.com/76420201/104118742-7d61c700-536e-11eb-822b-e792c83eb55b.GIF)

`[Container ID]`입력 하실때 ID 전체를 쓰지 않아도 구분할 수 있을정도만 써주면 된다. 실행중인 컨테이너는 삭제되지 않는다. 정지 중인 모든 컨테이너를 삭제하려면 `docker container prune`명령어를 입력하면 된다.

