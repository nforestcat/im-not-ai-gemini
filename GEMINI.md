# Humanize KR — AI 한글 티 제거 하네스 (Gemini CLI Optimized)

## 프로젝트 개요
AI(ChatGPT·Claude·Gemini 등)가 쓴 한글 텍스트를 "사람이 쓴 글처럼" 윤문해주는 5인 파이프라인 하네스입니다. Gemini CLI 환경에서 최적화되어 동작하도록 구성되었습니다.

## 4대 철칙
1. **의미 불변 (Fidelity First)** — 사실·주장·수치·고유명사·인용은 100% 원문 보존.
2. **근거 기반 (Span-Grounded)** — 모든 변경은 탐지 finding에 연결. 탐지 없는 구간은 건드리지 않음.
3. **장르 유지 (Tone Match)** — 칼럼을 문학으로, 리포트를 에세이로 옮기지 않음.
4. **과윤문 금지 (No Over-Polish)** — 변경률 30% 초과 시 경고, 50% 초과 시 강제 중단.

## 디렉토리 구조
```
im-not-ai-gemini/
├── GEMINI.md                      # 본 파일 — Gemini CLI 프로젝트 가이드
├── .claude/                       # 에이전트 및 스킬 정의 (Claude 호환 유지)
│   ├── agents/                    # 6인 에이전트 정의 (Markdown 페르소나)
│   └── skills/humanize-korean/
│       ├── SKILL.md               # 오케스트레이터 지침
│       └── references/            # SSOT 데이터 (Taxonomy, Playbook)
└── _workspace/                    # 런타임 산출물 (run_id별 저장)
```

## 에이전트 협업 파이프라인
작업 요청 시 다음 순서에 따라 `.claude/agents/`의 지침을 참고하여 수행합니다.
1. **ai-tell-detector**: AI 패턴 탐지 (span·category·severity)
2. **korean-style-rewriter**: 수술적 윤문 실행
3. **content-fidelity-auditor** & **naturalness-reviewer**: 의미 보존 및 자연스러움 검증
4. **최종 승인**: `final.md` 및 `summary.md` 생성

## 파일 시스템 접근 및 도구 사용 규칙 (Gemini CLI 전용)
Gemini CLI의 성능 극대화와 안전을 위해 다음 도구들을 우선 사용합니다.

| 작업 | 권장 도구 (Gemini Tool) | 주의 사항 |
|---|---|---|
| 파일 존재 확인 | `glob` | 와일드카드 패턴 사용 권장 |
| 디렉토리 목록 열거 | `list_directory` | 경로를 명확히 지정 |
| 파일 읽기 | `read_file` | 대용량 파일은 `start_line`, `end_line` 활용 |
| 파일 쓰기 | `write_file` | 새 파일 생성 시 사용 |
| 파일 편집 | `replace` | 기존 코드 수정 시 컨텍스트 유지 |
| 명령어 실행 | `run_shell_command` | 파이썬 스크립트 실행 시 활용 |

## 사용 방법 (User Prompt 예시)
새 세션에서 다음과 같이 요청하여 하네스를 가동합니다:
> "`.claude/skills/humanize-korean/SKILL.md`의 절차에 따라 다음 텍스트를 윤문해줘: [텍스트 입력]"

- **지원 장르**: 칼럼, 리포트, 블로그, 공적, **자기소개서** (장르를 명시하면 더 정확하게 윤문합니다.)

## 주요 금기 사항 (Major Taboos)
- **의미 훼손 절대 금지**: 수치·단위·날짜·시간 정보를 변경하지 말 것.
- **고유명사 보존**: 인명·지명·제품명·모델명·브랜드명을 임의로 치환하지 말 것.
- **인용구 보호**: 큰따옴표(`""`) 내부의 직접 인용문은 절대 수정하지 말 것.
- **전문 용어 유지**: 법률 조문, 학술 개념어, 기술적 정의를 일상어로 무리하게 바꾸지 말 것.
- **정보 가감 금지**: 원문에 없는 새로운 주장·사실·예시를 추가하거나, 원문의 핵심 정보를 누락하지 말 것.
- **과윤문 경계**: 변경률 30% 초과 시 경고, 50% 초과 시 작업을 즉시 중단하고 보고할 것.
- **자기소개서 특화 규칙**: AI 특유의 소심한 완곡어법("~인 것 같습니다", "~라고 생각합니다")은 자소서 문맥에서 **S1(결정적)** 패턴으로 간주하여 엄격히 제거한다. 화자의 주체성을 살리는 능동 동사를 우선적으로 사용하며, "귀사"와 같은 범용 지칭어는 실제 사명으로 치환한다.

## 참고 문서
- 분류 체계: `.claude/skills/humanize-korean/references/ai-tell-taxonomy.md`
- 윤문 처방: `.claude/skills/humanize-korean/references/rewriting-playbook.md`
