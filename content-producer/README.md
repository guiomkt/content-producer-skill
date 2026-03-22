# Content Producer Skill - README

## Quick Start

1. **User sends URL:** "Processa esse vídeo: [URL]"
2. **You transcribe:** Using tiktok-video-analyzer
3. **You improve:** Using Rafael's copywriting prompt (see SKILL.md)
4. **You ask:** "Salvar ou revisar?"
5. **You deliver:** Save to vault + sync, or send for review

## Rafael's Copywriting Prompt

The core of this skill is Rafael's professional copywriting prompt, which focuses on:

✅ **70% Substance + 30% Viral Elements**
- Real content that teaches something
- Strategic viral triggers (not fluff)

✅ **AIDA Structure**
- Attention: Specific hooks (not generic)
- Interest: Context and setup
- Desire: Main content with examples
- Action: Natural CTA

✅ **Profundity Rules**
- Develop points, don't just mention
- Explain WHY and HOW
- Use concrete comparisons
- Create "aha moments"

✅ **Teleprompter Ready**
- 10-18 words per sentence
- Natural spoken language
- Continuous text, no visual cues
- 150-250 total words (60-90 seconds)

See SKILL.md for the complete prompt embedded in the workflow.

## File Organization

```
/Claw/conteudo/
├── 2026-03-03.md (3 videos from March 3rd)
├── 2026-03-04.md (2 videos from March 4th)
└── ...
```

Each file contains:
- URL of original video
- Portuguese transcript
- Improved version ready to speak

## Example Output

**FILE:** `/Claw/conteudo/2026-03-03.md`

```markdown
## Vídeo 1 - Como ganhar dinheiro com IA

**URL:** https://vt.tiktok.com/ZSmwd44wp/

**Transcrição Original (Português):**
Ok, então aqui tem algo assustador mas também é uma forma bem fácil de ganhar dinheiro. Esse site se chama Renta Human AI...

**Versão Melhorada:**
Você sabia que 80% das pessoas cometem o mesmo erro quando tentam fazer isso? Eu cometi esse erro durante meses antes de entender o que estava errado.

A questão é que a maioria faz do jeito mais óbvio possível, que é também o jeito menos eficiente. E aí fica se perguntando por que o resultado não aparece.

Mas olha só o que acontece quando você muda uma coisa simples na abordagem...

[continues with full improved script]

---

## Vídeo 2 - [Next video title]
...
```

## Frequency

- **3+ videos per week** expected
- **5-10 minutes per video** (transcription + improvement + save)
- **Automatic sync** to Google Drive every 30 minutes

## Integration with Vault

- Saves to: `/Claw/conteudo/YYYY-MM-DD.md`
- Syncs via: `bash /data/.openclaw/scripts/sync-vault.sh`
- Visible in: Obsidian desktop + mobile (via Drive sync)

## When to Use Save vs Review

**SAVE** when:
- Quick test of the skill
- You trust the improvement output
- You want it live immediately

**REVIEW** when:
- First time testing a new topic
- You want to tweak the output
- You want to see both versions side-by-side
- You're unsure about something

## Tips for Best Results

1. **Be specific with topics:** "Processa esse vídeo sobre IA e copywriting" works better than just a link
2. **Check the Portuguese:** The translation should read naturally, not literally
3. **Read the improved script aloud:** It's designed for speaking, so hearing it matters
4. **Keep daily files:** One file per day makes it easier to find content later
5. **Use the vault:** Copy scripts directly from Obsidian to your teleprompter

## Troubleshooting

**Video won't transcribe?**
- Check if it's publicly available
- Try a different, shorter video first
- Manually provide the transcript if needed

**Translation seems off?**
- Ask me to review it in the chat first (choose "revisar" instead of "salvar")
- I'll fix the Portuguese before saving

**File sync issues?**
- Files save locally first
- Sync happens every 30 minutes automatically
- Can trigger manual sync with `bash /data/.openclaw/scripts/sync-vault.sh`

---

**Ready to start?** Send a URL and say "processa esse vídeo"! 🚀
