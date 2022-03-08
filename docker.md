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
RUN sed -i 's/\r//g' init.sh RUN sed -i 's/\r//g' /usr/local/zookeeper/conf/zoo.cfg CMD bash init.sh

출처: https://data-engineer-tech.tistory.com/8 [데이터 엔지니어 기술 블로그]
```



## Zookeeper Ensemble in docker



## Kafka in docker

