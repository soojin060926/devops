# 6주차: 도커파일

## 1. 이번 주 핵심 개념

- 도커파일 : 이미지를 만드는 레시피. "우분투로 준비해라 → 파이썬 설치해라 → 코드 넣어라 → 이렇게 실행해라"
  - `FROM` : 기반 이미지
  - `WORKDIR` : 작업 디렉터리 (없으면 생성)
  - `COPY 원본 대상` : 호스트 파일 → 이미지
  - `RUN` : 빌드할 때 실행
  - `ENV 이름=값` : 환경변수 기본값을 이미지에 넣는다
  - `EXPOSE` : 포트 번호를 문서화한다
  - `CMD` : 컨테이너가 시작할 때마다 실행 (↔ `RUN` = 이미지를 만들 때 한 번 실행)
- 레이어 캐시의 원리 : Dockerfile의 각 명령어는 하나의 레이어가 된다. 이전과 같은 명령어와 같은 파일이면 새로 만들지 않고 저장된 레이어를 재사용해서 빌드가 빨라진다
- 레이어 캐시의 순서의 중요성 : 어떤 레이어가 바뀌면 그 뒤의 레이어는 전부 캐시를 쓰지 못하고 다시 만들어진다. 그래서 자주 바뀌지 않는 명령어를 위에, 자주 바뀌는 명령어를 아래에 둔다
- 왜 requirements.txt를 먼저 복사하는가? : 코드는 자주 바뀌지만 requirements.txt는 잘 안 바뀐다. 먼저 복사해서 `pip install`까지 해 두면, 코드만 바뀔 때 패키지 설치 레이어는 캐시를 재사용해서 빌드가 빨라진다

## 2. 실습한 명령어

- `docker pull ghcr.io/makyraen/inhatc-devops-guestbook:v1` : GitHub 컨테이너 레지스트리(ghcr.io)에서 guestbook 이미지 v1을 내려받는다
- `docker run -d -p 8080:5000 --name demo ghcr.io/makyraen/inhatc-devops-guestbook:v1` : 내려받은 이미지를 백그라운드로 실행하고, 내 컴퓨터 8080 포트를 컨테이너의 5000 포트에 연결하며, 이름을 `demo`로 지정한다
- `unzip /mnt/c/Users/<윈도우계정>/Downloads/guestbook.zip` : 윈도우 다운로드 폴더에 있는 guestbook.zip 압축을 WSL에서 푼다
- `docker build -t guestbook:v1 .` : 현재 폴더의 Dockerfile로 이미지를 만들고 `guestbook:v1`이라는 이름과 태그를 붙인다
- `docker images | grep guestbook` : 이미지 목록 중 guestbook이 들어간 줄만 골라서 보여준다
- `cat > Dockerfile.bad << 'EOF'` : 터미널에서 `EOF`가 나올 때까지 입력한 내용을 `Dockerfile.bad` 파일로 저장한다 (캐시 순서를 일부러 나쁘게 한 비교용 Dockerfile을 만들 때 사용)
- `docker rmi guestbook:bad` : rmi = remove image. 이미지를 삭제한다 (비교: `docker rm`은 컨테이너 정리 시 사용)
- `gh auth token | docker login ghcr.io -u GitHub아이디 --password-stdin` : GitHub CLI로 토큰을 받아 Docker에 로그인한다
- `gh auth refresh -h github.com -s write:packages,read:packages` : GitHub 로그인 권한에 패키지 읽기/쓰기 권한을 추가한다
- `docker buildx build --platform linux/amd64,linux/arm64 ...` : 한 번에 여러 CPU 구조(amd64, arm64)용 이미지를 빌드한다

## 3. 어려웠던 점과 해결

- 레이어 캐시 순서가 왜 중요한지 처음에는 감이 오지 않았다. 일부러 순서를 나쁘게 만든 `Dockerfile.bad`와 비교해서 빌드해 보니, 코드만 바꿨는데도 패키지를 다시 설치하는 걸 보고 이해가 됐다.

## 4. 느낀 점

- Dockerfile 명령어 순서만 바꿔도 빌드 속도가 달라진다는 게 신기했다. 앞으로 Dockerfile을 쓸 때는 자주 바뀌는 파일을 아래쪽에 두도록 신경 쓰고 싶다.
