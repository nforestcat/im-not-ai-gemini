---
name: humanize-korean
description: AI 한글 티 제거 및 자연스러운 윤문 도구. ChatGPT, Claude, Gemini 등이 생성한 한글 텍스트에서 번역투, 기계적 병렬, 어색한 관용구 등을 탐지하고 사람처럼 자연스러운 문체로 재작성합니다. (Pro급 모델 최적화)
---

# Humanize Korean

AI가 작성한 한글 텍스트에서 'AI 티'를 제거하고, 의미 훼손 없이 자연스러운 한국어로 윤문하는 스킬입니다.

## 주요 기능
- **AI 패턴 탐지**: 12대 카테고리(번역투, 기계적 병렬, 자소서 특화, 이메일 특화 등) 50+ 패턴 탐지
- **수술적 윤문**: 탐지된 스팬(span)에 대해서만 정밀 수정 (의미 불변 원칙)
- **품질 검증**: 내용 동등성 감사 및 자연스러움 등급 판정

## 지원 장르
- **칼럼/에세이**: 논리적 흐름과 리듬감 강조
- **리포트**: 객관성 유지 및 Hype 어휘 제거
- **블로그**: 친근한 어조와 정보 전달력 개선
- **공적 문서**: 비즈니스 격식 및 문어체 정제
- **자기소개서**: 1인칭 주체성 및 성과 중심 동사 강화 (Category K)
- **이메일**: 간결한 비즈니스 커뮤니케이션 및 목적성 강화 (Category L)

## 워크플로우
이 스킬은 기본적으로 **Fast 모드**로 동작하며, 필요 시 `--strict` 옵션을 통해 **5인 에이전트 파이프라인**을 가동합니다. 모든 에이전트는 **Pro급 모델**(`model: pro`)을 사용하여 정밀하게 사고합니다.

### 1. Fast 모드 (디폴트)
짧은 글(5,000자 이하)에 최적화된 모드입니다.
- 지침: `references/quick-rules.md`를 참고하여 즉시 윤문 수행.

### 2. Strict 모드 (정밀 검증)
장문(8,000자 이상)이나 중요한 문서에 사용합니다.
- **Phase A**: `references/agents/ai-tell-detector.md`로 탐지 리포트 생성
- **Phase B**: `references/agents/korean-style-rewriter.md`로 정밀 윤문
- **Phase C**: `references/agents/content-fidelity-auditor.md`와 `naturalness-reviewer.md`로 교차 검증

## 사용 방법
- "이 AI 글 자연스럽게 윤문해줘: [텍스트]"
- "이메일 톤으로 자연스럽게 고쳐줘: [텍스트]"
- "자소서 번역투 제거하고 사람처럼 고쳐줘: [텍스트] --strict"

## 금기 사항 (Taboos)
- 수치, 단위, 날짜, 고유명사 절대 변경 금지
- 큰따옴표 내 인용문 보존
- 원문에 없는 정보 추가 금지

## 참고 자원
- 상세 분류 체계: [references/ai-tell-taxonomy.md](references/ai-tell-taxonomy.md)
- 윤문 레시피: [references/rewriting-playbook.md](references/rewriting-playbook.md)
