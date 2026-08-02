# Phase Gates — 분석·집필 단계 (G4–G6)

`yumc-research-analysis`가 강제하는 Human-in-the-loop 게이트입니다.
설계 단계(G0–G3)는 `yumc-research-design` 플러그인의
`skills/research-orchestrator/references/phase_gates.md`에 있습니다.

## G4 — Feasibility Verdict 승인

**위치**: Phase 4 (Data Inspector) 종료 시
**책임 스킬**: stat-analysis (data-inspector가 verdict 산출)
**유형**: 4지선다 + 사용자 검토 4항목
**Phase 4 작동 원칙**: data-inspect/SKILL.md의 "작동 원칙 5가지" 참조 (PHI Local-only 비타협, Auto-detection vs human review separation, Verdict as recommendation 등).

### 검증 항목 1 — verdict 응답 (4지선다)

| 선택 | 의미 |
|---|---|
| 진행 (verdict=testable 또는 partial) | Phase 5 진입 + variable_mapping.json 확정 |
| 대안 질문 채택 (verdict=not testable) | data-inspector가 제안한 후보 질문 중 사용자 선택 → Phase 2 amendment 절차 |
| 데이터 보완 후 재시도 | Phase 0으로 복귀 (예: 결측 변수 추가 추출) |
| 중단 | 프로젝트 보류 |

**not testable이어도 사용자가 명시적으로 진행을 요청하면 informed-consent로 진행 가능** (단, 결과는 약한 근거임을 명시 + evolution_log 기록).

### 검증 항목 2 — 자동 탐지 불가 영역 사용자 검토 (필수 4항목)

| 항목 | 자동 탐지 불가 이유 | 응답 |
|---|---|---|
| 선택 편향 (selection bias) | referral pattern, 등록 기준의 비기록 편향 — 데이터 외 정보 필요 | 있음/없음/모름 |
| 측정 편향 (information bias) | 결과 평가의 비맹검 — 임상 컨텍스트 필요 | 있음/없음/모름 |
| 교란 누락 (unmeasured confounding) | 데이터에 없는 변수 (예: 흡연력, SES) — 도메인 지식 필요 | 있음/없음/모름 |
| Collider 위험 (M-bias) | DAG 기반 인과 구조 — 도메인 지식 필요 | 있음/없음/모름 |

"있음" 또는 "모름" 응답 → Phase 5 limitation 섹션에 자동 기록.

### 통과 시 동작
- "진행" → Phase 5 진입 + variable_mapping.json + 사용자 검토 응답 확정
- "대안 질문 채택" → prereg-lock의 amendment 절차 진입 (Soft 모델, 자유 변경)

---

## G5 — 분석 결과 승인

**위치**: Phase 5 (Statistician) 종료 시 (1차 분석 후)
**책임 스킬**: stat-analysis (statistician가 결과 산출)
**유형**: 4지선다 + 검토 3항목
**Phase 5 작동 원칙**: stat-analysis/SKILL.md의 "작동 원칙 5가지" 참조 (Pre-reg as analysis source, Confirmatory/Exploratory auto-separation, Effect size + 95% CI 비타협, Reproducibility by seed, Diagnostics mandatory).

### IRB 게이트 사전 점검 (Phase 5 진입 시)

| irb_status | 처리 |
|---|---|
| `approved`/`exempt`/`expedited` | 정상 진행 |
| `submitted` (심사 중) | 경고 + 사용자 명시적 override 가능 (override 시 evolution_log 영구 기록) |
| `pending_submission` (미제출) | 경고 + 사용자 명시적 override 가능 (override 시 evolution_log 영구 기록) |

informed-consent 모델: 차단이 아닌 권고 + 사용자 책임.

### 사용자 검토 필수 3항목 (분석 결과 검토)

| 항목 | 점검 내용 |
|---|---|
| 임상적 해석 적절성 | effect size의 임상적 의미 (예: HR 1.30이 임상적으로 의미 있는 차이인가). 자동 생성된 해석 단락 검토 |
| 진단 플롯 | PH 가정 위반 (Schoenfeld p<0.05) 여부, calibration, ROC, residuals. 위반 시 대안 모델(stratified Cox, time-varying coefficient, AFT) 제안 검토 |
| 민감도 분석 일관성 | Complete-case vs MI vs IPTW 결과의 robustness. 한 분석에서만 유의하면 limitation 명시 |

### 다음 단계 4지선다

| 선택 | 동작 |
|---|---|
| 추가 분석 요청 | Phase 5 재실행 또는 exploratory 추가 (BH-FDR 자동 적용) |
| 종료 | v1 종료, 결과만 활용 |
| Phase 6 (v2) 대기 | 논문 초안 작성 — v1에서는 stub |
| Amendment 절차 | 분석 계획 수정 필요 시 prereg amendment (Soft 모델, 자유 변경 + 자동 로깅) |

---

## G6 — Manuscript Draft 검토

**위치**: Phase 6 (Manuscript Writer) 종료 시
**책임 스킬**: stat-analysis (manuscript-writer가 .docx 생성)
**유형**: 8개 검토 항목 + 4지선다
**Phase 6 작동 원칙**: manuscript-writer/SKILL.md 참조 (Wrapper, All sections grounded, Citation Grounding 비타협, STROBE 22항목 자동, ICMJE AI disclosure).

### 검토 필수 8항목
1. Introduction의 임상 배경 적절성
2. Methods의 STROBE 22항목 충족 (누락 시 사용자 입력)
3. Results의 effect size · 95% CI 정확성
4. **Discussion 임상적 함의 — 사용자 직접 작성·재작성 권장 영역**
5. Limitations에 Phase 4 사용자 검토 4항목(선택편향·측정편향·교란·collider) 반영
6. ICMJE AI disclosure 동의
7. 저자 기여(CRediT) 정확성 — AI는 저자 아님
8. 학술지 후보별 형식 요구 반영

### 다음 단계 4지선다

| 선택 | 동작 |
|---|---|
| Finalize | manuscript v1 종료 → **v1.0 하네스 종료** |
| 수정 요청 | manuscript-writer 재호출 |
| Phase 5로 복귀 | 추가 분석 필요 시 |
| 외부 검토로 이관 | Phase 7(peer review)는 본 하네스 범위 밖 — 사용자 직접 |

### 의도된 한계

- **Phase 7·8 비포함**: 학술지 동료심사·재투고는 본 하네스 범위 밖. 사용자가 직접 또는 외부 도구로 처리
- Discussion 임상 함의는 자동 생성하되 **사용자 직접 작성·재작성 권장**
- 학술지별 형식 차이는 일부만 반영 — 사용자가 학술지 가이드라인에 맞춰 재포맷
- AI disclosure 정책은 학술지마다 다름 — ICMJE 일반 권고만 따름

---

## 게이트 공통 정책

### 모든 게이트에 적용

- 사용자 응답은 명시적이어야 함 ("아마", "그럴 것 같다" 등은 재질문)
- 모든 게이트 결정은 evolution_log에 기록
- 게이트 우회 시도(예: 직접 prereg.json 편집) 자동 탐지 시 alert

### 게이트 우회 탐지

- prereg.json 해시 불일치
- 잠긴 파일의 mtime 변경
- amendment_log 미경유 변경

### 우회 발견 시
1. 분석 즉시 중단
2. 사용자에게 alert
3. evolution_log에 기록 (학술적 무결성을 위해 영구 기록)
