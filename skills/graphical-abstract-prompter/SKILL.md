---
name: graphical-abstract-prompter
description: clinical-research-harness Phase 7 (manuscript 이후 단계). 잠긴 manuscript와 핵심 결과를 받아, Midjourney / DALL-E / Adobe Firefly / Stable Diffusion 같은 이미지 생성 AI에 그대로 붙여넣을 수 있는 graphical abstract 생성 프롬프트를 산출한다. Journal별 스타일 가이드 + B&W safe + 의학 일러스트 컨벤션 자동 반영. "graphical abstract", "visual abstract", "그래픽 초록", "abstract figure"를 언급할 때 자동 호출. graphical-abstract-prompter agent가 호출.
---

# Graphical Abstract Prompter — Phase 7

## 목적

Phase 6 (manuscript-writer)에서 산출된 manuscript와 Phase 5 (stat-analysis)의 핵심 결과를 입력으로 받아, 이미지 생성 AI에 붙여넣을 수 있는 **graphical abstract 생성 프롬프트(.md + .txt)** 와, 옵션으로 **SVG 와이어프레임 schema**를 산출한다.

본 skill은 이미지를 직접 생성하지 **않는다** (PI가 선호하는 도구 — Adobe Firefly / Midjourney / DALL-E / BioRender — 를 사용하도록 프롬프트만 전달). Adobe Firefly MCP가 연결된 경우에 한해 이미지 직접 생성 옵션을 함께 제공한다.

## 입력

필수:
- `manuscript_path`: 완성된 manuscript .docx 또는 .md 경로
- `headline_finding`: 한 줄로 압축된 핵심 결과 (예: "LAVI ≥ 52 mL/m² + RespVar(-) + No malignancy → 7.7% tamponade rate in deferred patients")

옵션:
- `target_journal`: JASE / EHJ-CVI / Circulation / NEJM / JAMA / Lancet / A&CC / Korean Circulation J 등 (각 저널 가이드라인 반영)
- `style_preference`: clinical-illustration / schematic-flow / data-driven-infographic / before-after / patient-pathway 중 1개
- `color_palette`: B&W safe (default) / single-accent-color / journal-house-style
- `aspect_ratio`: 1:1 (default) / 16:9 / 4:3 / 2:1 banner
- `image_tool`: midjourney / dalle3 / firefly / stable-diffusion / biorender-manual / generic (default = generic, 모든 도구에 호환되는 프롬프트)

## 산출물

`_graphical_abstract/`:
1. `prompt_<tool>.txt` — 도구별 즉시 사용 가능한 프롬프트 (필수)
2. `prompt_full.md` — 구조화된 전체 프롬프트 + 부연 설명 + 텍스트 라벨 리스트
3. `layout_wireframe.svg` — 패널 배치 + 텍스트 위치 와이어프레임 (옵션, BioRender/Inkscape 수동 작업 시 참조)
4. `prompt_summary.json` — 메타데이터 (사용된 manuscript hash, headline finding, target journal)

## 작동 단계

1. **Manuscript 분석**: docx에서 핵심 4가지 추출 — (a) 환자군 정의, (b) 주요 변수 (예: LAVI, RespVar), (c) 핵심 결과 숫자 (cutoff, NPV, AUC 등), (d) 임상 권고 (defer / drain).
2. **Layout 선택**: 위 4가지를 layout pattern (`references/layout_patterns.md`)에 맵핑.
3. **Journal style 적용**: `references/journal_styles.md`에서 target journal의 graphical abstract 컨벤션 반영.
4. **Image tool 어댑테이션**: `references/prompt_templates.md`에서 해당 도구의 syntax 변환 (예: Midjourney `--ar 1:1 --style raw`, DALL-E natural language, Firefly text-to-image).
5. **Text label 표준화**: 모든 숫자/cutoff은 한 번 더 verify (manuscript의 Table/Results와 자동 대조).
6. **출력**: 위 산출물 4개 작성.

## Hard rules

- 환자나 의사의 실명 / 식별 가능 이미지 사용 금지 — 항상 익명·일반화된 인물 일러스트
- 핵심 숫자 (AUC, NPV, cutoff) 는 manuscript와 정확히 일치 (LLM 자유 추측 금지)
- B&W safe palette default — 컬러 인쇄 비용 절감 가능한 형태
- 의학 일러스트 표준 (heart anatomy, echo wave forms 등) 정확성 유지
- 저작권 충돌 방지를 위해 "in the style of [실제 일러스트레이터 이름]" 류 표현 사용 금지
- ICMJE AI disclosure 텍스트 자동 첨부 (`prompt_full.md` 하단)

## 호출 가이드 — orchestrator integration

clinical-research-harness Phase 7 게이트 진입 조건:
- Phase 6 manuscript-writer가 완료되어 `_manuscript/Manuscript_*.docx` 가 존재
- Phase 3 prereg hash가 manuscript에 인용됨
- PI가 "graphical abstract", "visual abstract", "그래픽 초록", "abstract figure" 중 하나를 언급

산출 후 사용자 액션:
- (a) `prompt_<tool>.txt` 를 본인 선호 이미지 생성 도구에 paste → 1차 image 생성
- (b) 1–3회 iteration으로 텍스트 라벨 / 색상 미세조정
- (c) 최종 PNG/SVG를 `_manuscript/Graphical_Abstract.png` 로 저장 → manuscript submission package에 추가

## 참고 파일

- `references/journal_styles.md` — 9개 저널의 graphical abstract 컨벤션
- `references/layout_patterns.md` — 6가지 표준 layout patterns
- `references/prompt_templates.md` — 5개 이미지 도구별 prompt syntax
- `references/medical_illustration_glossary.md` — 표준 의학 일러스트 모티프

## 예시 산출 (이번 manuscript 기준)

Headline: "LA dim ≥ 48 mm + RespVar(-) + No malignancy → 7.7% tamponade rate"

생성 프롬프트 (Midjourney syntax, generic medical illustration style):
```
A clean medical graphical abstract for an intensive care journal, single-panel
flow design, showing: LEFT side — a critically ill patient silhouette with a
large pericardial effusion highlighted as a crescent around the heart, three
small bedside POCUS icons (linear LA measurement caliper showing "≥48 mm",
mitral inflow Doppler waveform with "no respiratory variation" annotation,
clinical chart with "no active malignancy"); CENTER — a green-arrow flow
labeled "TRIPLE-NEGATIVE = DEFER"; RIGHT side — a clock icon labeled
"24–48 h" and a POCUS probe labeled "repeat scan", with the result
"7.7% tamponade physiology rate (3/39)" as the headline number.
Color palette: black, white, single sage green accent for safe-defer.
Aspect ratio 16:9. Vector medical illustration, no photorealism, no patient
identifying features. Title text at top: "Bedside Rule for Safe
Pericardiocentesis Deferral in Large PE". --ar 16:9 --style raw --v 6
```
