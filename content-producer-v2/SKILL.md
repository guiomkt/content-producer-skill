---
name: content-producer-v2
description: Transform short-form video references (TikTok, Reels, YouTube Shorts, and similar links) into an original, high-quality Portuguese script for split-screen social content. Use when the user asks to create content/roteiro/script from a video URL or transcript. Do not use for transcription-only requests.
---

# Content Producer V2

Replicate the production workflow for the user: transcribe source content, adapt to PT-BR, rewrite with the canonical roteiro optimization prompt, and then either save to vault or review in chat.

## Workflow Decision

Use this skill when the user intent is **creation**:
- "Crie um conteúdo desse vídeo [URL]"
- "Transforma esse vídeo em roteiro"
- "Me gera um script desse link"

Do **not** use this skill for transcription-only requests:
- "Transcreve esse vídeo"
- "Só quero a transcrição"

For transcription-only, route to `tiktok-video-analyzer`.

## Execution Flow (mandatory order)

1. **Receive URL or transcript source**.
2. **Transcribe** with `tiktok-video-analyzer` when source is a video URL.
3. **Translate/adapt** transcript to PT-BR natural language.
4. **Load and apply canonical prompt** from `references/roteiro-otimizacao-prompt.md`.
5. **Generate final script** with split-screen format requirements.
6. **Ask user decision**: `salvar` or `revisar`.
7. If `revisar`: show full output, collect adjustments, then ask confirmation to save.
8. If `salvar` (or approved after review): append to daily vault file and sync.

Never skip step 6.

## Canonical Prompt Policy

Before rewriting the script, always read:
- `references/roteiro-otimizacao-prompt.md`

Rules:
- Treat that file as the single source of truth for style and quality.
- If any other instruction conflicts, prioritize the canonical prompt for script generation quality.
- Do not rely on external docs (e.g., AGENTS snippets) for this prompt.

## Output Requirements

### 1) Chat output before save
After generating the script, ask exactly:

`Confirma? Salvar automaticamente ou revisar aqui? (salvar/revisar)`

### 2) Vault output format
Path:
- `/path/to/your/vault/Claw/conteudo/YYYY-MM-DD.md`

Append each processed video using:

```markdown
## Vídeo N — [Título inferido]

**URL:** [link original]

**Transcrição (PT-BR):**
[texto traduzido/adaptado]

**Roteiro Final (teleprompter-ready):**
[roteiro final]

---
```

Rules:
- One file per day.
- Append; never overwrite old entries.
- If same URL already exists in the daily file, warn and ask confirmation before duplicating.

### 3) Sync after save
After writing the file, run:

```bash
bash /path/to/your/scripts/sync-vault.sh
```

Then confirm success status to user.

## Progress Messages (recommended)

- `📡 Vídeo recebido, analisando...`
- `📥 Baixado! Transcrevendo agora...`
- `✅ Pronto! Confirma? Salvar automaticamente ou revisar aqui? (salvar/revisar)`

## Error Handling

- Invalid/missing URL: ask for full valid link.
- Private/removed video: inform it is unavailable and ask another link.
- Transcription failure: offer retry or manual transcript input.
- Save failure: report save error clearly and keep content in chat for manual copy.
- Sync failure: report that file was saved locally but sync failed.

## Quality Gate (before delivery)

Validate all items:
- Script is original (not paraphrase-copy of source).
- PT-BR is natural and spoken.
- Core value is substantive (not only viral phrases).
- Split-screen format blocks are present.
- Script follows the canonical prompt checklist from reference file.

If quality is below standard, revise before presenting to user.
