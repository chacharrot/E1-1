# 개발 워크스테이션 구축 미션

터미널 → Docker → Git/GitHub 순으로 개발 환경을 구축하고,
전 과정을 명령어와 출력 증거로 기록한 저장소입니다.

---

## 1) 실행 환경

| 항목 | 버전/내용 |
|---|---|
| OS | 24.6.0 Darwin Kernel Version 24.6.0
| Shell | zsh
| Docker | Docker version 28.5.2, build ecc6942
| Git | git version 2.53.0
| 기타 | orbStack 사용.

> ⚠️ 재현 시 주의사항: <!-- 개인 PC 종속 경로/설정이 있다면 대체 방법 기록 -->

---

## 2) 수행 체크리스트

- [v] 터미널 기본 조작 및 폴더 구성
- [v] 권한 변경 실습 (chmod)
- [v] Docker 설치/점검
- [v] hello-world 실행
- [v] Dockerfile 직접 작성 및 빌드/실행
- [v] 포트 매핑 접속 검증 (2회: 서로 다른 포트)
- [v] 바인드 마운트 변경 반영 확인
- [v] 볼륨 영속성 검증 (컨테이너 삭제 전/후)
- [v] Git 설정 + VSCode GitHub 연동
- [ ] 트러블슈팅 2건 이상 기록

---

## 3) 터미널 기초 실습

### 3-1. 폴더 구성

```bash
$ pwd #절대경로 확인.
/Users/djmd33128278/Desktop/E1-1

$ ls -la  #리스트 확인. -l long format 확인 -a 모든파일 확인.
drwxr-xr-x  4 djmd33128278  djmd33128278   128  7 29 15:39 .
drwx------+ 6 djmd33128278  djmd33128278   192  7 29 13:18 ..
-rw-r--r--  1 djmd33128278  djmd33128278  4035  7 29 13:42 README.MD
$ mkdir -p ~/codyssey/practice
$ ls -la ~/codyssey
drwxr-xr-x  4 djmd33128278  djmd33128278   128  7 29 15:39 .
drwx------+ 6 djmd33128278  djmd33128278   192  7 29 13:18 ..
drwxr-xr-x  2 djmd33128278  djmd33128278    64  7 29 15:39 practice
-rw-r--r--  1 djmd33128278  djmd33128278  4035  7 29 13:42 README.MD

### 3-2. 파일 생성/복사/이동/삭제

```bash
$ touch hello
$ ls -al
drwxr-xr-x  5 djmd33128278  djmd33128278   160  7 29 15:55 .
drwx------+ 6 djmd33128278  djmd33128278   192  7 29 13:18 ..
-rw-r--r--  1 djmd33128278  djmd33128278     0  7 29 15:55 hello
drwxr-xr-x  2 djmd33128278  djmd33128278    64  7 29 15:55 practice
-rw-r--r--  1 djmd33128278  djmd33128278  4467  7 29 15:54 README.MD
$ echo 123
123
$ echo hello world! > hello
$ cat hello
hello world!
$ cp hello hellocp
$ ls -al
total 32
drwxr-xr-x  6 djmd33128278  djmd33128278   192  7 29 15:58 .
drwx------+ 6 djmd33128278  djmd33128278   192  7 29 13:18 ..
-rw-r--r--  1 djmd33128278  djmd33128278    12  7 29 15:56 hello
-rw-r--r--  1 djmd33128278  djmd33128278    12  7 29 15:58 hellocp
drwxr-xr-x  2 djmd33128278  djmd33128278    64  7 29 15:55 practice
-rw-r--r--  1 djmd33128278  djmd33128278  4467  7 29 15:54 README.MD
$ mv hellocp ./pratice
$ ls -al
total 24
drwxr-xr-x  5 djmd33128278  djmd33128278   160  7 29 15:59 .
drwx------+ 6 djmd33128278  djmd33128278   192  7 29 13:18 ..
-rw-r--r--  1 djmd33128278  djmd33128278    12  7 29 15:56 hello
drwxr-xr-x  3 djmd33128278  djmd33128278    96  7 29 15:59 practice
-rw-r--r--  1 djmd33128278  djmd33128278  4467  7 29 15:54 README.MD
$ cd practice
$ pwd
~/practice
$ ls -al
total 8
drwxr-xr-x  3 djmd33128278  djmd33128278   96  7 29 16:04 .
drwxr-xr-x  5 djmd33128278  djmd33128278  160  7 29 15:59 ..
-rw-r--r--  1 djmd33128278  djmd33128278   12  7 29 16:04 hellocp
$ cat hellocp
hello world!
$ rm hellocp
$ ls -al
drwxr-xr-x  2 djmd33128278  djmd33128278   64  7 29 16:03 .
drwxr-xr-x  5 djmd33128278  djmd33128278  160  7 29 15:59 ..



```

### 3-3. 권한 변경 (전 → 명령어 → 후)

```bash
$ ls -l hello
<!-- 변경 전 --> -rw-r--r--  1 djmd33128278  djmd33128278  12  7 29 15:56 hello

