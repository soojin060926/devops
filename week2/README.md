# 2주차

## 오늘 배운 내용

- 셀 스크립트 ( 여러 명령어를 파일 하나에 모아 두고 한 번에 실행하는 방법 )
- Qwen ( 중국 알리바바에서 만든 AI 언어 모델 시리즈 이름 (Qwen 3세대) )
- Ollama ( 로컬 환경에서 대규모 언어 모델 (LLM)을 쉽게 설치 및 실행할 수 있게 해주는 오픈소스 플랫폼 )

## 새로 배운 명령어

```bash
ollama serve
ollama pull qwen3:0.6b
explorer.exe .
python3 chat.py
nano chat.py
nano start.sh
command -v
```

- `ollama serve` : Ollama(내 컴퓨터에서 AI 모델을 돌리는 프로그램)의 서버를 실행한다
- `ollama pull qwen3:0.6b` : qwen3:0.6b라는 작은 AI 모델을 내려받는다
- `explorer.exe .` : WSL에서 현재 폴더를 윈도우 탐색기로 연다
- `python3 chat.py` : chat.py 파이썬 파일을 실행한다
- `nano chat.py` : chat.py 파일을 nano 편집기로 열어서 만들거나 수정한다
- `nano start.sh`: start.sh를 편집기로 열어 작성한다
- `command -v` : 명령어를 실행할 수 있으면, 어느 경로에 있는지 확인. 없는 명령어라면, 아무것도 출력하지 않고 실패 상태를 반환

## 어려웠던 점과 해결
- start.sh를 실행하려는데 권한 오류가 났다. `bash start.sh`로 실행하니 해결됐다.

## 느낀 점

- 직접 AI모델을 내려받아 대화까지 해보니 답변이 정확하진 않았지만 신기하였다.
- 작은 모델이라 정확도는 낮았지만, 더 큰 모델도 돌려보고 싶다.
