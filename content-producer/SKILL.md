---
name: content-producer
description: Transform video transcripts into viral social media scripts (split-screen format). Transcribe, translate to Portuguese, improve with professional copywriting prompt, and save to vault. Works with TikTok, YouTube, Instagram, and 1000+ sites.
author: Rafael Riedel / Clawdia
version: 1.0.0
---

# Content Producer Skill

Transform raw video transcripts into publication-ready, viral social media scripts optimized for teleprompter delivery and split-screen format.

## When to Use This Skill

Activate when the user says:
- "Crie um conteúdo desse vídeo [URL]" (create content from this video)
- "Cria conteúdo desse vídeo [URL]"
- "Transforma esse vídeo em roteiro [URL]"

**DIFERENÇA IMPORTANTE:**
- "Transcreve esse vídeo [URL]" → Use tiktok-video-analyzer skill ONLY (transcription only, sent here in chat)
- "Crie um conteúdo desse vídeo [URL]" → Use content-producer skill (full workflow: transcribe + improve + save to vault)

## Workflow Overview

```
User provides URL
    ↓
[1] Transcribe video (tiktok-video-analyzer)
    ↓
[2] Translate to Portuguese
    ↓
[3] Improve with Rafael's copywriting prompt
    ↓
[4] Ask: Salvar automaticamente ou revisar? (save/review)
    ↓
IF SAVE:
  → Create /Claw/conteudo/YYYY-MM-DD.md
  → Include: URL + PT transcript + improved version
  → Sync with Drive via rclone
  → Confirm completion
  
IF REVIEW:
  → Send full content to chat
  → Wait for Rafael's feedback/approval
  → Then save to vault
```

## Step-by-Step Execution

### Step 1: Transcribe
```bash
python3 ~/.openclaw/workspace/skills/tiktok-video-analyzer/transcribe.py --download-only "[URL]"
python3 ~/.openclaw/workspace/skills/tiktok-video-analyzer/transcribe.py --transcribe-only "[VIDEO_ID]"
```

Returns JSON with `transcript` (in English, usually).

### Step 2: Translate to Portuguese
Take the English transcript and translate to Portuguese using Claude's translation capability. Preserve tone and meaning.

### Step 3: Improve with Copywriting Prompt
Apply Rafael's copywriting prompt exactly as defined. The prompt is embedded in this skill and covers:

- 70% substantial content + 30% viral elements
- AIDA structure (Attention, Interest, Desire, Action)
- 4-5 strategic viral triggers
- Natural teleprompter language (10-18 words per sentence)
- Concrete examples, not hyperbole
- Depth: develop, don't just mention

**RAFAEL'S COPYWRITING PROMPT:** [See full prompt in AGENTS.md under "Skill save-money" — use that exact structure and rules]

### Step 4: Ask for Save or Review
After delivering the improved script, ask:

"Confirma? Salvar automaticamente ou revisar aqui? (salvar/revisar)"

**IF SALVAR (Save):**
- Extract video title or create meaningful filename
- Create file: `/Claw/conteudo/YYYY-MM-DD.md`
- Structure:
  ```markdown
  ## Vídeo X - [Description/Title]
  
  **URL:** [original URL]
  
  **Transcrição Original (Português):**
  [Portuguese transcript]
  
  **Versão Melhorada:**
  [Improved script ready for teleprompter]
  
  ---
  ```
- If multiple videos same day, append to existing file
- After saving, trigger vault sync: `bash /data/.openclaw/scripts/sync-vault.sh`
- Confirm: "✅ Salvo em /Claw/conteudo/YYYY-MM-DD.md e sincronizado com Drive!"

**IF REVISAR (Review):**
- Send full content here in chat (URL + Portuguese transcript + improved version)
- Ask: "Quer que eu ajuste algo ou já salvo?"
- Wait for feedback
- After approval, save to vault following the SALVAR flow

## Parameters & Standards

**VIDEO DURATION:** 30-120 seconds (automatically transcribed, might be shorter/longer)

**SCRIPT PARAMETERS:**
- Duration: 60-90 seconds (target for social media)
- Words per minute: 150-170 (natural conversation pace)
- Total words: 150-250 spoken words

**TIME DISTRIBUTION (for the improved script):**
- Attention: 3-5 seconds
- Interest/Context: 15-25 seconds
- Main Content: 40-50 seconds
- CTA/Conclusion: 5-10 seconds

## File Structure in Vault

**Location:** `/Claw/conteudo/`

**Naming:** `YYYY-MM-DD.md` (one file per day, multiple videos appended)

**Content structure per video:**
- URL (original source)
- Portuguese transcript (readable, not phonetic)
- Improved version (ready for teleprompter, no visual cues)

## Important Rules

**RULE 1:** Always transcribe to Portuguese FIRST, then improve
- The copywriting prompt expects to work with Portuguese text
- Do not apply the prompt to English transcripts

**RULE 2:** The improved script must be teleprompter-ready
- Continuous text, no markers or block divisions
- No visual cues like "[cut to b-roll]" or "[show graphic]"
- Exactly as it will be spoken, with natural paragraph breaks for breathing

**RULE 3:** Preserve original information but add depth
- Don't invent data or examples
- Expand existing points per the copywriting prompt
- Make connections more explicit

**RULE 4:** Save/Review decision is USER'S CHOICE
- Always offer both options
- Don't pre-decide or force saving
- If they want to review, respect that

**RULE 5:** Sync only after saving
- Use: `bash /data/.openclaw/scripts/sync-vault.sh`
- Confirm sync completion to user
- File should appear in Obsidian + Google Drive within 30 minutes

## Frequency & Usage

**Expected frequency:** 3+ videos per week
**Time per video:** ~5-10 minutes (transcription + improvement + save)
**Output:** Portuguese-language scripts ready for re-recording

## Error Handling

| Error | Solution |
|-------|----------|
| Video is private/removed | "This video is private or has been removed. Try a different URL." |
| Transcription fails | Fall back to manual transcript if user provides one, or ask for different video |
| Translation issues | Always review Portuguese translation for accuracy before improvement step |
| File already exists | Append new video to existing YYYY-MM-DD.md file with clear separator |
| Sync fails | Notify user and save locally; sync will catch up next 30-min cycle |

## Example Interaction

**User:** "Processa esse vídeo: https://vt.tiktok.com/ZSmwd44wp/"

**You:** 
```
📡 Vídeo recebido, analisando...
📥 Baixado! Transcrevendo agora...
```

[After transcription and improvement]

**You:**
```
✅ Pronto!

Confirma? Salvar automaticamente ou revisar aqui? (salvar/revisar)
```

**User:** "salvar"

**You:**
```
✅ Salvo em /Claw/conteudo/2026-03-03.md e sincronizado com Drive!

Seu script está pronto para regravar. Pode copiar direto do Obsidian quando quiser!
```

## Notes for Clawdia

- This skill is a workflow orchestrator, not a script file
- You follow these steps when Rafael requests content processing
- The copywriting prompt is your core tool; apply it consistently
- Always respect Rafael's save/review choice
- Sync is automatic post-save; confirm it to the user
- Keep track of daily files (1 file per day in /Claw/conteudo/)

---

**Status:** Ready for production
**Last Updated:** 2026-03-03
