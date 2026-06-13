# [AI SKILL] Architecture Harness & Guardrail Principles

## [EXECUTION RULES]
너는 어떤 파이프라인이나 에이전트 시스템을 설계, 코딩, 실행할 때 반드시 아래의 7대 아키텍처 원칙을 준수해야 한다. 이 원칙들을 위반하는 코드를 작성하거나 아키텍처를 제안하면 시스템 오류가 발생한다.

---

## [CORE 7 PRINCIPLES]

### 1. Context & Memory (맥락과 메모리 분리)
- 대량의 원문 데이터를 시스템 프롬프트에 직접 포함하지 말 것.
- 외부 Vector DB나 파일 인덱스를 조회(RAG)하여 필요한 에센스만 프롬프트 앞부분(Prefix)에 주입하는 구조로 설계하라.

### 2. Tools & Action (도구 권한 제한 및 유효성 검사)
- AI에게 OS 터미널이나 웹 브라우저의 날것 그대로의 실행 권한을 넘기지 말 것.
- 모든 외부 행동은 입력 인자(Arguments) 검증 로직이 포함된 파이썬 함수로 캡슐화하여 제공하라.

### 3. Orchestration & Loop (오케스트레이션과 루프 제어)
- 하나의 모델이 전체 태스크를 올인원으로 처리하게 하지 말 것.
- 지휘자(Orchestrator)가 계획 수립(Plan), 실행(Execute), 검증(Review Loop) 단계를 비동기로 통제하는 멀티 에이전트 구조를 짜라.

### 4. State & Persistence (상태 영속성과 세이브 포인트)
- 장시간 실행되는 파이프라인의 각 단계가 완료될 때마다 디스크에 현재 진행 상태(`state.json`)를 기록하라.
- 에러 발생 시 처음부터 재실행하는 것이 아니라, 마지막 체크포인트 단계부터 이어하기(Resume)가 가능해야 한다.

### 5. Sandbox & Compute (샌드박스를 통한 컴퓨터 환경 격리)
- 에이전트가 코드를 실행하고 테스트하는 모든 컴퓨팅 워크스페이스 공간은 반드시 도커(Docker) 컨테이너 내부로 제한하라.

### 6. Observability & Governance (관측 가능성과 인간 승인 가드레일)
- 모든 API 호출 비용과 추론 경로를 로깅하라.
- 최종 발행, 결제, 외부 전송 등 치명적인 액션 직전에는 반드시 인간의 승인(`Human-in-the-loop: [Y/N]`)을 기다리는 차단(Blocking) 코드를 삽입하라.

### 7. Cost & Workflow Optimization (비용 및 워크플로우 최적화)
- 1차 필터링, 분류 등 단순 작업에는 경량 모델(Haiku, GPT-4o mini) 또는 로컬 LLM을 매칭하고, 고난도 추론 단계에서만 고성능 모델(Sonnet/Opus)을 호출하도록 라우팅 코드를 짜라.

---
## [OUTPUT FORMAT]
이 Skill을 적용하여 코드를 짤 때는, 주석이나 리포트 상단에 `[Harness Skill Applied: Principle #X]` 형태로 어떤 원칙이 어떻게 적용되었는지 명시하라.