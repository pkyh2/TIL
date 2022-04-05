# docker

## What is docker?

도커는 개발, 적재 그리고 어플리케이션 실행을 위한 오픈 플랫폼이다.
도커는 너가 너의 infrastructure로 부터 너의 어플리케이션들을 분리 가능하게 해준다. 그래서 너는 빠르게 소프트웨어를 전달할 수 있다.
도커와 함께 너는 너의 infrastructure를 관리할 수 있고, 같은 방법으로 너의 어플리케이션을 관리할 수 있다.

출처 : https://aws.amazon.com/ko/docker/

Docker는 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼이다.
Docker는 **소프트웨어를 컨테이너라는 표준화된 유닛으로 패키징**하며, 이 컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는 데 필요한 모든 것이 포함되어 있다.

### Docker 작동 방식

Docker는 컨테이너를 위한 운영 체제다. 가상머신(aws EC2, VMware, Virtual Box)이 서버 하드웨어를 가상화 하는 방식과 비슷하게 컨테이너는 서버 운영 체제를 가상화 한다.
![monolith_2-VM-vs-Containers](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_2-VM-vs-Containers.78f841efba175556d82f64d1779eb8b725de398d.png)

가상머신(VM)과 달리 Docker는 **하나의 Guest OS**에 Docker Engine을 올려 Docker Engine에서 여러개의 애플리케이션을 구축할 수 있다.



## Dockerfile

도커는 도커파일로부터 **지시사항을 읽어서** 자동적으로 이미지를 빌드 할 수 있다.
도커 파일은 유저가 모은 이미지를 커멘드 라인에서 불러 올 수 있는 **커멘드들을 포함한 텍스트 문서**다. 
유저가 `docker build` 명령을 사용하는 것은 dockerfile에 작성된 커멘드라인 지시들을 실행하여 자동적인 구축을 만들 수 있다.

```dockerfile
# FROM : 새로운 빌드 단계를 초기화 하고 다음에 오는 명령어들에 대한 기본 이미지를 설정
# RUN : 커멘드 명령어 실행 또는 쉘 명령어 실행
# ENV : 환경변수 세팅
# WORKDIR : 해당 경로로 이동
# COPY : <경로/파일> <경로/파일> 앞 경로에서 뒤 경로로 파일복사

FROM ubuntu:18.04
RUN mkdir -p /root/install
RUN apt-get update

WORKDIR /root/install

# java 설치 준비
ENV DEBIAN_FRONTEND noninteractive
# https://knight76.tistory.com/entry/aptgetyum-noninteractive
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 

# java 설치 
RUN apt-get install openjdk-8-jdk -y
RUN apt-get install wget -y
RUN apt-get install vim -y 

# zookeeper 설치
RUN wget downloads.apache.org/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz
RUN tar -zxvf apache-zookeeper-3.7.0-bin.tar.gz
RUN mv apache-zookeeper-3.7.0-bin /usr/local/zookeeper 

# 설정파일 및 초기화 파일 복사
COPY config/zoo.cfg /usr/local/zookeeper/conf/zoo.cfg
COPY config/init.sh init.sh 

# windows에서 작업 시 CRLF와 LF 처리 방식 문제 방지
RUN sed -i 's/\r//g' init.sh 
RUN sed -i 's/\r//g' /usr/local/zookeeper/conf/zoo.cfg CMD bash init.sh

출처: https://data-engineer-tech.tistory.com/8 [데이터 엔지니어 기술 블로그]
```



## Zookeeper Ensemble in docker

1. 작업 폴더 생성

2. `Dockerfile`을 다음과 같이 작성

   ```dockerfile
   # FROM : 새로운 빌드 단계를 초기화 하고 다음에 오는 명령어들에 대한 기본 이미지를 설정
   # RUN : 커멘드 명령어 실행 또는 쉘 명령어 실행
   # ENV : 환경변수 세팅
   # WORKDIR : 해당 경로로 이동
   # COPY : <경로/파일> <경로/파일> 앞 경로에서 뒤 경로로 파일복사
   
   FROM ubuntu:18.04
   RUN mkdir -p /root/install
   RUN apt-get update
   
   WORKDIR /root/install
   
   # java 설치 준비
   ENV DEBIAN_FRONTEND noninteractive
   # https://knight76.tistory.com/entry/aptgetyum-noninteractive
   ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
   
   # java 설치 
   RUN apt-get install openjdk-8-jdk -y
   RUN apt-get install wget -y
   RUN apt-get install vim -y 
   
   # zookeeper 설치
   RUN wget downloads.apache.org/zookeeper/zookeeper-3.7.0/apache-zookeeper-3.7.0-bin.tar.gz
   RUN tar -zxvf apache-zookeeper-3.7.0-bin.tar.gz
   RUN mv apache-zookeeper-3.7.0-bin /usr/local/zookeeper 
   
   # 설정파일 및 초기화 파일 복사
   COPY config/zoo.cfg /usr/local/zookeeper/conf/zoo.cfg
   COPY config/init.sh init.sh 
   
   # windows에서 작업 시 CRLF와 LF 처리 방식 문제 방지
   # RUN sed -i 's/\r//g' init.sh 
   # RUN sed -i 's/\r//g' /usr/local/zookeeper/conf/zoo.cfg CMD bash init.sh
   
   # 출처: https://data-engineer-tech.tistory.com/8 [데이터 엔지니어 기술 블로그]
   ```

