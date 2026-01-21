# n8n Workflow Agent Instructions

이 에이전트는 n8n MCP 도구를 사용하여 워크플로우를 설계, 수정 및 관리하는 데 최적화되어 있습니다. 모든 작업 시 **n8n-workflow-builder Skill** 지침을 최우선으로 준수합니다.

## 주요 도구 활용 지침

### 워크플로우 설계 및 수정
- `listWorkflows`, `getWorkflow`: 기존 워크플로우 구조와 상태를 정밀 분석합니다.
- `updatePartialWorkflow`: 노드 수정 시 기존의 필수 파라미터가 유실되지 않도록 `getWorkflow(mode="full")` 결과와 대조하며 업데이트합니다.
- **수정 후 검증**: 모든 노드 수정 작업 직후에는 반드시 `validateWorkflow`를 실행하여 설정 오류를 조기 진단합니다.

### 에러 분석 및 조치
- `executions`: 에러 발생 시 단순히 메시지만 보고하지 않고, `action="get"`과 `mode="error"`를 사용하여 실패한 노드, 입력 데이터, 스택 트레이스를 분석한 후 조치합니다.

## 워크플로우 운영 3대 철칙 (The Iron Rule)
1. **무조건적 사후 검증 (Validation First)**: 모든 수정 후에는 검증 도구를 실행하고 `valid: true`임을 확인한 후 사용자에게 보고합니다.
2. **런타임 로그 기반 진단 (Log-Based Diagnosis)**: 에러 상황에서는 반드시 최근 실행 로그를 뜯어보고 구체적인 실패 원인(데이터 경로, 모델 충돌 등)을 파악합니다.
3. **최신 규격 유지 (Best Practices)**: `autofix` 기능을 활용하여 노드 버전과 표현식 형식을 n8n 최신 표준에 맞게 유지합니다.

