# Graphify Installation

## installation in ubuntu 

```
curl -LsSf https://astral.sh/uv/install.sh | sh

source ~/.bashrc

uv --version
```

uv installation
```
uv python install 3.12
echo "3.12" > .python-version
uv init
```

The following files are created.
- pyproject.toml
- README.md
- main.py
- .python-version


create a virtual environment and then activate the environment
```
 uv venv
 source .venv/bin/activate
```

you can execute without activating the virtual environment
```
 uv run python main.py
```

install graphify
```
uv tool install graphifyy
uv tool update-shell
source ~/.bashrc
graphify --version
```

install graphify skill for codex. Claude and antigravity don't need to do this.
```
graphify install --project --platform codex
```


Run the following in the project after building a graph in the command 
This writes a small config file that tells your assistant to consult the knowledge graph for codebase questions, preferring scoped queries like `graphify query "<question>"` over reading the full report or grepping raw files.
```
graphify codex install
graphify antigravity install
graphify claude install
```

update the storage by graphify
```
graphify update 20_Wiki
```

question
```
$graphify query "question.........."
```

## Installation in MacOs

install uv
```
brew install uv
uv --version
```

install graphifyy
```
uv tool install graphifyy
```
set up graphify for codex
```
graphify install --project --platform codex
```
set up graphify for codex to have a hook in .codex folder
```
graphify codex install
graphify antigravity install
graphify claude install
```

코덱스 병렬 추출하려면 다음의 설정
```
nano ~/.codex/config.toml
```
아래의 설정을 추가하거나 수정
```
[features]
multi_agent = true
```