$ chmod 755 hello.txt
$ ls -l hello.txt
<!-- 변경 후 --> -rwxr-xr-x  1 djmd33128278  djmd33128278  12  7 29 15:56 hello
```

> 💡 배운 점: r 읽기 4 w 쓰기 2 x 실행 1 권한. 처음부터 소유자, 그룹, 기타사용자 순. 

---

## 4) Docker 설치 및 기본 실행

```bash
$ docker --version
Docker version 28.5.2, build ecc6942

$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

$ docker ps -a
djmd33128278@c4r3s1 E1-1 % docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
d1051a2ee2b8   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             beautiful_carson
a0bff801c991   hello-world   "/hello"   22 minutes ago       Exited (0) 22 minutes ago                 kind_rubin
---
```

## 5) Dockerfile 작성 및 빌드

### 5-1. 폴더 구조

```
nginxdoc/
├── Dockerfile
└── site/
    └── index.html
```

### 5-2. Dockerfile (각 줄 설명 포함)

```
FROM nginx:alpine ##nginx alpine 버전 다운로드 및 사용 

COPY ./site/ /usr/share/nginx/html/ ## dockerfile 상대경로 /site/ 에 있는 파일 컨테이너 절대경로로 복사.

EXPOSE 80    ##명시적으로 80번 port를 쓸꺼라 해뒀지만 안써도 무관.
```

### 5-3. 빌드 로그

```bash
$ docker build -t un1 . ##-t 태그: 이름 및 버전 작성. 없이하면 고유 ID값으로 실행 해야함.
[+] Building 2.0s (5/5) FINISHED                                                                                         docker:orbstack
 => [internal] load build definition from Dockerfile                                                                                0.1s
 => => transferring dockerfile: 70B                                                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                    1.5s
 => [internal] load .dockerignore                                                                                                   0.0s
 => => transferring context: 2B                                                                                                     0.0s
 => CACHED [1/1] FROM docker.io/library/ubuntu:latest@sha256:3131b4cc82a783df6c9df078f86e01819a13594b865c2cad47bd1bca2b7063bb       0.0s
 => exporting to image                                                                                                              0.1s
 => => exporting layers                                                                                                             0.0s
 => => writing image sha256:0189053db6a0c9fc074981a188bda38efa1f81a1bb02b0914f0bb54fe3bf0df2                                        0.0s
 => => naming to docker.io/library/un1                                                                                              0.0s
```

 ### 5-4 docker images 기록
```bash
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
un1          latest    0189053db6a0   2 weeks ago   100MB
```

### 5-5 docker run 컨테이너 생성 기록
```bash
$ docker run -d -it -p 8080:80 --name untest1 un1 ## -d 백그라운드 실행, -i interactive 통신성. -t TTY 터미널 유지. -p 포트포워딩, 포트맵핑
8136b2571b5d6ddaa50cc23b210feabf4e159e769e13c8f05acd32215caa5324
```
### 5-6 docker ps 컨테이너 프로세스 확인. 

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS                                     NAMES
8136b2571b5d   un1       "/bin/bash"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   untest1
```

### 5-7 dokcer logs 컨테이너 로그 확인.
```bash
$ docker logs -f untest1  # -f 지속적으로 로그 확인. 다른 터미널로 입력함.
root@8136b2571b5d:/# mkdir test
root@8136b2571b5d:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  test  usr
boot  etc  lib   media  opt  root  sbin  sys  tmp   var
root@8136b2571b5d:/# good
bash: good: command not found
root@8136b2571b5d:/# ^C
root@8136b2571b5d:/# ^C
root@8136b2571b5d:/# exit
exit
```

### 5-8 docker stats 도커 실시간 리소스 사용량 확인 
```bash
djmd33128278@c4r4s1 unbuntudo % docker stats --no-stream ## --no-stream 옵션 없이 하면 지속적으로 확인, 옵션 있을시 현재시점 한번 출력.
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS
8136b2571b5d   untest1   0.00%     3.918MiB / 15.67GiB   0.02%     1.13kB / 126B   18.3MB / 0B   1
```

### 5-9 docker attach, exec 실행 기록.
```bash
$ docker exec untest1 ls
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
test
tmp
usr
var
$ docker exec untest1 echo hello world 
hello world
$ docker attach untest1 
root@8136b2571b5d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
root@8136b2571b5d:/# echo hello world
hello world
root@8136b2571b5d:/# 
```

---

## 6) 포트 매핑 검증 (2회)

### 6-1. 첫 번째 포트 (8080)

