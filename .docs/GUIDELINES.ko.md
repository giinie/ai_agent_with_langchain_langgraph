# ai-agent-test 프로젝트 가이드라인 (Windows/PowerShell)

본 문서는 D:\JWS\PyCharm\ai_agent_test 저장소에서 LangChain, LangGraph, Autogen, CrewAI, LlamaIndex 등으로 작성된 노트북 중심 워크플로우를 원활히 사용하기 위한 프로젝트 전용 가이드라인입니다. 모든 예시는 Windows 10/11 + PowerShell 환경을 기준으로 합니다.


## 1. 목적과 범위
- 이 저장소는 AI 에이전트 관련 프레임워크(Autogen, LangChain, LangGraph, CrewAI, LlamaIndex, AutoGPT) 실습용 노트북을 제공합니다.
- 코드 배포형 패키지가 아니라 학습/실험 중심이며, 노트북 실행 환경 구성과 키 관리, 실행 규칙, 커밋 정책을 다룹니다.


## 2. 요구 사항
- Python 3.12 이상(권장: 3.12)
- Windows PowerShell
- 선택: uv(의존성 잠금/동기화), Jupyter (또는 PyCharm/VS Code의 노트북 실행 기능)


## 3. 개발 환경 설정(Windows PowerShell)
프로젝트 루트: `D:\JWS\PyCharm\ai_agent_test`

### 3.1 uv 기반(권장)
1) uv 설치(둘 중 하나)
- Winget: `winget install Astral.Uv`
- 또는 pip: `pip install uv`

2) 의존성 동기화(프로젝트 루트에서 실행)
- `uv sync --frozen`
  - uv.lock을 엄격히 존중합니다. (.venv 가상환경이 자동 생성됩니다.)

3) 가상환경 활성화
- `.\.venv\Scripts\Activate.ps1`

4) (필요 시) 노트북 도구 설치
- `uv pip install jupyter ipykernel`
- `python -m ipykernel install --user --name ai-agent-test`

### 3.2 pip 기반(대안)
uv 사용이 어려운 경우만 사용하세요. pyproject.toml의 의존성 목록을 직접 설치합니다.
1) 가상환경 생성 및 활성화
- `py -3.12 -m venv .venv`
- `.\.venv\Scripts\Activate.ps1`

2) pip 업그레이드
- `pip install -U pip`

3) 필요한 패키지 설치(한 줄 실행)
- `pip install "langchain>=0.3.27" "langchain-community>=0.3.27" "langchain-core>=0.3.72" "langchain-experimental>=0.3.4" "langchain-openai>=0.3.28" "langchain-text-splitters>=0.3.9" "openai>=1.99.3" "wikipedia>=1.4.0"`
- (노트북 실행용) `pip install jupyter ipykernel`
- `python -m ipykernel install --user --name ai-agent-test`


## 4. API 키 및 환경 변수 설정
일부 노트북은 외부 API 키가 필요합니다. 키는 커밋 금지이며, 환경 변수로만 주입하세요.

### 4.1 세션(현재 PowerShell 세션에서만 유효)
- OpenAI: `$env:OPENAI_API_KEY = "sk-..."`
- LangSmith: `$env:LANGSMITH_API_KEY = "lsv2-..."`; `$env:LANGSMITH_TRACING = "true"`; `$env:LANGSMITH_PROJECT = "ai-agent-test"`
- Tavily: `$env:TAVILY_API_KEY = "tvly-..."`

### 4.2 영구(사용자 프로필 범위)
- `[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-...", "User")`
- `[Environment]::SetEnvironmentVariable("LANGSMITH_API_KEY", "lsv2-...", "User")`
- `[Environment]::SetEnvironmentVariable("LANGSMITH_TRACING", "true", "User")`
- `[Environment]::SetEnvironmentVariable("LANGSMITH_PROJECT", "ai-agent-test", "User")`
- `[Environment]::SetEnvironmentVariable("TAVILY_API_KEY", "tvly-...", "User")`

