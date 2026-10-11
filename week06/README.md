# Week06 - Dockerfile & ghcr.io 배포

## 1. 내 이미지 주소
- ghcr.io/soojin060926/guestbook:v1
- ghcr.io/soojin060926/guestbook:v2

## 2. 친구 이미지 실행 결과
- 친구 이미지: ghcr.io/친구아이디/guestbook:v1
- 실행 명령: docker run -d -p 9090:5000 --name friend ghcr.io/친구아이디/guestbook:v1
- (캡처 화면 첨부)

## 3. Dockerfile 설명
- FROM python:3.12-slim : 파이썬 3.12가 깔린 가벼운 이미지에서 시작
- WORKDIR /app : 컨테이너 안 작업 폴더를 /app으로 지정
- COPY requirements.txt . : 패키지 목록만 먼저 복사 (캐시 활용)
- RUN pip install -r requirements.txt : Flask, redis 패키지 설치
- COPY . . : 나머지 소스코드 전체 복사
- RUN useradd -m appuser : 일반 사용자 appuser 생성
- USER appuser : root가 아닌 appuser로 실행 (보안)
- ENV APP_TITLE, THEME_COLOR : 기본 제목과 테마 색 설정
- EXPOSE 5000 : 5000번 포트를 쓴다는 표시
- CMD ["python", "app.py"] : 컨테이너 시작 시 app.py 실행

## 4. 빌드 캐시 로그
- CACHED [2/6] WORKDIR /app
- CACHED [3/6] COPY requirements.txt .
- CACHED [4/6] RUN pip install --no-cache-dir -r requirements.txt
- [5/6] COPY . .
- 설명: requirements.txt가 안 바뀌어서 pip install은 캐시를 썼고, 소스가 바뀐 COPY . .부터 다시 실행됨

## 5. 설정을 넣는 세 가지 방법
| 방법 | 언제 확정 | 바꾸려면 |
|---|---|---|
| app.py 기본값 | 코드 | 코드 수정 후 재빌드 |
| Dockerfile ENV | 이미지 빌드 시점 | 재빌드 |
| docker run -e | 컨테이너 실행 시점 | 컨테이너만 다시 실행 |

- 우선순위: 코드 기본값 < Dockerfile ENV < docker run -e
- 실험 결과: -e로 THEME_COLOR를 주면 이미지에 구운 색보다 -e 색이 적용됨
