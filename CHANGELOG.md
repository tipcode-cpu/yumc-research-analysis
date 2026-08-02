# CHANGELOG

## 1.2.0

### 제거
- `skills/graphical-abstract-prompter/` — 어느 Phase에도 연결되지 않은 고아 스킬.
  description이 매 세션 시스템 프롬프트에 상주해 **호출된 적 없는 채로 152토큰**을
  계속 소비했다. 상주 비용 1,490 → 1,338토큰 (−10%).
  내용은 v1.1.0 커밋 히스토리와 원본 `clinical-research-harness`에 그대로 남아 있어
  필요하면 되살릴 수 있다.

## 1.1.0

`clinical-research-harness` 1.0.0에서 포크·분할한 뒤의 정리.

### 상속된 것 (원본에서 그대로 이어짐)
- **Citation Grounding** — 인용은 도구가 반환한 PMID/DOI 동반 필수. 자유 생성 인용 거절
- **PHI 행 비전송** — 개별 환자 행은 LLM 컨텍스트로 가지 않음
- **Effect size + 95% CI** — p-value 단독 보고 거절
- **Human gate G0–G6**
- **사전등록 해시 기록** (`lock` 시점)과 수동 `verify` 명령
- **STROBE 22항목 점검표** — `skills/stat-analysis/references/`로 이동

### 제거 (모델 능력에 걸린 제약)
- 에이전트 6개의 `model: sonnet` 핀 → 세션 모델 상속
- 사전등록 해시 **매 Phase 재검증 루프** (13개 지점). Soft 기록 모델이라 불일치에도
  차단하지 않아 강제력이 없었고, 분석 쪽이 설계 쪽 스크립트를 subprocess로 띄우는
  구조라 두 플러그인을 묶는 유일한 의존이었음. 기록 해시와 수동 verify는 유지

### 제거 (과설계)
- 중복 에이전트 5개를 위임형으로 축소 (514줄 → 112줄). 정책·절차는 각 스킬이 단일 출처
  - `hypothesis-refiner`는 **축소하지 않음** — 4축 평가·PICO 템플릿·9카테고리 정제 전략이
    어느 스킬에도 없어 이 파일이 유일한 보관처
- 죽은 코드: `PHI_SUSPECT` 별칭, `import stat`, `import os`, `import numpy`,
  `summarize_continuous`의 미사용 `import pandas`, `verify_prereg`의 `import subprocess`

### 수정
- 정규식 삭제가 남긴 흐름도 고아 화살표·번호 건너뜀 복구 (3개 SKILL.md)

### 이 플러그인 고유
- 중첩 저장소 `graphical-abstract-prompter/.git` (35파일) 제거
- 설계 플러그인 경로 참조 전부 제거 → 독립 실행 가능
