# 워크플로우 설정 가이드

이 문서는 `Customer Optimization Final Build (Local) 2` 워크플로우의 각 노드별 상세 설정을 정리한 가이드입니다.

---

## 📋 워크플로우 개요

**워크플로우 ID**: `YOUR_ID`  
**워크플로우 이름**: `Customer Optimization Final Build (Local) 2`  
**상태**: Active  
**마지막 업데이트**: 2026-01-19T07:28:18.390Z

### 노드 구성
```
Trigger (Google Sheets) 
  → Filter (IF) 
  → AI (Google Gemini) 
  → Email (Gmail)
```

---

## 1️⃣ Trigger 노드 (Google Sheets Trigger)

### 기본 정보
- **노드 타입**: `n8n-nodes-base.googleSheetsTrigger`
- **버전**: 1
- **노드 ID**: `node1-trigger`
- **노드 이름**: `Trigger`

### 파라미터 설정

#### Document ID
```json
{
  "__rl": true,
  "value": "YOUR_SPREADSHEET_ID",
  "mode": "list",
  "cachedResultName": "n8n test",
  "cachedResultUrl": "https://docs.google.com/spreadsheets/d/YOUR_SPREADSHEET_ID/edit?usp=drivesdk"
}
```

#### Sheet Name
```json
{
  "__rl": true,
  "value": "gid=0",
  "mode": "list",
  "cachedResultName": "Sheet1",
  "cachedResultUrl": "https://docs.google.com/spreadsheets/d/YOUR_SPREADSHEET_ID/edit#gid=0"
}
```

#### Event & Options
```json
{
  "event": "rowUpdate",
  "options": {
    "columnsToWatch": ["Action"]
  },
  "pollTimes": {
    "item": [
      {
        "mode": "everyMinute"
      }
    ]
  }
}
```

### Credentials
- **타입**: `googleSheetsTriggerOAuth2Api`
- **ID**: `YOUR_ID`
- **이름**: `Google Sheets Trigger account`

### 주의사항
⚠️ **중요**: n8n UI에서 Document와 Sheet를 수동으로 재선택해야 트리거가 정상 작동합니다.

---

## 2️⃣ Filter 노드 (IF)

### 기본 정보
- **노드 타입**: `n8n-nodes-base.if`
- **버전**: 2.3
- **노드 ID**: `node2-if`
- **노드 이름**: `Filter`

### 파라미터 설정

#### Conditions
```json
{
  "conditions": {
    "combinator": "and",
    "conditions": [
      {
        "id": "cond-1-abc-1234567890",
        "leftValue": "={{ $json.Action }}",
        "operator": {
          "operation": "true",
          "type": "boolean"
        }
      }
    ]
  },
  "options": {}
}
```

### 핵심 포인트
✅ **Boolean 타입 사용**: Google Sheets 체크박스는 Boolean 값을 반환하므로 `type: "boolean"` 필수  
✅ **Dot Notation**: `$json.Action` 형태로 깔끔하게 참조  
❌ **피해야 할 설정**: `type: "string"`, `rightValue: "true"` (타입 불일치 에러 발생)

---

## 3️⃣ AI 노드 (Google Gemini)

### 기본 정보
- **노드 타입**: `@n8n/n8n-nodes-langchain.googleGemini`
- **버전**: 1.1
- **노드 ID**: `node3-gemini`
- **노드 이름**: `AI`

### 파라미터 설정

#### Resource & Model
```json
{
  "resource": "text",
  "modelId": "models/gemini-3-flash-preview",
  "options": {
    "simplify": true
  }
}
```

#### Messages (프롬프트)
```javascript
=고객명: {{ $json.광고주 }}
현재 상태: {{ $json["Health Check"] }}
지출: {{ $json.Spending }}
예산: {{ $json.Budget }}

위 데이터를 바탕으로 고객에게 보낼 광고 최적화 제안 이메일을 작성하세요.

규칙:
1. **순수 본문만 작성**: 제목, 서명 등 제외
2. **마크다운 금지**: 코드 블록, 굵게 표시 등 금지
3. **완성된 본문**: 실제 데이터를 사용하여 바로 보낼 수 있는 상태로 작성
4. **어조**: 프로페셔널하고 친절한 한국어

본문 내용:
```

### Credentials
- **타입**: `googlePalmApi`
- **ID**: `YOUR_ID`
- **이름**: `Google Gemini(PaLM) Api account`