```bash
$ docker build -t ng1 .
[+] Building 2.2s (7/7) FINISHED                                                                                                                       docker:orbstack
 => [internal] load build definition from Dockerfile                                                                                                              0.1s
 => => transferring dockerfile: 102B                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                   1.6s
 => [internal] load .dockerignore                                                                                                                                 0.0s
 => => transferring context: 2B                                                                                                                                   0.0s
 => [internal] load build context                                                                                                                                 0.1s
 => => transferring context: 61B                                                                                                                                  0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752                                             0.0s
 => CACHED [2/2] COPY ./site/ /usr/share/nginx/html/                                                                                                              0.0s
 => exporting to image                                                                                                                                            0.0s
 => => exporting layers                                                                                                                                           0.0s
 => => writing image sha256:4f44d3fb28559d1766f267056d29fed67c4adf00d06f86736cdbe11725cde882                                                                      0.0s
 => => naming to docker.io/library/ng1 
$ docker run -d -p 8080:80 --name ngtest1 ng1
16ea41b80f6c50e2b37d807f48c19f430b8dd32f2d37622dcec1ca26427212c3
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head><title>My Docker Web</title></head>
<body>
  <h1>Hello Docker!</h1>
  <p>test for port mapping page</p>
</body>
</html>%                    
```

📸 브라우저 접속 증거 (주소창 포트 포함):

![8080 접속](./images/port-8080.png)

### 6-2. 두 번째 포트 (8081)

```bash
$ docker run -d -p 8081:80 --name ngtest2 ng1
32a5aa8c880c5e4ec494d22a714f8f7e97d81411027132d2ed09c859c9949398
$ curl http://localhost:8081
<!DOCTYPE html>
<html>
<head><title>My Docker Web</title></head>
<body>
  <h1>Hello Docker!</h1>
  <p>test for port mapping page</p>
</body>
</html>%       
```

![8081 접속](./images/port-8081.png)

> 💡 관찰: 같은 이미지로 서로 다른 포트의 컨테이너 2개가 동시에 동작 → 이미지와 컨테이너의 분리 확인

```bash
$ docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED              STATUS              PORTS                                     NAMES
32a5aa8c880c   ng1       "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   ngtest2
16ea41b80f6c   ng1       "/docker-entrypoint.…"   4 minutes ago        Up 4 minutes        0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   ngtest1
```
---

## 7) 바인드 마운트 변경 반영

```bash
$ docker run -d -v ./site:/usr/share/nginx/html -p 8083:80 --name ngtest3 ng1. ## -v 호스트경로:컨테이너절대경로. 폴더 파일을 공유함.
693b5dbbb7da97921266dae1d37e5ab69973078298db153e46680415b8e3f991
$ curl http://localhost:8083/
<!DOCTYPE html>
<html>
<head><title>My Docker Web</title></head>
<body>
  <h1>Hello Docker!</h1>
  <p>test for port mapping page</p>
</body>
<html>%                                                                                                                                                                                                        
$ echo "<html><p>if i fix this?</p></html>" > ./site/index.html  
$ curl http://localhost:8083/                                   
<html><p>if i fix this?</p></html>
```

---

## 8) 볼륨 영속성 검증

```bash
$ docker volume create E1                                                    
E1
$ docker volume ls
DRIVER    VOLUME NAME
local     E1
$ docker run -v E1:/data -it --name untest2 un1  ## -v 볼륨이름:컨테이너 절대경로
root@044c52af26fc:/# echo "hi voloume" >  /data/volume.txt
root@044c52af26fc:/# cat /data/volume.txt 
hi voloume
root@044c52af26fc:/# exit
exit
$ docker run -v E1:/data -it --name untest3 un1
root@f86dbda80a3c:/# cat /data/volume.txt 
hi voloume
```

> 💡 결론: 컨테이너를 삭제해도 볼륨의 데이터는 유지됨을 확인

---

## 9) Git / GitHub 연동

```bash
$ git config --global user.name "chacharrot"
$ git config --global user.email "djmd3312@gmail.com"
$ git config --list
credential.helper=osxkeychain
user.name=chacharrot
user.email=djmd3312@gmail.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.E1.url=https://github.com/chacharrot/E1-1.git
remote.E1.fetch=+refs/heads/*:refs/remotes/E1/*
```

📸 VSCode GitHub 연동 증거:

![vscode 연동](./images/vscode-github.png)

---

## 10) 트러블슈팅 (2건 이상)

### 사례 1: docker run 을 실행하면 컨테이너 터미널이 바로 뜸.
| 구분 | 내용 |
|---|---|
| 문제 | run 을 실행하고 컨테이너 터미널에 바로 들어가져 터미널 하나로 작업을 할 수 없음.
| 가설 | 바로 접속 하는게 아닌 실행만 하는 옵션이 있나?
| 확인 | -d 옵션을 통해 백그라운드로 실행만 가능.
| 해결 | docker run -d 옵션으로 실행하면 터미널 하나로 작업 가능.

### 사례 2: <!-- 제목 -->

| 구분 | 내용 |
|---|---|
| 문제 |dokcerfile 로 build를 했는데 FROM 단계부터 막힘. |
| 가설 | 이미지 다운로드 이름을 오타 쳤나?|
| 확인 | 더이상 배포하지 않는 이미지 파일이라 다운로드 자체가 불가능.|
| 해결 | ubuntu최신 버전으로 dockerfile수정|

---