참고
- LangChain 최신 버전에서는 LangSmith 연동이 기본입니다. (과거 `LANGCHAIN_TRACING_V2` → 현재 `LANGSMITH_TRACING` 권장)
- 키 문자열은 절대 저장소에 추가하지 마세요.


## 5. 노트북 실행
1) 가상환경 활성화: `.\.venv\Scripts\Activate.ps1`
2) (필요 시) Jupyter 설치: `uv pip install jupyter` 또는 `pip install jupyter`
3) 실행:
   - `jupyter lab` 또는 `jupyter notebook`
   - PyCharm/VS Code의 내장 기능을 사용해도 됩니다.
4) 커널 선택: `ai-agent-test` 커널(혹은 현재 .venv 커널)을 선택하세요.


## 6. 노트북/코드 스타일 가이드
- 출력 정리: 커밋 전 반드시 출력 비우기
  - UI: Cell → All Output → Clear
  - CLI: `jupyter nbconvert --clear-output --inplace LangChain\랭체인에서_에이전트_사용하기.ipynb`
- 실행 순서: 위에서 아래로 일관 실행 후 저장(Out 번호 역전 금지)
- 비밀정보: 키/토큰/쿠키/개인정보를 노트북/코드/출력에 남기지 않기
- 파일명: 공백 대신 `_` 사용 권장, 한국어 파일명은 가능하나 경로/인코딩 문제에 유의
- 코드 셀: 재실행 가능하도록 의존 순서를 명확히, 하드코딩 지양
- 마크다운: 제목/목차/전제조건/실행방법/결과/참고 링크를 간단히 정리


## 7. 저장소 구조 안내(요약)
- `Autogen\`/`CrewAI\`/`LangChain\`/`LangGraph\`/`LangSmith\`/`LlamaIndex\`: 각 프레임워크별 예제 노트북
- `.docs\memo.md`: 학습 메모 문서
- `.docs\GUIDELINES.ko.md`: 본 가이드라인
- `pyproject.toml`: 의존성/프로젝트 메타정보(uv 사용 시 `uv sync` 기반)
- `uv.lock`: 의존성 잠금 파일(수정 금지)
- `.gitignore`: 가상환경/IDE 디렉토리 제외


## 8. 커밋/PR 체크리스트
- [ ] 노트북 출력 비움(필수)
- [ ] 외부 키/개인정보가 노출되지 않음
- [ ] 노트북이 상단부터 하단까지 재실행 가능
- [ ] uv 사용 시 `uv sync --frozen`으로 재현 가능
- [ ] 새 문서/노트북은 폴더 구조/네이밍을 따름
- [ ] 큰 파일(>50MB) 또는 바이너리 데이터는 커밋하지 않음(필요 시 Git LFS 고려)


## 9. 문제 해결(FAQ)
- 가상환경 활성화 스크립트 실행이 차단됨
  - `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`
- 한글 경로/출력 인코딩 문제
  - `chcp 65001`로 UTF-8 코드페이지 전환 후 재실행
- OpenAI/네트워크 오류(429/5xx)
  - 재시도 지수 백오프, 속도 제한 준수, 키/조직/프로젝트 설정 재확인
- LangSmith 트레이싱이 보이지 않음
  - `LANGSMITH_API_KEY`, `LANGSMITH_TRACING=true`, `LANGSMITH_PROJECT` 값을 확인


## 10. 보안 수칙
- API 키는 환경 변수로만 공급하고, 노트북/코드/출력/스크린샷에 포함하지 않습니다.
- 로컬/원격 저장 위치 권한을 적절히 제한합니다.


## 11. 유지보수
- 의존성 업데이트는 uv를 통해 잠금 파일 기준으로 관리합니다.
- 새로운 프레임워크 예제를 추가할 때는 본 가이드를 준수하고, 필요 시 본 문서를 업데이트하세요.
