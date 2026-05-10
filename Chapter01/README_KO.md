# Claude Code를 위한 MCP 컨텍스트 최적화

이 튜토리얼은 가용한 모든 MCP 서버를 로드하는 대신, 프로젝트 전용 MCP 설정을 사용하여 Claude Code의 컨텍스트 토큰 소모를 최소화하는 방법을 실습합니다.

## 문제점

여러 MCP 서버가 포함된 범용 [.mcp.json](https://docs.claude.com/en/docs/claude-code/mcp) 파일을 사용할 때, Claude Code는 모든 서버의 도구와 그 설명을 컨텍스트 윈도우에 로드합니다. 이는 가용한 기능 중 일부분만 필요할 때도 상당한 양의 토큰을 불필요하게 소모하게 만듭니다.

## 해결책

`--mcp-config` 플래그를 사용하여 현재 작업에 최적화된 최소한의 프로젝트 전용 MCP 설정을 로드하십시오.

## 튜토리얼 단계

### 1단계: [Verbose MCP 서버](https://github.com/emarco177/claude-code-crash-course/commit/edef7ffdcaeeeca4d109048053e7444e47cf4a78)

먼저, 토큰 낭비를 시연하기 위해 의도적으로 도구 설명이 과도하게 장황한 MCP 서버를 생성했습니다.

- 과도하게 상세한 문서를 가진 [verbose_mcp_server.py](verbose_mcp_server.py) 생성
- 각 도구는 수백 개의 불필요한 토큰 설명을 포함함
- 장황한 도구 설명이 어떻게 불필요한 컨텍스트를 소모하는지 시연

### 2단계: [모든 MCP 서버 로드](https://github.com/emarco177/claude-code-crash-course/commit/d6e830a881d448aa75502edf05c5b5b8be23fa1d)

여러 MCP 서버를 로드하는 일반적인 [.mcp.json](.mcp.json) 설정을 추가했습니다:
- verbose-server (로컬)
- context7
- tavily
- playwright

**결과**: Claude Code 실행 후 `/context` 명령어를 입력하면, 사용하지 않는 도구 설명들로 인해 수천 개의 토큰이 소비되는 것을 확인할 수 있습니다.

### 3단계: [최소 MCP 설정](https://github.com/emarco177/claude-code-crash-course/commit/c0b0538570b431a24166e5f33ffab901284097c5)

리서치 작업에 필요한 Tavily MCP 서버만 포함된 [.mcp.json.tavily](.mcp.json.tavily)를 생성했습니다.

**사용법**:
```bash
claude --mcp-config .mcp.json.tavily
```

**결과**: 필요한 기능은 유지하면서 컨텍스트 소모량을 획기적으로 줄였습니다.

## 커밋 참조

| 단계 | 커밋 | 변경된 파일 |
|------|--------|---------------|
| 1. Verbose MCP Server | [edef7ff](https://github.com/emarco177/claude-code-crash-course/commit/edef7ffdcaeeeca4d109048053e7444e47cf4a78) | `verbose_mcp_server.py`, `main.py`, `pyproject.toml` |
| 2. General MCP Config | [d6e830a](https://github.com/emarco177/claude-code-crash-course/commit/d6e830a881d448aa75502edf05c5b5b8be23fa1d) | `.mcp.json` |
| 3. Minimal MCP Config | [c0b0538](https://github.com/emarco177/claude-code-crash-course/commit/c0b0538570b431a24166e5f33ffab901284097c5) | `.mcp.json.tavily` |

## 핵심 요점

1. **기본 동작은 낭비적일 수 있습니다**: 모든 MCP 서버를 로드하면 사용하지 않을 도구들에 대해 토큰이 소비됩니다.
2. **프로젝트 전용 설정**: 서로 다른 워크플로우를 위해 최소한의 `.mcp.json.*` 파일들을 만드십시오.
3. **`--mcp-config` 플래그 사용**: 필요한 서버들만 사용하여 Claude Code를 부트스트랩하십시오.
4. **`/context`로 모니터링**: 토큰 사용량을 정기적으로 확인하여 설정을 최적화하십시오.

## 베스트 프랙티스

- 작업별 전용 MCP 설정을 생성하십시오 (예: `.mcp.json.research`, `.mcp.json.testing`).
- MCP 서버를 구축할 때 도구 설명을 간결하게 유지하십시오.
- `--mcp-config`를 사용하여 각 세션에 필요한 최소한의 서버 세트만 선택하십시오.
- 불필요한 도구가 포함되어 있는지 MCP 설정을 정기적으로 점검하십시오.
