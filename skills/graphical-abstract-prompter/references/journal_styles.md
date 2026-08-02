# Journal-specific Graphical Abstract Style Guide

This file is referenced by the `graphical-abstract-prompter` skill to adapt
the generated prompt to the target journal's conventions.

## Journal styles (9 most relevant for cardiology / CCM)

### JASE (Journal of the American Society of Echocardiography)
- Format: ≤ 1 panel, square or 4:3
- Tone: clean medical illustration; echo loop motifs welcome
- Convention: echo views (apical 4-ch, PSLAX) as core motif
- Required elements: study design + key result + clinical takeaway in 3 zones
- Color: full color allowed; B&W safe preferred for cost
- Text density: minimal — 3–4 short phrases max

### EHJ-CVI (European Heart Journal — Cardiovascular Imaging)
- Format: rectangular 16:9 or 2:1 banner
- Tone: schematic / data-forward; histogram or ROC inset welcome
- Convention: "Methods → Findings → Clinical Translation" left-to-right flow
- Color: ESC house color (Pantone 187 red) for accent welcomed

### Circulation / Circulation: Cardiovascular Imaging
- Format: square, 1:1, 1200×1200 px
- Tone: high-design, AHA-house style; minimal text density
- Convention: central anatomical figure with peripheral data callouts
- Color: AHA red for accent

### NEJM
- Format: rarely accepts; if accepted, schematic with ≤ 6 elements
- Tone: very restrained, scientific illustration
- Convention: 2–3 panels, sequential reasoning

### JAMA / JAMA Cardiology
- Format: square 1:1, "Visual Abstract" style
- Tone: data-driven infographic, large numbers, comparison bars
- Convention: arrows showing primary outcome comparison (intervention vs control or rule vs not-rule)
- Color: JAMA blue accent

### Lancet / Lancet Digit Health / Lancet Respir Med
- Format: square 1:1; minimal house style requirement
- Tone: clinical-translation, real-world
- Convention: patient pathway / care journey welcomed

### Acute and Critical Care (대한중환자의학회지)
- Format: square or 4:3
- Tone: pragmatic ICU-bedside, POCUS / bedside ultrasound motifs preferred
- Convention: "bedside dilemma → rule → action" flow
- Color: any; print is grayscale-tolerant
- Text language: English (Korean abstract allowed)

### Korean Circulation Journal / Korean J Internal Medicine
- Format: square preferred
- Tone: standard medical illustration
- Convention: methods → result → conclusion vertical or horizontal flow

### Annals of Intensive Care / Journal of Intensive Care
- Format: square or 4:3
- Tone: ICU clinical practice forward, POCUS welcomed
- Convention: "decision tree" style preferred; rule branches visualised

## Default convention if `target_journal` unspecified

- Aspect 1:1 (square)
- Layout: 3-zone horizontal: Study population → Rule → Clinical action
- Color: B&W with one accent
- Text: ≤ 50 words on the image
- Anatomical accuracy required for cardiac / echo motifs
- No patient identifying features
