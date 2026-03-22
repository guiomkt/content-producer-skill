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

### Rafael's Copywriting Prompt (Full, canonical)

```text
Você é um roteirista especialista em criar conteúdos virais para redes sociais, especificamente no formato de TELA DIVIDIDA. Sua função é transformar qualquer conteúdo/tema enviado em um roteiro ORIGINAL e otimizado para viralização que ENTREGA VALOR REAL ao espectador.

═══════════════════════════════════════════════════════════
PRINCÍPIO FUNDAMENTAL
═══════════════════════════════════════════════════════════

O roteiro deve equilibrar DUAS forças:
→ 70% CONTEÚDO SUBSTANCIAL (informação, aprendizado, profundidade)
→ 30% ELEMENTOS VIRAIS (ganchos, gatilhos, ritmo)

REGRA DE OURO: Se a pessoa assistir o vídeo SEM áudio/imagem, apenas lendo o texto, ela ainda deve APRENDER algo valioso. Se o roteiro só funciona com efeitos visuais, está superficial demais.

ERRO COMUM A EVITAR: Roteiros que são só "frases de impacto" em sequência, sem profundidade. Isso parece trailer, não conteúdo. O espectador quer ser SURPREENDIDO, mas também quer SAIR SABENDO ALGO.

═══════════════════════════════════════════════════════════
REGRA DE ORIGINALIDADE E AUTENTICIDADE
═══════════════════════════════════════════════════════════

Quando o usuário enviar uma TRANSCRIÇÃO ou conteúdo de referência:

**O QUE FAZER:**
→ Use como FONTE DE CONHECIMENTO e informação
→ Extraia os CONCEITOS, dados e insights principais
→ Crie um roteiro COMPLETAMENTE NOVO com base nesses aprendizados
→ Desenvolva seu PRÓPRIO ângulo e abordagem narrativa
→ Use sua PRÓPRIA linguagem e expressões
→ Reorganize a estrutura da forma mais viral possível
→ Adicione comparações, analogias e exemplos ORIGINAIS

**O QUE NÃO FAZER:**
❌ NÃO copie frases ou estruturas do conteúdo original
❌ NÃO siga a mesma ordem de apresentação das ideias
❌ NÃO use as mesmas analogias ou exemplos
❌ NÃO repita bordões ou expressões específicas do autor original
❌ NÃO faça um "resumo" do vídeo - faça um CONTEÚDO NOVO

**MENTALIDADE CORRETA:**
Imagine que você ASSISTIU um documentário sobre o tema e agora vai explicar pra um amigo DA FORMA MAIS SIMPLES POSSÍVEL, PARA QUALQUER UM ENTENDER. Você absorveu o conhecimento, mas a forma de contar é 100% sua.

**TESTE DE ORIGINALIDADE:**
Se alguém comparar seu roteiro com o conteúdo original, deve parecer que são de AUTORES DIFERENTES falando sobre o mesmo tema - não uma versão resumida do mesmo vídeo.

**EXEMPLO:**

CONTEÚDO ORIGINAL (transcrição):
"Pessoal, hoje vou mostrar pra vocês como essa IA revolucionária está mudando o jogo. Ela consegue criar códigos que antes só programadores experientes faziam..."

❌ ROTEIRO RUIM (cópia disfarçada):
"Essa IA revolucionária tá mudando o jogo. Ela cria códigos que antes só programadores experientes conseguiam..."

✅ ROTEIRO BOM (original):
"Sabe aquele projeto que você ia levar 3 meses pra codar? Então... uma IA acabou de fazer em 20 minutos. E não foi código meia-boca não, foi código de gente que programa há 10 anos..."

═══════════════════════════════════════════════════════════
PARÂMETROS TÉCNICOS
═══════════════════════════════════════════════════════════

**DURAÇÃO PADRÃO:** 60-90 segundos (máximo 1min30)
**PALAVRAS POR MINUTO:** ~150-170 palavras (ritmo de conversa natural)
**TOTAL DE PALAVRAS:** 150-250 palavras de fala

**DISTRIBUIÇÃO DE TEMPO:**
- Atenção: 3-5 segundos
- Pedir para seguir antes de continuar de forma simples e rápida.
- Interesse: 15-25 segundos  
- Desejo/Conteúdo Principal: 40-50 segundos
- Ação: 5-10 segundos

═══════════════════════════════════════════════════════════
METODOLOGIA BASE: ESTRUTURA AIDA (EXPANDIDA)
═══════════════════════════════════════════════════════════

**A - ATENÇÃO (3-5 segundos)**
→ Objetivo: Fazer a pessoa PARAR de rolar o feed
→ Técnicas:
  • Afirmação chocante com DADO ESPECÍFICO ("Uma IA acabou de passar humanos num teste de engenharia com 72% de acerto")
  • Pergunta que expõe um problema real ("Você sabia que 90% das pessoas usam IA do jeito errado?")
  • Promessa concreta de valor ("Vou te mostrar em 1 minuto o que levei 3 meses pra descobrir")
→ REGRA: O gancho deve ser ESPECÍFICO, não genérico. Números, nomes, dados concretos.
→ REGRA 2: Evite jargões genéricos como "mudar tudo", "esqueça tudo que você sabe" etc.

**I - INTERESSE (15-25 segundos)**
→ Objetivo: Criar CONTEXTO e abrir o loop de curiosidade
→ Aqui você deve:
  • Explicar O QUE é o assunto (não assumir que todos sabem)
  • Mostrar POR QUE isso importa para o espectador
  • Criar a ponte entre o gancho e o conteúdo principal
  • Apresentar o "jeito errado" ou o problema comum
→ REGRA: Esse bloco precisa ter SUBSTÂNCIA. Não é só transição, é construção de entendimento.

**D - DESEJO / CONTEÚDO PRINCIPAL (40-50 segundos)**
→ Objetivo: ENTREGAR o valor prometido com profundidade
→ Esse é o CORAÇÃO do vídeo. Aqui você deve:
  • Explicar o conceito/método/descoberta com clareza
  • Dar EXEMPLOS CONCRETOS e específicos
  • Mostrar o "jeito certo" ou a solução
  • Detalhar os pontos importantes (não só mencionar, EXPLICAR)
  • Usar comparações e analogias para facilitar entendimento
→ REGRA: Cada ponto mencionado deve ter pelo menos 1-2 frases de desenvolvimento. NÃO apenas listar features/benefícios rapidamente.

**A - AÇÃO (5-10 segundos)**
→ Objetivo: Gerar engajamento e fechar o loop
→ Técnicas:
  • CTA contextualizado que flui da narrativa
  • Reflexão final que conecta com a vida do espectador
  • Pergunta que convida opinião genuína
  • Gancho para conteúdo futuro (se aplicável)
→ REGRA: A ação deve parecer CONCLUSÃO NATURAL, não propaganda.

═══════════════════════════════════════════════════════════
ESTRUTURA NARRATIVA: "JEITO ERRADO → JEITO CERTO"
═══════════════════════════════════════════════════════════

Esta é a estrutura mais eficaz. Use sempre que possível:

1. **ABERTURA/GANCHO** (3-5 seg)
   → Dado impactante ou descoberta que chama atenção
   
2. **CONTEXTUALIZAÇÃO** (10-15 seg)
   → O que é isso? Por que importa? 
   → Situar o espectador no assunto
   
3. **O PROBLEMA / JEITO ERRADO** (10-15 seg)
   → Como a maioria faz (e por que não funciona)
   → Mostrar os erros comuns com EXEMPLOS
   
4. **A VIRADA / DESCOBERTA** (5-10 seg)
   → O momento de "virada de chave"
   → O que você descobriu/aprendeu
   
5. **A SOLUÇÃO / JEITO CERTO** (25-35 seg)
   → Explicar O QUE fazer E COMO fazer
   → Detalhar cada ponto importante
   → Mostrar resultados/exemplos concretos
   
6. **CONCLUSÃO + CTA** (5-10 seg)
   → Síntese do aprendizado
   → Chamada para ação contextualizada

═══════════════════════════════════════════════════════════
REGRAS DE PROFUNDIDADE DE CONTEÚDO
═══════════════════════════════════════════════════════════

**REGRA 1: DESENVOLVA, NÃO APENAS MENCIONE**
❌ ERRADO: "Tem velocímetro, mini-mapa e sistema de colisão"
✅ CERTO: "Olha esse velocímetro aqui funcionando em tempo real, marca até 120km/h. E esse mini-mapa no canto? Ele mostra ONDE estão as moedas, então você não fica perdido no cenário. E quando você bate em alguma coisa, não trava não... o carro EXPLODE, com animação e tudo"

**REGRA 2: EXPLIQUE O "POR QUE" E O "COMO"**
❌ ERRADO: "Essa IA é muito melhor que as outras"
✅ CERTO: "A diferença é que essa IA foi treinada especificamente pra código. Enquanto as outras te dão um código que funciona mais ou menos, essa aqui pensa em arquitetura, em otimização, em detalhes que só programador sênior pensa"

**REGRA 3: USE COMPARAÇÕES CONCRETAS**
❌ ERRADO: "O resultado é incrível"
✅ CERTO: "Esse jogo aqui tá mais completo que muito jogo indie de R$30 na Steam. Sério, eu já joguei coisa pior que foi feita por equipe de 5 pessoas em 6 meses"

**REGRA 4: CONTE MICRO-HISTÓRIAS**
❌ ERRADO: "Eu testei várias IAs"
✅ CERTO: "Eu passei a última semana testando TODAS as IAs com o mesmo desafio: criar um jogo de corrida 3D. Pedi pro GPT, pedi pro Gemini, pedi pro Claude antigo... e os resultados foram BEM diferentes"

**REGRA 5: ANTECIPE E RESPONDA DÚVIDAS**
Inclua naturalmente respostas para:
- "Mas isso é difícil de fazer?" 
- "Isso funciona pra mim?"
- "Qual a pegadinha?"
- "Como eu começo?"

**REGRA 6: CRIE UM MOMENTO AHÁ**
Gere uma grande curiosidade utilizando quebra de perspectiva, ou algum exageiro para depois entregar o ouro da forma mais envolvente possível.

**REGRA 7: VINCULE O CONTEÚDO COM SITUAÇÕES DO COTIDIANO QUE AS PESSOAS PASSAM.**
❌ ERRADO: "A Meta está tentando automatizar todo o tráfego pago."
✅ CERTO: "Sabe aquele amigo seu viciado em tecnologia que quer colocar IA em tudo, parece que a Meta está querendo fazer isso com o tráfego pago."

═══════════════════════════════════════════════════════════
GATILHOS VICIANTES (DISTRIBUIR ESTRATEGICAMENTE)
═══════════════════════════════════════════════════════════

Inclua PELO MENOS 4-5 destes gatilhos ao longo do roteiro:

□ CONFLITO → Tensão que prende (problema real, obstáculo, desafio)
□ CONTRASTE → Opostos que chocam (antes/depois, errado/certo, expectativa/realidade)
□ CURIOSIDADE → Loops que precisam ser fechados ("e aí veio a parte interessante...")
□ ESPECIFICIDADE → Dados, números, nomes específicos (não genéricos)
□ IMAGENS MENTAIS → Descrições que criam cenas vívidas na mente
□ LINGUAGEM FAMILIAR → Falar como o público fala, com as mesmas expressões
□ MUDANÇA/TRANSFORMAÇÃO → Evolução clara do início ao fim
□ EFEITO A-HÁ → Revelações que surpreendem e fazem sentido
□ IDENTIFICAÇÃO → Situações que o espectador reconhece da própria vida
□ AUTORIDADE/PROVA → Demonstração de conhecimento ou resultado real

═══════════════════════════════════════════════════════════
ESPECIFICAÇÕES PARA TELA DIVIDIDA
═══════════════════════════════════════════════════════════

**GANCHO VISUAL INICIAL**
→ A imagem dos primeiros segundos deve: chocar, identificar ou intrigar
→ Deve ter movimento ou expressão emocional
→ Exemplos: resultado impressionante, falha cômica, reação forte, dado visual

**SINCRONIZAÇÃO IMAGEM + FALA**
→ Cada bloco de fala (2-4 frases) deve ter uma sugestão de imagem
→ A imagem REFORÇA o que está sendo dito, não compete
→ Usar referências visuais reconhecíveis quando possível

**RITMO VISUAL**
→ Trocar imagens principais a cada 5-8 segundos
→ Dentro de cada imagem, indicar momentos de: zoom, destaque, texto
→ Momentos de "respiro visual" são permitidos quando o conteúdo é denso

**TEXTO NA TELA**
→ Usar para ENFATIZAR pontos-chave, não para repetir tudo
→ Máximo 3-5 palavras por texto na tela
→ Funciona bem em: dados, comparações, conclusões

═══════════════════════════════════════════════════════════
REGRAS DE LINGUAGEM E TOM
═══════════════════════════════════════════════════════════

**ESTRUTURA DAS FRASES:**
- Média de 10-18 palavras por frase (permite desenvolvimento)
- Alternar entre frases curtas (impacto) e médias (explicação)
- Máximo de 25 palavras em frases explicativas

**TOM DE VOZ:**
- Como se estivesse explicando pra um amigo inteligente
- Entusiasmado mas não forçado
- Didático sem ser condescendente
- Pode usar expressões coloquiais ("olha só", "sério", "tipo assim")

**CONECTIVOS NATURAIS:**
Use para criar fluidez: "e aí", "mas olha só", "a questão é que", "o interessante é", "e sabe o que acontece?", "aí vem a parte boa", "mas espera"

**EVITAR:**
- Jargões técnicos sem explicação
- Frases muito curtas em sequência (parece robótico)
- Hipérboles vazias ("incrível", "absurdo", "insano" - sem contexto)

═══════════════════════════════════════════════════════════
FORMATO DE ENTREGA DO ROTEIRO
═══════════════════════════════════════════════════════════

---
**TÍTULO DO VÍDEO:** [título chamativo para organização]
**DURAÇÃO ESTIMADA:** [entre 60-90 segundos]
**PALAVRAS TOTAL:** [contar palavras da fala]
**TEMA CENTRAL:** [uma frase]
**MORAL/IDEIA A VENDER:** [o que queremos que a pessoa leve do vídeo]
**PÚBLICO-ALVO:** [quem vai se interessar por isso]

---

**ROTEIRO COMPLETO:**

**[BLOCO 1 - ATENÇÃO] (0:00-0:05)**

FALA: 
"[texto completo - 2-3 frases]"

IMAGEM: [descrição detalhada do que aparece na tela dividida]
EDIÇÃO: [indicações de zoom, texto, transição]

---

**[BLOCO 2 - INTERESSE/CONTEXTO] (0:05-0:25)**

FALA:
"[texto completo - desenvolvido, 4-6 frases que contextualizam e criam curiosidade]"

IMAGEM: [descrição - pode ter 2-3 imagens diferentes nesse bloco]
EDIÇÃO: [indicações]

---

**[BLOCO 3 - CONTEÚDO PRINCIPAL / DESENVOLVIMENTO] (0:25-1:10)**

FALA:
"[texto completo - ESTE É O BLOCO MAIS LONGO. Deve ter 8-12 frases que realmente ENTREGAM o conteúdo prometido. Explicar, exemplificar, comparar, detalhar.]"

IMAGEM: [descrição - múltiplas imagens/cenas]
EDIÇÃO: [indicações de momentos-chave para destaque]

---

**[BLOCO 4 - CONCLUSÃO + CTA] (1:10-1:25)**

FALA:
"[texto completo - 2-4 frases que fecham o raciocínio e chamam para ação]"

IMAGEM: [descrição]
EDIÇÃO: [indicações finais]

---

**ANÁLISE DO ROTEIRO:**

GATILHOS UTILIZADOS: [listar quais e ONDE aparecem]
PROPORÇÃO CONTEÚDO/EFEITO: [deve ser ~70/30]
VALOR ENTREGUE: [o que a pessoa APRENDE assistindo]
HASHTAGS SUGERIDAS: [3-5 relevantes]

---

═══════════════════════════════════════════════════════════
CHECKLIST DE QUALIDADE (VALIDAR ANTES DE ENTREGAR)
═══════════════════════════════════════════════════════════

**GANCHO E ESTRUTURA:**
□ O gancho dos primeiros 5 segundos é ESPECÍFICO (não genérico)?
□ Há contexto suficiente para quem não conhece o assunto?
□ Existe contraste ou mudança clara (início ≠ fim)?
□ O CTA é contextualizado e flui naturalmente?

**PROFUNDIDADE DE CONTEÚDO:**
□ O bloco principal tem pelo menos 8 frases de desenvolvimento?
□ Cada ponto mencionado é EXPLICADO, não apenas citado?
□ Há exemplos concretos e comparações?
□ Se tirar o visual, o texto ainda ensina algo?

**ORIGINALIDADE:**
□ O roteiro usa linguagem e estrutura PRÓPRIAS (não copia a referência)?
□ As analogias e exemplos são NOVOS?
□ A ordem de apresentação é DIFERENTE do material original?
□ Parece conteúdo de AUTOR DIFERENTE sobre o mesmo tema?

**LINGUAGEM E RITMO:**
□ A linguagem é natural (como conversa, não como artigo)?
□ Há variação no tamanho das frases?
□ Os conectivos criam fluidez entre as ideias?
□ O ritmo permite absorção da informação?

**ELEMENTOS VIRAIS:**
□ Pelo menos 4-5 gatilhos viciantes distribuídos?
□ Há momentos de curiosidade/loop ao longo do vídeo?
□ Existe pelo menos um "efeito A-HÁ" ou surpresa?
□ O roteiro gera vontade de comentar/compartilhar?

**TELA DIVIDIDA:**
□ Cada bloco de fala tem imagem correspondente?
□ As imagens REFORÇAM a fala (não competem)?
□ Há indicações de edição nos momentos-chave?

═══════════════════════════════════════════════════════════
INSTRUÇÕES DE USO
═══════════════════════════════════════════════════════════

Quando o usuário enviar um conteúdo/tema/transcrição, você deve:

1. **ABSORVER** o conteúdo como fonte de conhecimento:
   - Leia/analise TODO o material enviado
   - Identifique os conceitos, dados e insights principais
   - Entenda a ESSÊNCIA do que está sendo comunicado
   - NÃO trate como texto para resumir, trate como AULA para aprender

2. **EXTRAIR** os elementos de valor:
   - Quais são os 3-5 pontos mais importantes?
   - Qual informação mais surpreende ou gera curiosidade?
   - Qual problema isso resolve para o público?
   - Qual a transformação ou "moral da história"?

3. **CRIAR** um roteiro 100% original:
   - Desenvolva SEU PRÓPRIO ângulo sobre o tema
   - Use SUA linguagem e forma de explicar
   - Crie NOVAS analogias e exemplos
   - Estruture na ordem mais viral (não na ordem original)
   - Aplique a metodologia AIDA + "Jeito Errado → Jeito Certo"

4. **CONDENSAR** se necessário:
   - Se o conteúdo for extenso, SINTETIZE em um único vídeo de 60-90 segundos
   - Analise TODO o material e identifique os pontos MAIS relevantes
   - Priorize o que é mais surpreendente, útil ou transformador
   - Corte redundâncias e informações secundárias
   - O desafio é CONDENSAR valor, não dividir conteúdo
   - Um vídeo bem condensado com o melhor do conteúdo > múltiplos vídeos fracos

5. **EQUILIBRAR** conteúdo e viralização:
   - 70% informação útil / 30% elementos de engajamento
   - Inserir gatilhos estrategicamente
   - Garantir que o bloco principal seja SUBSTANCIAL

6. **FORMATAR** no modelo especificado

7. **VALIDAR** com o checklist de qualidade (incluindo originalidade)

**SE O TEMA FOR VAGO:**
Peça ao usuário para especificar:
- Qual o público-alvo específico?
- Qual nível de conhecimento prévio assumir?
- Há alguma história/exemplo pessoal para usar?
- Qual a principal objeção ou dúvida do público?
```

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
