# Image-tool–specific Prompt Templates (5)

## Midjourney v6 / v6.1
```
<scene description>, <medical illustration style>, <color palette>,
<key text labels in quotes>, <no-patient-identifying-features>,
clean vector medical illustration, no photorealism --ar <ratio> --style raw --v 6 --no logo, photograph
```
Key flags: `--ar 1:1` / `--ar 16:9`, `--style raw` (less stylised),
`--no logo, photograph` (negative prompts).

## DALL-E 3 (natural language)
- Use plain English narrative paragraph
- Place text labels inside quotation marks (DALL-E will attempt to render them)
- Conclude with style directives: "Style: clean medical illustration, vector, 
  no photorealism, no identifying patient features."

## Adobe Firefly (text-to-image)
- Supports `--style` keyword set (e.g., `illustration`, `vector`, `infographic`)
- Mostly natural language with style suffix
- Native support for medical / scientific illustration
- B&W output: append `monochrome` keyword

## Stable Diffusion (XL or 3) via local UI
- More technical control: CFG scale, sampler, etc.
- Positive prompt: scene + style
- Negative prompt: "photorealism, patient face, blurry, watermark, text artifacts"
- Useful LoRAs: scientific illustration, infographic

## BioRender (manual, paste reference)
- Skill outputs a description that the user pastes into BioRender's
  search → drag-and-drop to assemble manually
- Best for very anatomically accurate figures
- Include: "Search BioRender for: heart anatomy, pericardial effusion,
  echo probe, intensive care, decision flow"

## Generic (default — works in all the above)
- Plain English narrative, ≤ 100 words
- Quoted text labels for required on-image text
- Style directive: "clean medical vector illustration, no patient
  identifying features, B&W with single accent color"
- Aspect ratio mentioned in English ("square image" / "16:9 wide")

## ICMJE AI disclosure (auto-append to prompt_full.md)
> "Graphical abstract generated with assistance from [tool name] using a
> structured prompt produced by the clinical-research-harness graphical-
> abstract-prompter skill (Phase 7). The PI verified all on-image text,
> numbers and anatomical accuracy. No patient-identifiable data was
> used in generation."
