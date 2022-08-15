# Docker

## VM과 Docker 비교

### Virtual Machine

- hypervisor 소프트웨어를 통해 만들어짐
    - vmware
    - VirtualBox 등
- 고립된 환경에서 어플리케이션을  구동하기 위해 앱 별 vm 필요
- OS가 내장되어 무거움

### Docker

- 컨테이너 엔진의 한 종류
- 기존 OS에서 컨테이너 엔진을 사용
- 컨테이너 생성을 통해 어플리케이션을 고립된 환경에서 구동할 수 있음
- OS를 내장하지 않아 가벼움

## Docker의 3요소

### Dockerfile

- 구성요소
    - 필요한 파일 목록
    - 종속성
        - 필요 라이브러리
        - 필요 프레임워크
    - 환경병수
    - 구동 스크립트

### Image

- dockerfile 통해 생성된 이미지
- 실행되고 있는 어플리케이션 상태의 스냅샷
    - 불변의 상태
- 구성요소
    - 런타임 환경
    - 시스템 툴
    - 시스템 라이브러리

### Container

- 이미지를 고립된 환경에서 실행
- 스냅샷된 이미지를 통해 어플리케이션을 구동
- 클래스(이미지)를 통해 인스턴스(컨테이너)를 만드는 것과 유사

## Dockerfile 생성

### 기본 문법

- `FROM`
    - 도커에서 사용될 기본 이미지
        - 기본 리눅스 이미지
            - `FROM baseImage`
        - node 공식 제공 이미지
            - `FROM node:16-alpine3.11`
- `WORKDIR`
    - 컨테이너 내부에서 어떤 경로의 이미지를 실행할 것인지 명시
        - `WORKDIR /app`
    - `cd` 와 동일한 역할
- `COPY`
    - 변경된 디렉토리로 명시된 파일들을 복사
        - `COPY package.json package-lock.json ./`
- `RUN`
    - 스크립트 실행
        - `RUN npm install`
            - 버전이 명시되지 않은경우 최신 버전 설치
        - `RUN npm ci`
            - package-lock.json에 명시된 버전 설치
- `ENTRYPOINT`
    - 실행환경 및 파일 목록
        - `ENTRYPOINT [ "node", "index.js" ]`

### 주의사항

- dockerfile은 레이어 형태로 작성할 것
    - 순차적으로 실행됨
    - 가장 빈번히 변경될수록 마지막에 작성하기
        - 낮은 레이어 재사용 가능
        

## 이미지 생성

### 문법

`docker build -f Dockerfile -t docker-basic .`

- 사용하고자 하는 도커 파일 선택
    - `-f Dockerfile`
- 도커 파일에 태그 부여
    - `-t docker-basic`
- 경로
    - `.` - 현재 경로에서 명령어 실행
    

## 이미지 실행

### 문법

`docker run -d -p 8080:8080 docker-basic`

- 백그라운드에서 수행
    - `-d`
- 포트 지정
    - `-p 8080:8080`
        - 호스트 머신의 포트와 컨테이너 포트를 연결

### 명령어

- 실행중인 컨테이너 확인
    - `docker ps`
- 실행중인 컨테이너 로그
    - `docker logs <CONTAINER ID>`
        - docker ps에서 확인된 컨테이너 ID
- 이미지 이름 변경
    - docker tag <기존 이미지 이름>:<기존 태그 이름> <변경할 이미지 이름>:<변경할 태그 명>
    - `docker tag docker-basic:latest cheonaru/docker-basic:latest`
    - docker hub 업로드 시에는 이미지 이름을 `<계정명>/<이미지 이름>:<태그명>` 형식으로 맞춰야함

## 레퍼런스

- Docker Doc
    - [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
    - [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [드림코딩 유튜브](https://www.youtube.com/watch?v=LXJhA3VWXFA)
    
- notion-github-issue-sync
    
    [https://github.com/noah0316/notion-github-issue-sync](https://github.com/noah0316/notion-github-issue-sync)
    
    [Docker Hub](https://hub.docker.com/r/noah0316/notion-github-issue-sync)
    
- [인프런 유료강의](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci#curriculum)(필요하면 수강하기)

## TODO

- notion-github-issue-sync 참고하여 PR 시 Notion DB에 등록되도록 만들기
- 깃허브에 md로 작성한 TIL notion에 데이터베이스로 정리 가능?
