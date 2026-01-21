# 프로젝트 리소스 및 참조 문서

이 문서는 n8n 자동화 워크플로우 프로젝트에서 사용된 모든 리소스와 참조 문서를 정리한 것입니다.

---

## 📚 공식 문서

### Antigravity 관련
| 문서명 | URL | 용도 |
|--------|-----|------|
| Antigravity 설명 | https://codelabs.developers.google.com/getting-started-google-antigravity | 구글 공식 Antigravity 활용 가이드
| Antigravity Agent SKills | https://codelabs.developers.google.com/getting-started-with-antigravity-skills?hl=en#0 | Agent 제작 레퍼런스 |

### n8n 관련
| 문서명 | URL | 용도 |
|--------|-----|------|
| n8n 공식 문서 | https://docs.n8n.io/ | 전체 플랫폼 가이드 |
| Self-Hosting Guide | https://docs.n8n.io/hosting/ | 로컬 환경 구축 |
| Node Reference | https://docs.n8n.io/integrations/builtin/ | 노드별 상세 설명 |
| Expression Reference | https://docs.n8n.io/code/expressions/ | 표현식 문법 |
| Task Runners | https://docs.n8n.io/hosting/configuration/task-runners/ | Python/JS Runner 설정 |

### Google API
| 문서명 | URL | 용도 |
|--------|-----|------|
| Google Gemini API | https://ai.google.dev/docs | AI 모델 사용법 |
| Google Sheets API | https://developers.google.com/sheets/api | Sheets 연동 |
| Gmail API | https://developers.google.com/gmail/api | 이메일 발송 |
| Google Cloud Console | https://console.cloud.google.com/ | API 키 발급 |

### MCP (Model Context Protocol)
| 문서명 | URL | 용도 |
|--------|-----|------|
| n8n MCP GitHub | https://docs.n8n.io/advanced-ai/accessing-n8n-mcp-server/ | MCP 서버 소스코드 |
| MCP Specification | https://modelcontextprotocol.io/ | MCP 프로토콜 명세 |


---

## 🛠️ 레퍼런스 문서
| 문서명 | URL | 용도 |
|--------|-----|------|
| n8n Skills | https://github.com/czlonkowski/n8n-skills/tree/main/skills | n8n skills 참고 |
| Antigravity-MCP-Setup 레퍼런스 | https://github.com/czlonkowski/n8n-mcp/blob/main/docs/ANTIGRAVITY_SETUP.md | Antigravity에서 n8n 셋업 참고 |
| n8n 셀프호스팅 | https://wikidocs.net/290898 | n8n 셀프호스팅 가이드 문서 |

---

## 🛠️ 개발 도구

---

### 필수 도구
| 도구명 | 버전 | 설치 명령어 | 용도 |
|--------|------|-------------|------|
| Node.js | v18+ | https://nodejs.org/ | 런타임 환경 |
| npm | v9+ | Node.js 포함 | 패키지 관리자 |
| n8n | v1.x | `npm install -g n8n` | 워크플로우 엔진 |
| n8n-mcp | latest | `npm install -g n8n-mcp` | MCP 서버 |

### 선택 도구
| 도구명 | 용도 |
|--------|------|
| Git | 버전 관리 |
| VS Code | 코드 에디터 |
| Postman | API 테스트 |

---

## 📁 프로젝트 내부 문서

### 핵심 문서
| 파일명 | 경로 | 설명 |
|--------|------|------|
| README.md | `/` | 프로젝트 전체 개요 및 작업 이력 |
| AGENTS.md | `/` | AI Agent 운영 규칙 (3대 철칙) |
| SKILL.md | `/.agent/skills/n8n-workflow-builder/` | n8n Workflow Builder Skill 정의 |
| config_guide.md | `/.agent/skills/n8n-workflow-builder/` | 노드별 상세 설정 가이드 |
| patterns.md | `/.agent/skills/n8n-workflow-builder/` | 재사용 가능한 워크플로우 패턴 |

### 문서별 주요 내용

