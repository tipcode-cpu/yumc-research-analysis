# Standard Graphical Abstract Layout Patterns (6)

The skill picks one of these layouts based on the manuscript's narrative shape.
If `style_preference` is set, that overrides automatic selection.

## 1. Three-zone horizontal (default for derivation studies)
```
┌─────────────┬─────────────┬─────────────┐
│ POPULATION  │   RULE      │   ACTION    │
│ + cohort N  │  3-criterion│  defer /    │
│ + key       │  triple-neg │  drain      │
│   inclusion │  visualised │  + outcome  │
└─────────────┴─────────────┴─────────────┘
```
Best for: bedside rule derivation, decision aids.

## 2. Pathway / care journey (vertical)
```
┌─────────────────────┐
│  Patient (ICU/ED)   │
│       ↓             │
│  POCUS (3 checks)   │
│       ↓             │
│  Triple-negative?   │
│      / \            │
│   YES   NO          │
│    ↓     ↓          │
│ DEFER  DRAIN        │
└─────────────────────┘
```
Best for: clinical-care-pathway papers.

## 3. Before/after comparison (left-right)
```
┌──────────────┬──────────────┐
│  WITHOUT RULE│  WITH RULE   │
│  drain all?  │  triple-neg  │
│  PCC risk    │  → 7.7% TP   │
│              │  → safe defer│
└──────────────┴──────────────┘
```
Best for: intervention vs no-intervention, comparative effectiveness.

## 4. Anatomic-central with data callouts (radial)
```
        ┌────┐
        │ N= │
        └────┘
           ↘
  ┌───┐    💗    ┌───┐
  │AUC│  (heart) │NPV│
  └───┘          └───┘
           ↗
        ┌────┐
        │rule│
        └────┘
```
Best for: imaging journals (JASE, EHJ-CVI, Circulation: CVI) where the anatomic
illustration is the main visual.

## 5. Data-driven infographic (JAMA visual-abstract style)
```
┌─────────────────────────────────────┐
│  Title                              │
├───────────┬─────────────┬───────────┤
│ POPULATION│ METHODS     │ PRIMARY   │
│ N = 392   │ retrospect. │ OUTCOME   │
│           │ cohort      │ 7.7% TP   │
├───────────┴─────────────┴───────────┤
│  KEY MESSAGE (one line)             │
└─────────────────────────────────────┘
```
Best for: JAMA/Lancet/JACC style, results-forward.

## 6. ROC + clinical translation hybrid
```
┌──────────────┬──────────────┐
│ ROC curve    │  Rule:       │
│ (figure)     │  3 criteria  │
│ AUC = 0.78   │  ≥ 48 mm     │
│              │  no RespVar  │
│              │  no maligny  │
├──────────────┴──────────────┤
│  Defer-safe pocket (7.7%)   │
└─────────────────────────────┘
```
Best for: prediction-model papers needing to show statistical rigor.

## Default layout selection rule

- Triple-negative / hard-cutoff rule → **Layout 1 (three-zone horizontal)**
- Continuous predictor + cutoff → **Layout 6 (ROC + translation)**
- Clinical pathway emphasis → **Layout 2 (vertical pathway)**
- Comparative trial / RCT → **Layout 3 (before/after)**
- Imaging journal target → **Layout 4 (anatomic-central)**
- JAMA / Lancet target → **Layout 5 (infographic)**
