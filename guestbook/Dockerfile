# ---------------------------------------------------------------
# 학생 과제: 아래 각 줄이 무엇을 하는지 주석으로 설명을 달아보세요.
# ---------------------------------------------------------------

FROM python:3.12-slim
# 시작점이 되는 기본 이미지 => 파이썬 3.12가 설치된 가벼운 리눅스

WORKDIR /app
# 컨테이너의 작업 폴더를 /app 으로 정하고, 이후 명령이 실행되는 곳

COPY requirements.txt .
# 패키지 목록 파일만 먼저 /app 으로 복사한다
RUN pip install --no-cache-dir -r requirements.txt
# 목록의 패키지( Flask, redis ) 를 설치한다.
# ( --no-cache-dir 은 캐시를 남기지 않음 => 이미지 용량을 줄여준다. )

COPY . .
# 나머지 소스코드 전체 ( app.py, tempates 등 ) 를 /app으로 복사한다.

RUN useradd -m appuser
# 일반 사용자 appuser 을 생성한다.
USER appuser
# 이후부터 root 가 아닌 appuser 로 실행. (보안)

ENV APP_TITLE="InhaTC DevOps 방명록" \
    THEME_COLOR="#0054A6"
#  기본 환경 변수 값. 
# 기본 테마 = 파란색 ( #0054A6 ) 
# docker run -e 로 덮어쓸 수 있다.

EXPOSE 5000
# 이 컨테이너가 5000번 포트를 쓴다고 알리는 표시이다.
# 실제 연결은 -p가 수행.

CMD ["python", "app.py"]
# 컨테이너가 시작될 때 실행할 명령이다.
