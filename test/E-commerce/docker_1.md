# mysql DB 생성

linux환경에서 docker mysql image를 다운로드`docker pull`받고 컨테이너를 실행했다.<br/>
`docker run --name mysql -p 3306:3306 -d -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql:latest`<br/>

잘 구동되고 있는지 확인하고 `docker ps -a` mysql에 접속했습니다.<br/>
`docker exec -it mysql mysql -h127.0.0.1 -uroot -p`<br/>

이제 DB를 만들고 table을 생성하면 되는데 문제가 생겼습니다. DB를 생성하고 기존 데이터를 넣어두고 `docker commit`해서 이미지로 배포하려고 했으나 `commit`한 이미지를 확인해 보니 아무 데이터도 없더군요.

구글 검색 결과..<br/>


```note
The official mysql image stores data in a volume. Normally this is desired so that your data can persist beyond the life span of your container, but data volumes bypass the Union File System and are not committed to the image.

You can accomplish what you're trying to do by creating your own mysql base image with no volumes. You will then be able to add data and commit it to an image, but any data added to a running container after the commit will be lost when the container goes away.

[출처](https://stackoverflow.com/questions/30740828/commit-data-in-a-mysql-container)
```

```
공식 mysql 이미지는 데이터를 볼륨에 저장합니다. 일반적으로 이는 데이터가 컨테이너의 수명 이상으로 지속될 수 있도록하는 것이 바람직하지만
데이터 볼륨은 Union File System을 우회하고 이미지에 커밋되지 않습니다.

볼륨이없는 자체 mysql 기본 이미지를 생성하여 수행하려는 작업을 수행 할 수 있습니다.
그러면 데이터를 추가하고 이미지에 커밋 할 수 있지만 커밋 후 실행중인 컨테이너에 추가 된 모든 데이터는 컨테이너가 사라지면 손실됩니다.
```

그렇다네요.

```
FROM mysql:latest

RUN cp -r /var/lib/mysql /var/lib/mysql-no-volume

CMD ["--datadir", "/var/lib/mysql-no-volume"]
```

이렇게 이미지를 새로 빌드`docker build`해서 사용하면 데이터가 잘 보관되어 있네요.
![docker_build](https://user-images.githubusercontent.com/76420201/104550631-9c1ad300-5678-11eb-8ea9-a8bcf51ab739.GIF)

```
docker run -d --name mysql_test -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql_test:0.1
docker ps -a
docker run exec -it mysql_test mysql -h127.0.0.1 -uroot -p
```

```
show databases;
```