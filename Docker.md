# Docker 사용법
- 설치
  ```
  sudo wget -qO- https://get.docker.com/ | sh
  ```
- Docker를 sudo 없이 사용하기(이 설정 후에는 sudo를 안붙여도 되며 재접속 이후 사용 가능)
  ```
  sudo usermod -aG docker $USER
  sudo service docker restart
  ```
- 도커 버전 확인
  ```
  docker version
  ```
- 실행 환경 확인
  ```
  docker system info
  ```
- 디스크 이용 상황
  ```
  docker system df
  ```
- 이미지 검색
  ```
  sudo docker search ubuntu
  ```
  ```
  docker search <image_name>
  ```
- Docker 이미지 받아보기
  ```
  sudo docker pull ubuntu:latest
  ```
- 모든 이미지 출력해보기
  ```
  sudo docker images
  ```
- run 명령으로 컨테이너 생성
  ```
  sudo docker run -i -t --name hello ubuntu /bin/bash
  ```
  ```
  docker run <옵션> <이미지 이름> <실행할 파일>
  -i : interactive / -t(pseudo-tty) 옵션을 사용하여 실행된 셸에 입/출력 가능
  --name 옵션으로 생성될 도커 이름 지정
  ```
- 컨테이너 목록 확인
  ```
  sudo docker ps -a
  ```
- 컨테이너 (재)시작
  ```
  sudo docker (re)start hello
  ```
  ```
  docker (re)start <container_name>
  ```
- 컨테이너 접속
  ```
  sudo docker attach hello
  ```
  ```
  docker attach <container_name>
  ctrl+p, ctrl+q를 차례로 누르면 셸이 정지되지 않고 빠져나올 수 있다.
  ```
- 접속 없이 명령 실행
  ```
  sudo docker exec hello echo "Hello World"
  ```
  ```
  docker exec <container_name> <command> <parameters>
  정지된 상태에서는 사용 불가
  apt-get, yum 등을 사용할 때 활용
  ```
- 컨테이너 정지
  ```
  sudo docker stop hello
  ```
  ```
  docker stop <container_name>
  ```
- 컨테이너 삭제
  ```
  sudo docker rm hello
  ```
  ```
  docker rm <container_name>
  ```
- 이미지 삭제
  ```
  sudo docker rmi ubuntu:latest
  ```
  ```
  docker rmi <image_name>:<tag>
  태그 미지정시 image_name에 해당하는 모든 이미지 삭제
  ```
- 컨테이너의 현재 상태를 이미지로 만들기
  ```
  sudo docker commit tf tf/tf_keras
  ```
  ```
  docker commit [container_name] [image_name:tag_name]
  ```
- 정지된 컨테이너 모두 삭제
  ```
  sudo docker rm $(docker ps -a -q)
  ```
- `<none>` image 삭제
  ```
  docker rmi $(docker images -f "dangling=true" -q)
  ```
- DCT(Docker Content Trust) 활성화 / 비활성화 : 이미지가 변조되었는지를 체크하고 pull 하는 기능
  ```
  export DOCKER_CONTENT_TRUST=1                   활성화
  export DOCKER_CONTENT_TRUST=0                   비활성화
  ```
- Docker hub에 로그인
  ```
  docker login -u username -p passwd
  ```
- Docker hub에 이미지 업로드
  ```
  docker image push image_name:tag
  ```
- Docker hub에서 로그아웃
  ```
  docker logout
  ```
- Docker image 및 container를 tar파일로 추출
  ```
  docker export -o tar_file_name target_container
  ```
- Docker image 및 container를 tar파일로부터 가져오기
  ```
  docker import tar_file_name
  ```
- host 장치를 container 장치에 매핑하기
  ```
  docker run ... --device="/dev/sda1:/dev/sda1"
  ```
- timezone 변경
  - ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
  - (contiainer 안에서 `apt install tzdata`
  ```
  docker run \
  -e TZ=Asia/Seoul \
  -v /etc/localtime:/etc/localtime:ro \
  -v /etc/timezone:/etc/timezone:ro \
  ...
  ```
- ssh 설정
  1. host의 포트 맵핑
    - `docker run -p '10022:22'`
  2. container 내에서 openssh-server 설치
    - `apt install openssh-server`
  3. container 내에서 ssh config 변경
    - `vi /etc/ssh/sshd_config`에서 PermitRootLogin yes로 변경
  4. root 비밀번호 변경
    - `passwd root`
  5. ssh 서비스 시작
    - `service ssh start`
  
# Docker-compose
- 설치
  - $ `sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
  - $ `sudo chmod +x /usr/local/bin/docker-compose`
  - $ `sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`
  - $ `docker-compose --version`
- 기본 파일()을 설정파일로 사용하여 실행
  - `docker-compose up -d`
- 계속 재시작(restarting)되는 컨테이너 정지
  - `docker-compose down`
  
# nginx 빌드 및 실행 예제
1. Dockerfile 작성
  ```
  From ubuntu:14.04
  MAINTAINER Foo Bar foo@bar.com

  RUN apt-get update
  RUN apt-get install -y nginx
  RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
  RUN chown -R www-data:www-data /var/lib/nginx

  VOLUME ["\data", "/etc/nginx/site-enabled", "/var/log/nginx"]

  WORKDIR /etc/nginx

  CMD ["nginx"]

  EXPOSE 80
  EXPOSE 443
  ```
2. 빌드 하기
  ```
  sudo docker build --tag hello:0.1 .
  --tag 옵션으로 이미지 이름과 태그 설정 가능
  ```
3. 이미지 실행해보기
  ```
  sudo docker run --name hello-nginx -d -p 80:80 -v /root/data:/data hello:0.1
  -d 옵션 : 컨테이너를 백그라운드로 실행
  -p 80:80 옵션으로 호스트 80번 포트와 컨테이너 80번 포트를 연결하고 외부에 노출
  -v /root/data:/data 옵션으로 호스트의 /root/data 디렉토리를 컨테이너의 /data 디렉토리에 연결
  ```
4. 웹 페이지 접속
  웹 브라우저에 다음과 같은 주소로 접속
  ```
  http://localhost:80
  ```

# Rules
- /bin/bash 실행 파일에서 빠져나오면 해당 컨테이너는 정지된다

# Trouble Shooting
- 다음과 같은 에러 발생
  ```
  Creating network "dmojdockercompose_isolated_nw" with driver "bridge"
  ERROR: Pool overlaps with other one on this address space
  ```
  - 네트워크 충돌 문제로, `docker ps -a` 한 뒤 모든 사용하지 않는 컨테이너 삭제
  - [참고](https://github.com/maxking/docker-mailman/issues/85)

- `apt-get`이 작동하지 않음
  - 처음으로 이미지를 run 한뒤 `apt-get`을 사용하려면, `apt-get update` 를 먼저 실행해 주어야 함
  
- ` Error starting userland proxy: listen tcp 0.0.0.0:80: bind: address already in use` 에러가 발생
  - 이미 사용되고 있는 포트일 가능성이 있음
  - 다음과 같은 명령을 통해 사용되고 있는 80번 포트 검색
    - `sudo lsof -i -P -n | grep LISTEN`
  - nginx가 사용하고 있는 경우, nginx 서비스 종료
    - `sudo service nginx stop`

# Reference
- [docker](https://www.slideshare.net/pyrasis/docker-fordummies-44424016)
- [docker_compose](http://raccoonyy.github.io/docker-usages-for-dev-environment-setup/)
