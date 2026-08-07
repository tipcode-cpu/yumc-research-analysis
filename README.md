# YUMC Research Analysis (Phase 4–6)

임상연구의 **분석·집필 단계**를 담당하는 하네스입니다.
[`yumc-research-design`](https://github.com/tipcode-cpu/yumc-research-design)이 만든 `prereg.json`을 입력으로 받습니다.

`clinical-research-harness` (MIT, © 2026 Jeon Kihyun)의 포크입니다. 변경 내역은 아래 참조.

## 플러그인 설치

**Claude Code (터미널)**

```
/plugin marketplace add tipcode-cpu/yumc-research-analysis
/plugin install yumc-research-analysis@yumc-research-analysis
```

**Claude 데스크톱 앱** — 설정 → 플러그인 → `추가` → `tipcode-cpu/yumc-research-analysis`

업데이트는 `/plugin marketplace update yumc-research-analysis` 후 다시 설치하면 됩니다.

| Phase | 스킬 | 산출물 |
|---|---|---|
| 4. 데이터 검정가능성 | `data-inspect` | `feasibility_report.md` — verdict + 변수 매핑 (G4) |
| 5. 통계 분석 | `stat-analysis` | `results.json`, `results.md`, STROBE 점검표 (G5) |
| 6. 원고 | `manuscript-writer` | IMRaD 초안 .docx (G6) |

## 입력 계약

`prereg.json` 하나입니다. 설계 플러그인 없이도 그 파일만 있으면 독립 동작합니다.

## 비타협 정책

**PHI 행 비전송** — 개별 환자 행(row)은 어떤 경우에도 LLM 컨텍스트로 가지 않습니다.
LLM이 보는 것은 컬럼명·요약 통계·결측 패턴뿐입니다.
근거: 개인정보보호법, HIPAA, ICH-GCP. **모델 성능과 무관하게 유지됩니다.**

- 자동 마스킹(직접 식별자): `name`, `dob`, `ssn`, `rrn`, `jumin`, `national_id` 등
- 인지 확인(informed-consent): 차트번호·주소·전화·이메일 — 후향 코호트에서 추적용으로
  의도적으로 쓰이는 경우가 많아 자동 마스킹 대신 경고만. 포함 여부는 사용자 결정
- **사용자 책임**: 산출물을 외부 공유(논문 supplementary, 학회 발표)할 때 차트번호·주소가
  섞이지 않았는지 직접 점검

**Effect size + 95% CI** — p-value 단독 보고를 거절합니다 (ASA 2016).

Human gate G4–G6도 유지됩니다.

## 원본에서 바꾼 것

| | |
|---|---|
| **모델 핀 제거** | 에이전트 전원의 `model: sonnet` 삭제 → 세션 모델 상속 |
| **사전등록 재검증 루프 제거** | `run_analysis.py`가 설계 쪽 `lock.py`를 subprocess로 띄워 해시를 재대조하던 동작 삭제. 해시는 `results.json`에 그대로 기록됨 |
| **설계 플러그인 의존 제거** | 위 변경으로 `../prereg-lock/` 경로 참조가 모두 사라져 독립 실행 가능 |
| **그래픽 초록 스킬 제거** | 어느 Phase에도 연결되지 않은 고아였고, description이 매 세션 상주 비용만 발생시켰음 (v1.2.0) |
| **Phase 0–3 분리** | 문헌·가설·IRB는 `yumc-research-design`으로 이관 |

## 설치

Phase 5는 파이썬 패키지가 필요합니다.

```bash
pip install pandas numpy scipy lifelines statsmodels
```

`lifelines`가 없으면 Cox 분석만 건너뛰고 Table 1과 STROBE 점검표는 정상 생성됩니다.

## 시작

```
"이 데이터로 사전등록한 가설 검정 가능한지 봐줘"   → Phase 4
"분석 돌려줘", "Cox", "생존분석", "Table 1"        → Phase 5
"논문 초안", "IMRaD"                              → Phase 6
```

## 라이선스

MIT. 원저작권 © 2026 Jeon Kihyun — `LICENSE` 참조.
