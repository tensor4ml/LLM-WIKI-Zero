# Graphify 설치 가이드 (Graphify Installation)

## Ubuntu에서 설치하기

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh

source ~/.bashrc

uv --version
```

uv 초기화
```bash
uv python install 3.12
echo "3.12" > .python-version
uv init
```

위 명령을 실행하면 다음 파일들이 생성됩니다.
- pyproject.toml
- README.md
- main.py
- .python-version


가상 환경을 생성하고 활성화합니다.
```bash
uv venv
source .venv/bin/activate
```

가상 환경을 활성화하지 않고도 실행할 수 있습니다.
```bash
uv run python main.py
```

graphify를 설치합니다.
```bash
uv tool install graphifyy
uv tool update-shell
source ~/.bashrc
graphify --version
```

Codex용 graphify 스킬을 설치합니다. Claude와 Antigravity는 이 단계가 필요 없습니다.
```bash
graphify install --project --platform codex
```


프로젝트에서 그래프를 빌드한 후 다음을 실행합니다.
이 명령은 어시스턴트가 코드베이스 관련 질문에 대해 전체 보고서를 읽거나 raw 파일을 grep하는 대신 `graphify query "<질문>"` 과 같은 범위 지정 쿼리를 우선적으로 사용하도록 안내하는 소규모 설정 파일을 작성합니다.
```bash
graphify codex install
graphify antigravity install
graphify claude install
```

graphify로 저장소를 업데이트합니다.
```bash
graphify update 20_Wiki
```

질문하기
```bash
graphify query "질문 내용........."
```

## macOS에서 설치하기

uv 설치
```bash
brew install uv
uv --version
```

graphifyy 설치
```bash
uv tool install graphifyy
```

Codex용 graphify 설정
```bash
graphify install --project --platform codex
```

.codex 폴더에 hook이 연결되도록 Codex용 graphify 설정
```bash
graphify codex install
graphify antigravity install
graphify claude install
```