3. `config` 폴더를 `Dockerfile`이 있는 폴더에 생성

4. `config` 폴더에 `init.sh` 파일을 생성 후 아래 스크립트 작성

   ```shell
   mkdir -p /data
   
   echo $MY_ID > /data/myid
   
   /usr/local/zookeeper/bin/zkServer.sh start
   
   tail -f /dev/null
   ```

5. `config` 폴더에 `zoo.cfg`파일을 생성 후 아래 스크립트 작성

   ```
   initLimit=10
   tickTime=2000
   
   syncLimit=5
   
   dataDir=/data
   
   clientPort=2181
   
   server.1=pipeline-zookeeper-a:2888:3888
   server.2=pipeline-zookeeper-b:2888:3888
   server.3=pipeline-zookeeper-c:2888:3888
   
   autopurge.snapRetainCount=3
   autopurge.purgeInterval=24
   
   # 출처: https://data-engineer-tech.tistory.com/8 [데이터 엔지니어 기술 블로그]
   ```

6. 네트워크를 생성하기 위해 `docker network create zoo` 명령어를 입력하면, zoo라는 네트워크 안에 있는 컨테이너는 서로 통신이 가능하다.

7. `Dockerfile` 이 있는 곳에서 `docker build --tag pipeline-zookeeper .` 명령어를 입력

8. `docker-compose.yml` 파일을 생성하고 아래 스크립트를 입력

   - 볼륨을 공유해주기 위해 개별 볼륨을 생성했고 각 컨테이너에 연결하는 설정을 했다.
   - MY_ID를 환경변수로 넣으면, init.sh에서 /data/myid 파일에 만들면서 저장한다.

   ```docker-compose
   version: '3.8'
   volumes:
           pipeline-zookeeper-a-volume:
                   name: pipeline-zookeeper-a-volume
           pipeline-zookeeper-b-volume:
                   name: pipeline-zookeeper-b-volume
           pipeline-zookeeper-c-volume:
                   name: pipeline-zookeeper-c-volume
   
   services:
           pipeline-zookeeper-a:
                   image: pipeline-zookeeper
                   container_name: pipeline-zookeeper-a
                   restart: always
                   hostname: pipeline-zookeeper-a
                   environment:
                           MY_ID: 1
                   volumes:
                           - pipeline-zookeeper-a-volume:/data
   
           pipeline-zookeeper-b:
                   image: pipeline-zookeeper
                   container_name: pipeline-zookeeper-b
                   restart: always
                   hostname: pipeline-zookeeper-b
                   environment:
                           MY_ID: 2
                   volumes:
                           - pipeline-zookeeper-b-volume:/data
   
           pipeline-zookeeper-c:
                   image: pipeline-zookeeper
                   container_name: pipeline-zookeeper-c
                   restart: always
                   hostname: pipeline-zookeeper-c
                   environment:
                           MY_ID: 3
                   volumes:
                           - pipeline-zookeeper-c-volume:/data
   
   networks:
           default:
                   external:
                           name: zoo
   
   ```

9. `docker-compose` 설치

   ```shell
   sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   
   # 실행권한 적용
   chmod +x /usr/local/bin/docker-compose
   
   # 심볼릭 링크 설정
   ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   
   # 설치 확인
   docker-compose --version
   
   # ------------------------------
   # docker root 권한없이 사용하기
   sudo usermod -aG docker {username}
   # username 확인하기
   whoami
   ```

10. `docker-compose.yml` 이 있는 폴더에서 `docker-compose up -d` 명령어를 사용하여 컨테이너들을 실행 `docker-compose down` `docker-compose up -d` 명령으로 새로 실행 가능

11. 아래 명령어로 잘 실행 되었는지 확인

    ```shell
    docker exec pipeline-zookeeper-a /usr/local/zookeeper/bin/zkServer.sh status
    docker exec pipeline-zookeeper-b /usr/local/zookeeper/bin/zkServer.sh status
    docker exec pipeline-zookeeper-c /usr/local/zookeeper/bin/zkServer.sh status
    ```

### command 정리

```shell
# Linux 터미널 네트워크 삭제
docker rm -f $(docker ps -aq)
docker network prune
docker rmi image ID name(dev삭제)
docker rmi -f $(docker images -aq)
sudo service docker restart


sudo systemctl restart docker # (다시시작)
sudo shutdown -r now

docker network ls
docker network

```



### Issue

1. `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`
   - docker service가 실행이 안됐다.
   - `sudo systemctl status docker` : 상태확인
   - `sudo systemctl start docker` : 재시작
   - `sudo systemctl enable docker` : 재가동?

2. `docker ps` 상태 확인에서 실행중이 컨테이너의 STATUS의 restarting 문제로 진행 불가중.



## Kafka in docker