#### README.md
- 프로젝트 개요 및 기술 스택
- 시간순 작업 이력 (Phase 1~6)
- 설치 및 실행 가이드
- 주요 에러 및 해결 방법
- 트러블슈팅 FAQ

#### AGENTS.md
- AI Agent 운영 규칙
- **워크플로우 운영 3대 철칙**:
  1. 무조건적 사후 검증 (Validation First)
  2. 런타임 로그 기반 진단 (Log-Based Diagnosis)
  3. 최신 규격 유지 (Best Practices)
- 도구 활용 지침

#### SKILL.md
- n8n Workflow Builder Skill 정의
- 핵심 기능 영역 5가지
- Antigravity n8n 전문가 운영 규칙 (Locked-In)
- 3단계 프로세스 상세 설명

#### config_guide.md
- 워크플로우 각 노드별 상세 설정
- 파라미터 JSON 예제
- 표현식 작성 규칙
- 데이터 타입 매칭 가이드
- 디버깅 체크리스트

#### patterns.md
- 재사용 가능한 워크플로우 패턴
- 노드 조합 베스트 프랙티스
- 일반적인 자동화 시나리오

---

## 🔧 MCP 도구 목록

### 워크플로우 관리
| 도구명 | 기능 | 사용 예시 |
|--------|------|----------|
| `n8n_list_workflows` | 워크플로우 목록 조회 | 전체 워크플로우 확인 |
| `n8n_get_workflow` | 워크플로우 상세 조회 | 특정 워크플로우 분석 |
| `n8n_create_workflow` | 새 워크플로우 생성 | 초기 워크플로우 구축 |
| `n8n_update_partial_workflow` | 부분 수정 | 특정 노드만 업데이트 |
| `n8n_update_full_workflow` | 전체 수정 | 워크플로우 전체 재구성 |
| `n8n_delete_workflow` | 워크플로우 삭제 | 불필요한 워크플로우 제거 |

### 검증 및 디버깅
| 도구명 | 기능 | 사용 예시 |
|--------|------|----------|
| `n8n_validate_workflow` | 유효성 검사 | 설정 오류 조기 발견 |
| `n8n_autofix_workflow` | 자동 오류 수정 | 노드 버전 업그레이드 |
| `n8n_executions` | 실행 로그 조회 | 에러 원인 분석 |
| `n8n_test_workflow` | 테스트 실행 | 워크플로우 동작 확인 |

### 노드 및 템플릿
| 도구명 | 기능 | 사용 예시 |
|--------|------|----------|
| `search_nodes` | 노드 검색 | 필요한 노드 찾기 |
| `get_node` | 노드 상세 정보 | 노드 파라미터 확인 |
| `validate_node` | 노드 설정 검증 | 개별 노드 검증 |
| `search_templates` | 템플릿 검색 | 참조 워크플로우 찾기 |
| `get_template` | 템플릿 상세 조회 | 템플릿 구조 분석 |
| `n8n_deploy_template` | 템플릿 배포 | 템플릿 기반 생성 |

### 버전 관리
| 도구명 | 기능 | 사용 예시 |
|--------|------|----------|
| `n8n_workflow_versions` | 버전 이력 관리 | 이전 버전 복구 |

---

## 📖 학습 리소스

### 공식 튜토리얼
| 제목 | URL | 난이도 |
|------|-----|--------|
| n8n Quickstart | https://docs.n8n.io/try-it-out/ | 초급 |
| Building Your First Workflow | https://docs.n8n.io/workflows/ | 초급 |
| Advanced Expressions | https://docs.n8n.io/code/expressions/advanced/ | 중급 |
| Error Handling | https://docs.n8n.io/workflows/error-handling/ | 중급 |

### 커뮤니티 리소스
| 리소스 | URL | 설명 |
|--------|-----|------|
| n8n Community Forum | https://community.n8n.io/ | 질문 및 토론 |
| n8n YouTube Channel | https://www.youtube.com/@n8n-io | 비디오 튜토리얼 |
| n8n Templates | https://n8n.io/workflows/ | 공유 워크플로우 |

