# n8n Workflow Patterns Guide

이 가이드는 `n8n-workflow-builder` Skill의 일부로, 신뢰할 수 있는 워크플로우 구조를 설계하는 데 도움을 줍니다.

## 1. AI 에이전트 패턴
AI Agent 노드를 중심으로 한 동적 의사결정 워크플로우입니다.
- **Triggers**: Webhook, Chat, or Schedule
- **Core**: `AI Agent` Node
- **Tools**: `n8n Tool`, `HTTP Request`, `Custom Code`
- **Output**: 완성된 응답을 사용자에게 반환하거나 다른 시스템으로 전송

## 2. 데이터 통합 (ETL) 패턴
서로 다른 시스템 간의 데이터를 동기화하거나 가공하는 패턴입니다.
- **Extract**: API 호출 또는 DB 쿼리를 통한 데이터 획득
- **Transform**: `Edit Fields` (Set), `Filter`, `Code` 노드를 사용한 정제
- **Load**: 대상 시스템(DB, CRM, Google Sheets 등)으로 전송

## 3. 에러 처리 및 알림 패턴
워크플로우 실패 시 자동으로 감지하고 알림을 보내는 견고한 구조입니다.
- **Error Trigger**: 워크플로우 내에서 에러 발생 시 자동 트리거
- **Notification**: Slack, Email, Discord 등을 통한 에러 리포트
- **Reporting**: 에러 내용(노드 이름, 메시지, 시간)을 통합 로그에 기록
