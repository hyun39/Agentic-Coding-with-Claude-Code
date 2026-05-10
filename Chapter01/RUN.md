# Chapter 01: Execution Guide (실행 가이드)

이 문서는 Chapter 01의 MCP 컨텍스트 최적화 실습을 위한 단계별 명령어를 다룹니다.

## 1. 환경 구축 (Setup)

의존성 관리 도구인 `uv`를 사용하여 환경을 구축합니다. (uv가 없는 경우 `pip install fastmcp`로 대체 가능)

```bash
# 의존성 설치
uv sync

# 또는 pip 사용 시
pip install fastmcp
```

## 2. MCP 서버 실행 테스트 (Optional)

장황한 설명을 포함한 로컬 MCP 서버가 정상적으로 작동하는지 개별적으로 확인해볼 수 있습니다.

```bash
uv run verbose_mcp_server.py
```

## 3. Claude Code 실습 시나리오

### 시나리오 A: 모든 MCP 서버 로드 (컨텍스트 오염 확인)
기본 설정을 사용하여 시중에 로드된 모든 도구들이 얼마나 많은 토큰을 소모하는지 확인합니다.

```bash
# 기본 모드로 실행
claude
# (Claude 대화창 진입 후)
# 컨텍스트 소모량 확인 명령어 입력
/context
```

### 시나리오 B: 최소화된 설정으로 실행 (컨텍스트 최적화 적용)
작업에 꼭 필요한 Tavily 서버만 포함된 설정을 로드하여 토큰 소모량을 비교합니다.

```bash
# 특정 MCP 설정 파일을 지정하여 실행
claude --mcp-config .mcp.json.tavily

# (Claude 대화창 진입 후)
# 시나리오 A와 비교하여 대폭 줄어든 토큰 사용량 확인
/context
```

## 4. 유용한 명령어 요약

| 목적 | 명령어 |
| :--- | :--- |
| **의존성 설치** | `uv sync` |
| **특정 MCP 설정 실행** | `claude --mcp-config <설명파일명>` |
| **텍스트 내 토큰 사용량 확인** | `/context` |
| **현재 로드된 도구 목록 보기** | `/tools` (대화창 내부) |
