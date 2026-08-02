# graphical-abstract-prompter

A [Claude Code](https://claude.com/claude-code) skill that turns a finished
manuscript + its headline finding into a **ready-to-paste prompt** for image
generation AIs (Midjourney / DALL·E / Adobe Firefly / Stable Diffusion) to
produce a journal-quality **graphical abstract**.

It does **not** generate the image itself — it writes the prompt (and an
optional SVG wireframe) so you paste it into whichever tool you prefer.
Per-journal style guides, B&W-safe palettes, and medical-illustration
conventions are applied automatically.

Part of the `clinical-research-harness` (Phase 7, after manuscript writing).

## Install

Drop the folder into your Claude Code skills directory:

```
~/.claude/skills/graphical-abstract-prompter/
```

(or keep it inside the `clinical-research-harness` plugin under
`skills/graphical-abstract-prompter/`). Restart Claude Code so it picks up
the skill.

## Usage

Just mention it in natural language — the skill auto-triggers on
`graphical abstract`, `visual abstract`, `그래픽 초록`, or `abstract figure`:

```
내 manuscript로 graphical abstract 프롬프트 만들어줘
```

or invoke explicitly:

```
/graphical-abstract-prompter
```

### Inputs

**Required**
- `manuscript_path` — finished manuscript, `.docx` or `.md`
- `headline_finding` — one-line core result
  (e.g. `LAVI ≥ 52 mL/m² + RespVar(−) + No malignancy → 7.7% tamponade rate`)

**Optional**
- `target_journal` — JASE / EHJ-CVI / Circulation / NEJM / JAMA / Lancet / … (applies that journal's convention)
- `style_preference` — `clinical-illustration` · `schematic-flow` · `data-driven-infographic` · `before-after` · `patient-pathway`
- `color_palette` — `B&W safe` (default) · `single-accent-color` · `journal-house-style`
- `aspect_ratio` — `1:1` (default) · `16:9` · `4:3` · `2:1 banner`
- `image_tool` — `midjourney` · `dalle3` · `firefly` · `stable-diffusion` · `biorender-manual` · `generic` (default)

### Outputs (`_graphical_abstract/`)

| File | What |
|------|------|
| `prompt_<tool>.txt` | Tool-specific prompt, paste-and-go |
| `prompt_full.md` | Structured full prompt + text-label list + ICMJE AI-disclosure note |
| `layout_wireframe.svg` | Optional panel/label wireframe for manual BioRender/Inkscape work |
| `prompt_summary.json` | Metadata (manuscript hash, headline finding, target journal) |

Then: paste `prompt_<tool>.txt` into your image tool → iterate 1–3× on labels/colors
→ save the final as `_manuscript/Graphical_Abstract.png` for the submission package.

## Guardrails

- No real patient/physician names or identifiable images — always anonymized, generalized illustration.
- Key numbers (AUC, NPV, cutoff) must match the manuscript exactly — no LLM guessing.
- B&W-safe palette by default (cheaper color printing).
- No `"in the style of [real illustrator]"` phrasing — avoids copyright conflict.
- ICMJE AI-disclosure text auto-appended.

## Files

- `SKILL.md` — the skill definition
- `references/journal_styles.md` — per-journal graphical-abstract conventions
- `references/layout_patterns.md` — standard layout patterns
- `references/prompt_templates.md` — per-tool prompt syntax
