# AI/SW 개발 워크스테이션 구축

## 1. 프로젝트 개요
터미널, Docker, Git을 활용해 어디서나 재현 가능한 실행 환경을 구축하고, 협업과 배포의 기반이 되는 인프라 설계 원칙을 체득한다.

## 2. 실행 환경
- **OS**: Windows 11 (Build 26100)
- **Shell**: PowerShell 5.1
- **Terminal**: VSCode Integrated Terminal
- **Docker**: Docker version 29.3.1, build c2be9cc
- **Git**: git version 2.53.0.windows.2

## 3. 수행 항목 체크리스트
- [x] **터미널**
- [x] **권한**
- [x] **Docker**
- [x] **Dockerfile**
- [ ] **포트**
- [ ] **볼륨**
- [ ] **마운트**
- [x] **Git**
- [ ] **GitHub**

## 4. 검증 방법 및 결과 위치   
| 수행 항목 | 검증 방법 (수행 명령어 및 확인 내용) | 결과 위치 링크 |
| :--- | :--- | :--- |
| **터미널** | `pwd`, `ls -Force`, `cd`, `mkdir`, `cp`, `mv`, `rm` 등 기본 명령어를 통한 파일 시스템 조작 및 파일 생성(`ni`, `echo`)/내용 확인(`cat`) 검증 | [터미널 실습 로그](#41-터미널-기본-조작-로그) |
| **권한** | `ls -l`의 권한 비트(`-rw-r--r--`) 변화를 통해 `chmod` 변경 전/후 비교 | [권한 변경 로그](#42-권한-변경-실습-로그) |
| **Docker** | `docker --version` 실행을 통해 도커 엔진 설치 및 클라이언트 응답 확인 | [실행 환경](#43-실행-환경) |
| **Dockerfile** | `docker run` 실행 결과 및 `docker images` 목록 내 생성 이미지 확인 | [이미지 빌드 로그](#44-dockerfile-빌드-및-이미지-확인) |
| **포트** | `docker ps`로 포트(`8080`) 상태 확인 및 브라우저 접속 결과 검증 | [서비스 검증 로그](#45-포트-및-볼륨-검증) |
| **볼륨/마운트** | 컨테이너 재시작 후 마운트된 경로 내 파일 보존 여부 확인 | [서비스 검증 로그](#45-포트-및-볼륨-검증) |
| **Git/GitHub** | `git log` 커밋 이력 및 GitHub Repository URL 연결 상태 확인 | [Git 연동 정보](#46-git-및-github-최종-연동) |

### 4.1 터미널 기본 조작 로그
# 1. 현재 위치 확인 
- 입력한 명령어<br>```PS C:\Users\MYPC> pwd```
- 출력 결과<br> ```Path```

# 2. 폴더 생성
- 입력한 명령어<br>```PS C:\Users\MYPC> mkdir test1```
- 출력 결과<br>
```
Mode                 LastWriteTime         Length 
----                 -------------         ------ 
d-----      2026-04-02   오전 9:39                
```

# 3. 목록 확인 (숨김 파일 포함)
- 입력한 명령어<br>```PS C:\Users\MYPC> ls -Force```
- 출력 결과<br>
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-04-02   오전 8:05                .copilot
d-----      2026-04-02   오전 8:24                .docker
d-----      2026-04-02   오전 10:15                test1
```

# 4. 폴더 이동
- 입력한 명령어<br>```PS C:\Users\MYPC> cd test1```
- 출력 결과<br>
```
PS C:\Users\MYPC\test1> 
```

# 5. 폴더 복사
- 입력한 명령어<br>```PS C:\Users\MYPC> cp -r test1 test2```
- 출력 결과<br>
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-04-02   오전 9:39                test1
d-----      2026-04-02  오전 10:05                test2
```

# 6. 폴더 이름 변경
- 입력한 명령어<br>```PS C:\Users\MYPC> mv test2 test3```
- 출력 결과<br>
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-04-02   오전 9:39                test1
d-----      2026-04-02  오전 10:05                test3
```

# 7. 폴더 이동
 입력한 명령어<br>```PS C:\Users\MYPC> mv test3 test1/```
- 출력 결과<br>
```
PS C:\Users\MYPC\test1> ls

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-04-02  오전 10:05                test3
```

# 8.폴더 삭제
- 입력한 명령어<br>```PS C:\Users\MYPC\test1> rm test3```
- 출력 결과<br>
```
PS C:\Users\MYPC\test1> ls
PS C:\Users\MYPC\test1>
```

# 9. 빈 파일 생성
- 입력한 명령어<br>```PS C:\Users\MYPC\test1> ni file1```
- 출력 결과<br>
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-04-02  오전 10:24              0 file1
```

# 8. 파일 내용 확인
- 입력한 명령어<br>```PS C:\Users\MYPC\test1> cat file1```
- 출력 결과<br>
```
hi
```

### 4.2 권한 변경 실습 로그
# 1. 디렉토리/파일 생성 및 권한 확인
- 입력한 명령어
```
root@534e669a1496:/# mkdir my_folder
root@534e669a1496:/# touch my_file.txt
root@534e669a1496:/# ls -l
```
- 출력 결과<br>
```
-rw-r--r--   1 root root    0 Apr  2 02:28 my_file.txt
drwxr-xr-x   2 root root 4096 Apr  2 02:28 my_folder
```

# 2.권한 변경
- 입력한 명령어
```
root@534e669a1496:/# chmod 777 my_folder
root@534e669a1496:/# chmod 444 my_file.txt
root@534e669a1496:/# ls -l
```
- 출력 결과
```
-r--r--r--   1 root root    0 Apr  2 02:28 my_file.txt
drwxrwxrwx   2 root root 4096 Apr  2 02:28 my_folder
```

### 4.3 실행 환경
# 1. Docker 버전 확인
- 입력한 명령어
```
PS C:\Users\MYPC> docker --version
```
-출력 결과
```
Docker version 29.3.1, build c2be9cc
```

# 2. Docker 데몬 동작 여부 확인
- 입력한 명령어
```
PS C:\Users\MYPC> docker info
```
- 출력 결과
```
Client:
 Version:    29.3.1
 Context:    desktop-linux
 Debug Mode: false
Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 29.3.1
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
```

### 4.4 dockerfile 빌드 및 이미지 확인
# 1. 이미지 다운로드/목록 확인
- 입력한 명령어
```
PS C:\Users\MYPC> docker images
```
- 출력 결과
```
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
ubuntu:latest   186072bba1b2        119MB         31.7MB
```

# 2. 컨테이너 실행/중지/목록 확인 
# 2-1. 컨테이너 실행
- 입력한 명령어
```
PS C:\Users\MYPC> docker run -d --name test ubuntu sleep 1000   
```
- 출력 결과
```
d096b318b9bd5947c2485c4feeb1eb7e724d0a7ee22de5a3fc3d2f9ea8cae51f
```

# 2-2. 컨테이너 목록
- 입력한 명령어
```
PS C:\Users\MYPC> docker ps
```
- 출력 결과
```
CONTAINER ID   IMAGE     COMMAND        CREATED              STATUS              PORTS     NAMES
d096b318b9bd   ubuntu    "sleep 1000"   About a minute ago   Up About a minute             test
```

# 2-3. 컨테이너 중지
- 입력한 명령어
```
PS C:\Users\MYPC> docker stop test
```
- 출력 결과
```
test
```

# 3. 운영 로그 확인
- 입력한 명령어
```
PS C:\Users\MYPC> docker logs test
```
- 출력 결과
```

```

# 4. 리소스 확인
- 입력한 명령어
```
docker stats --no-stream
```
- 출력 결과
```
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O   PIDS                                                                                       
d096b318b9bd   test      0.00%     400KiB / 7.703GiB   0.00%     1.17kB / 126B   0B / 0B     1
```

# 5. 컨테이너 실행 실습 
# 5-1. hello-world 실행 
- 입력한 명령어
```
PS C:\Users\MYPC> docker run hello-world
```
- 출력 결과
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete
d5e71e642bf5: Download complete
Digest: sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```
<img width="948" height="79" alt="image" src="https://github.com/user-attachments/assets/9b4591e6-670c-4e81-bac1-f1538b1efa04" />

# 5-2. ubuntu 컨테이너를 실행하고 내부 진입 후 간단 명령
- 입력한 명령어
```
root@af5a50527ece:/# ls
```
- 출력 결과
```
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr``

```
- 입력한 명령어
```
root@af5a50527ece:/# echo "hello"
```
- 출력 결과
```
hello
```

# 5-3 컨테이너 종료/유지(attach/exec 등)의 차이







### 4.5 포트 및 볼륨 검증












- 입력한 명령어
```

```
- 출력 결과
```

```






















# 2. 이미지 다운로드 확인

## 5. 트러블슈팅 2건 이상(문제 → 원인 가설 → 확인 → 해결/대안)
