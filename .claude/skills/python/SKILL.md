---
name: python
description: Skill de Python. Usar quando a tarefa envolve scripts Python, automacao, manipulacao de dados, ou integracao com APIs.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# Python

## Documentacao Oficial

- Python Docs: https://docs.python.org/3/
- PEP Index: https://peps.python.org/
- Typing: https://docs.python.org/3/library/typing.html

## Padroes

- Python 3.10+ (usar match/case, union com `|`, etc.)
- Type hints em todas as funcoes
- f-strings para formatacao
- pathlib ao inves de os.path
- Docstrings no formato Google

## Exemplos de Referencia

### Funcao tipada com docstring

```python
from pathlib import Path

def read_json_file(file_path: Path) -> dict | None:
    """Le e parseia um arquivo JSON.

    Args:
        file_path: Caminho para o arquivo JSON.

    Returns:
        Dicionario com o conteudo ou None se falhar.
    """
    try:
        return json.loads(file_path.read_text(encoding='utf-8'))
    except (json.JSONDecodeError, FileNotFoundError):
        return None
```

### Dataclass moderna

```python
from dataclasses import dataclass, field
from datetime import datetime

@dataclass
class UserProgress:
    user_id: str
    score: int = 0
    completed_blocks: list[str] = field(default_factory=list)
    last_activity: datetime = field(default_factory=datetime.now)

    @property
    def is_active(self) -> bool:
        return (datetime.now() - self.last_activity).days < 7
```

### Async com aiohttp

```python
import aiohttp

async def fetch_data(url: str, timeout: int = 30) -> dict:
    async with aiohttp.ClientSession() as session:
        async with session.get(url, timeout=aiohttp.ClientTimeout(total=timeout)) as resp:
            resp.raise_for_status()
            return await resp.json()
```

## Quando Buscar Docs Atualizadas

- Features novas do Python 3.12+ / 3.13+
- Sempre consultar: https://docs.python.org/3/whatsnew/