---

## 🎓 핵심 개념 정리

### 1. n8n 표현식 시스템
```javascript
// 기본 문법
={{ $json.fieldName }}

// 노드 참조
={{ $node.NodeName.json.fieldName }}

// 조건부 표현식
={{ $json.value > 100 ? "High" : "Low" }}

// JavaScript 메서드
={{ $json.text.toUpperCase() }}
```

### 2. 데이터 흐름
```
Input Data → Node Processing → Output Data
```
- 각 노드는 이전 노드의 출력을 입력으로 받음
- `$json`: 현재 아이템의 JSON 데이터
- `$node.NodeName.json`: 특정 노드의 출력 데이터

### 3. 노드 타입
| 타입 | 설명 | 예시 |
|------|------|------|
| Trigger | 워크플로우 시작점 | Google Sheets Trigger |
| Regular | 데이터 처리 | IF, Set, Code |
| Action | 외부 서비스 호출 | Gmail, Slack |

### 4. 에러 핸들링
- **Continue on Fail**: 노드 실패 시 계속 진행
- **Error Trigger**: 에러 발생 시 별도 워크플로우 실행
- **Retry**: 실패 시 자동 재시도

---

## 🔍 디버깅 가이드

### 1. 검증 프로세스
```bash
# 1단계: 정적 검증
n8n_validate_workflow(id="워크플로우ID")

# 2단계: 실행 로그 확인
n8n_executions(action="list", workflowId="워크플로우ID")

# 3단계: 에러 상세 분석
n8n_executions(action="get", id="실행ID", mode="error")

# 4단계: 자동 수정
n8n_autofix_workflow(id="워크플로우ID", applyFixes=true)
```

### 2. 일반적인 에러 패턴
| 에러 메시지 | 원인 | 해결 방법 |
|------------|------|----------|
| `Cannot read properties of undefined` | 데이터 경로 오류 | 실행 로그에서 데이터 구조 확인 |
| `invalid syntax` | 표현식 문법 오류 | `=` 접두사, 따옴표 확인 |
| `Wrong type` | 데이터 타입 불일치 | 연산자 타입 변경 |
| `Resource not found` | 노드 설정 오류 | UI에서 수동 재설정 |

---

## 📊 프로젝트 통계

### 개발 기간
- **시작일**: 2026-01-19
- **완료일**: 2026-01-19
- **총 소요 시간**: 약 12시간

### 워크플로우 통계
- **총 노드 수**: 4개
- **총 연결 수**: 3개
- **실행 기록**: 19회
- **최종 상태**: Active, Valid

### 디버깅 통계
- **주요 에러 유형**: 6가지
- **노드 버전 업그레이드**: 2회
- **검증 실행**: 10회 이상

---

## 🚀 다음 단계

### 추가 학습 권장 사항
1. [ ] n8n Code Node 마스터 (JavaScript/Python)
2. [ ] Webhook 기반 실시간 트리거 구현
3. [ ] 데이터베이스 연동 (PostgreSQL, MongoDB)
4. [ ] 복잡한 조건부 로직 (Switch, Merge)
5. [ ] 에러 핸들링 고급 패턴

### 프로젝트 확장 아이디어
1. [ ] Slack 알림 추가
2. [ ] 성과 대시보드 연동
3. [ ] A/B 테스트 자동화
4. [ ] 배치 처리 (여러 행 동시 처리)
5. [ ] HTML 이메일 템플릿

---

## 📞 지원 및 문의

### 공식 지원
- **n8n Forum**: https://community.n8n.io/
- **GitHub Issues**: https://github.com/n8n-io/n8n/issues
- **Discord**: https://discord.gg/n8n

### 프로젝트 관련
- **GitHub Repository**: (프로젝트 저장소 URL)
- **Issues**: GitHub Issues 탭 활용

---

**문서 버전**: 1.0  
**마지막 업데이트**: 2026-01-19  
**작성자**: JINSOO HWANG
