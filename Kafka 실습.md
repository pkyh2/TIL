# Kafka 실습

### aws 인스턴스 생성

1. aws 에 접속하여 EC2 인스턴스시작
2. t2.micro 를 선택하여 `인스턴스 세부 정보 구성` 에서 인스턴스의 개수를 3개로 변경
3. 새 키 발급 받기를 선택하고 인스턴스 시작
4. vscode연동을 위해 .pem키 속성 변경

### java jdk 설치

1. `sudo yum install java-1.8.0-openjdk.x86_64`
2. `vim /etc/profile` 들어가서 `export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk` 추가
3. `source /etc/profile` 적용

### zookeeper 설치

1. 3개의 인스턴스를 실행한 vscode의 각각 커멘트창에 `wget 주키퍼 다운링크`를 입력하여 설치

2. `tar xvf .gz` 를 통해 압축해제

3. 압축한 폴더로 들어가 /conf/zoo.cfg 파일 생성

4. `zoo.cfg` 파일에서 zookeeper의 configuration을 설정

   ```
   tickTime=2000
   dataDir=/var/lib/zookeeper
   clientPort=2181
   initLimit=20
   syncLimit=5
   server.1=test-broker01:2888:3888
   server.2=test-broker02:2888:3888
   server.3=test-broker03:2888:3888
   ```

5. 각 서버별로 ip가 아닌 hostname으로 통신하기 위해 `/etc/hosts`를 수정

   - `hosts` 파일 권한변경 `sudo chmod 666 hosts` : 쓰기 권한 추가

   ```
   # 각 기준서버를 0.0.0.0으로 설정하고 나머지 서버는 각각의 원래 ip주소로 작성
   0.0.0.0 test-broker01
   52.78.162.38 test-broker02
   3.36.96.114 test-broker03
   
   0.0.0.0 test-broker02
   3.36.119.48 test-broker01
   3.36.96.114 test-broker03
   
   0.0.0.0 test-broker03
   3.36.119.48 test-broker01
   52.78.162.38 test-broker02
   ```

6. zookeeper들을 서로 연동하기 위해서는 방화벽 설정이 필요

   - security group에서 inbound와 outbound를 설정
   - zookeeper은 2181, 2888, 3888 포트를 사용하므로 3개의 port에 대해 anywhere조건으로 open
   - 주의! 기존 ssh 설정을 냅두고 다른 port 추가!!

7. zookeeper 앙상블을 만들기 위해 각 zookeeper마다 myid라는 파일을 생성 myid의 위치는 `/var/lib/zookeeper/myid` 이고, 해당 파일에 숫자 1, 2, 3을 작성

   - `/lib` 폴더까지가서 `zookeeper`폴더가 없으면 생성 후 폴더안에 `myid` 파일 생성

8. `/apache zookeeper/bin` 으로 들어가서 `./zkServer.sh start` 실행

   - `FAILED TO WRITE PID` Error 발생
     - `sudo chmod o+w /var/lib/zookeeper/` : zookeeper 폴더 권한 변경

9. `./zkCli.sh -server ip주소` : 여기서 잘 안됨.

   - 되는거 같긴한데 계속 `log` 메세지 같은게 뜸.
   - https://box0830.tistory.com/340 다른 버전으로 다시 설치

10. 카프카 설치(내일 부터!)

    - `tar xvf kafka gz` : kafka 파일 압축풀기

11. 카프카 설정
    - `/config/server.properties` : 파일에서 `broker.id=0 1 2` 설정, `zookeeper.connect=test-broker01:2181,test-broker02:2181,test-broker03:2181/test` 설정

12. 카프카 시작
    - `/bin` 경로에서 `kafka-server-start.sh ../config/server.properties` 실행
    - `failed; error='Cannot allocate memory'` 오류 발생
      - `kafka-server-start.sh` 파일에서 `export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"` 로 수정

13. 카프카 강의 좀 듣고

14. python kafka 연동 해보기(docker)

