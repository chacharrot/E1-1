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
| 기타 | <!-- OrbStack 사용 여부 등 --> |

> ⚠️ 재현 시 주의사항: <!-- 개인 PC 종속 경로/설정이 있다면 대체 방법 기록 -->

---

## 2) 수행 체크리스트

- [v] 터미널 기본 조작 및 폴더 구성
- [v] 권한 변경 실습 (chmod)
- [v] Docker 설치/점검
- [v] hello-world 실행
- [ ] Dockerfile 직접 작성 및 빌드/실행
- [ ] 포트 매핑 접속 검증 (2회: 서로 다른 포트)
- [ ] 바인드 마운트 변경 반영 확인
- [ ] 볼륨 영속성 검증 (컨테이너 삭제 전/후)
- [ ] Git 설정 + VSCode GitHub 연동
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
$ docker build -t my-web:1.0 .
<!-- 출력 -->
```

---

## 6) 포트 매핑 검증 (2회)

### 6-1. 첫 번째 포트 (8080)

```bash
$ docker run -d -p 8080:80 --name my-web-8080 my-web:1.0
$ curl http://localhost:8080
<!-- 출력 -->
```

📸 브라우저 접속 증거 (주소창 포트 포함):

![8080 접속](./images/port-8080.png)

### 6-2. 두 번째 포트 (8081)

```bash
$ docker run -d -p 8081:80 --name my-web-8081 my-web:1.0
$ curl http://localhost:8081
<!-- 출력 -->
```

![8081 접속](./images/port-8081.png)

> 💡 관찰: 같은 이미지로 서로 다른 포트의 컨테이너 2개가 동시에 동작 → 이미지와 컨테이너의 분리 확인

---

## 7) 바인드 마운트 변경 반영

```bash
<!-- -v $(pwd)/site:/usr/share/nginx/html 실행 →
     호스트에서 index.html 수정 → curl로 변경 반영 확인 로그 -->
```

**변경 전/후 비교:**
<!-- 수정 전 curl 결과 vs 수정 후 curl 결과 -->

---

## 8) 볼륨 영속성 검증

```bash
$ docker volume create mydata
<!-- 데이터 쓰기 → 컨테이너 삭제 → 새 컨테이너에서 데이터 확인 로그 -->
```

> 💡 결론: 컨테이너를 삭제해도 볼륨의 데이터는 유지됨을 확인

---

## 9) Git / GitHub 연동

```bash
$ git config --global user.name "이름"
$ git config --global user.email "이메일"
$ git config --list
<!-- 출력 (⚠️ 민감정보 마스킹!) -->
```

📸 VSCode GitHub 연동 증거:

![vscode 연동](./images/vscode-github.png)

---

## 10) 트러블슈팅 (2건 이상)

### 사례 1: <!-- 제목 -->

| 구분 | 내용 |
|---|---|
| 문제 | <!-- 발생한 에러/현상 --> |
| 가설 | <!-- 원인 추측 --> |
| 확인 | <!-- 확인한 명령어/방법 --> |
| 해결 | <!-- 해결 과정과 결과 --> |

### 사례 2: <!-- 제목 -->

| 구분 | 내용 |
|---|---|
| 문제 | |
| 가설 | |
| 확인 | |
| 해결 | |

---

## 11) 배운 점 / 회고

<!-- exec vs attach 차이, 이미지 vs 컨테이너 등 개념 정리 -->