### 핵심 포인트
✅ **표현식 모드**: 프롬프트 앞에 `=` 필수  
✅ **Simplify 옵션**: 출력 구조를 `$json.text`로 단순화  
✅ **명확한 규칙**: AI에게 출력 형식을 구체적으로 지시  
⚠️ **모델 안정성**: `gemini-3-flash-preview`는 프리뷰 모델로 간헐적 오류 가능 → `gemini-1.5-flash` 권장

### 출력 데이터 구조
- **Simplify = true**: `$json.text`
- **Simplify = false**: `$json.content.parts[0].text`

---

## 4️⃣ Email 노드 (Gmail)

### 기본 정보
- **노드 타입**: `n8n-nodes-base.gmail`
- **버전**: 2.2
- **노드 ID**: `node4-gmail`
- **노드 이름**: `Email`

### 파라미터 설정

#### Resource & Operation
```json
{
  "resource": "message",
  "operation": "send",
  "emailType": "text"
}
```

#### Email Fields
```json
{
  "sendTo": "={{ $node.Filter.json.이메일 }}",
  "subject": "=[광고 성과 알림] {{ $node.Filter.json.광고주 }}님, 광고 최적화 제안입니다.",
  "message": "={{ $json.text || $json.content.parts[0].text }}"
}
```

### Credentials
- **타입**: `gmailOAuth2`
- **ID**: `YOUR_ID`
- **이름**: `Gmail account`

### 핵심 포인트
✅ **노드 참조**: `$node.Filter.json.이메일` (이전 노드 데이터 참조)  
✅ **표현식 모드**: `subject`에도 `=` 접두사 필수  
✅ **유연한 데이터 경로**: `$json.text || $json.content.parts[0].text` (AI 출력 구조 변경 대응)  
❌ **피해야 할 설정**: 역슬래시 포함 표현식 (`$node[\"Filter\"]`)

### 필수 파라미터 체크리스트
- [ ] `resource`: "message"
- [ ] `operation`: "send"
- [ ] `emailType`: "text"
- [ ] `sendTo`: 표현식으로 설정
- [ ] `subject`: 표현식으로 설정
- [ ] `message`: 표현식으로 설정

---

## 🔗 노드 간 연결 (Connections)

```json
{
  "Trigger": {
    "main": [[{
      "node": "Filter",
      "type": "main",
      "index": 0
    }]]
  },
  "Filter": {
    "main": [[{
      "node": "AI",
      "type": "main",
      "index": 0
    }]]
  },
  "AI": {
    "main": [[{
      "node": "Email",
      "type": "main",
      "index": 0
    }]]
  }
}
```

---

## 🛠️ 공통 설정 팁

### 1. 표현식 작성 규칙
```javascript
// ✅ 권장
={{ $json.fieldName }}
={{ $node.NodeName.json.fieldName }}

// ❌ 비권장
{{ $json.fieldName }}           // = 접두사 누락
={{ $json["fieldName"] }}       // 따옴표 사용 (Dot Notation 권장)
={{ $node["NodeName"].json }}   // 역슬래시 발생 가능
```

### 2. 데이터 타입 매칭
| Google Sheets | n8n 타입 | 연산자 타입 |
|---------------|----------|-------------|
| 체크박스 | Boolean | `type: "boolean"` |
| 텍스트 | String | `type: "string"` |
| 숫자 | Number | `type: "number"` |

### 3. 디버깅 체크리스트
1. ✅ 워크플로우 검증: `n8n_validate_workflow`
2. ✅ 실행 로그 확인: `n8n_executions` (mode="error")
3. ✅ 노드 버전 확인: 최신 버전 사용 권장
4. ✅ 자동 수정: `n8n_autofix_workflow`

---

## 📝 변경 이력

### v1.0 (2026-01-19)
- 초기 워크플로우 생성
- Trigger, Filter, AI, Email 노드 구성

### v1.1 (2026-01-19)
- Filter 노드 타입 수정 (String → Boolean)
- AI 프롬프트 최적화 (마크다운 금지 규칙 추가)
- Email 노드 데이터 경로 유연화

### v1.2 (2026-01-19)
- 노드 버전 업그레이드 (Filter v2.3, Email v2.2)
- 표현식 Dot Notation 적용
- 최종 검증 완료 (`valid: true`)

---

**작성일**: 2026-01-19  
**작성자**: JINSOO HWANG  
**워크플로우 버전**: efc3a6cf-3211-46ac-9c5e-f78268f70388
