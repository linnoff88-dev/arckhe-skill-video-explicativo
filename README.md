# ðŸŽ¬ Skill `video-explicativo` â€” VÃ­deos explicativos com Claude Code

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


RepositÃ³rio da Skill **`video-explicativo`** (Claude Code) + o **curso completo** no formato
**INEMA.CLUB** que ensina a usÃ¡-la. A Skill cria vÃ­deos explicativos narrados (**HTML â†’ MP4**) com o
framework [HyperFrames](https://github.com/heygen-com/hyperframes) â€” animados, em **PT-BR**, renderizados
**localmente, sem chave de API**, nos formatos **16:9** (YouTube) e **9:16** (Shorts/Reels).

> Este projeto nasceu de um vÃ­deo que foi, ele mesmo, **feito por uma Skill**.

**VersÃ£o da Skill:** `v1.10.3` Â· esquema `v1.yy.xxx` (yy = recurso, xxx = bug) Â· histÃ³rico em [`skill/video-explicativo/CHANGELOG.md`](skill/video-explicativo/CHANGELOG.md).

---

## ðŸ“¦ A Skill

A Skill `video-explicativo` estÃ¡ incluÃ­da neste repositÃ³rio:

- **Pasta navegÃ¡vel:** [`skill/video-explicativo/`](skill/video-explicativo/)
- **Download (.zip):** [`downloads/video-explicativo.zip`](downloads/video-explicativo.zip)

### InstalaÃ§Ã£o

Copie a pasta da skill para um dos diretÃ³rios que o Claude Code lÃª:

```bash
# Global (vale para todos os projetos)
cp -r skill/video-explicativo ~/.claude/skills/

# OU por projeto
cp -r skill/video-explicativo .claude/skills/
```

Ou baixe e descompacte o `.zip`:

```bash
unzip downloads/video-explicativo.zip -d ~/.claude/skills/
```

### Como usar

Depois de instalada, basta pedir um vÃ­deo ao Claude Code em linguagem natural:

> *"faÃ§a um vÃ­deo explicativo sobre X, em 16:9 e 9:16, terminando com a CTA do INEMA.CLUB."*

A Skill dispara automaticamente em pedidos como *"fazer um vÃ­deo"*, *"vÃ­deo explicativo"*,
*"vÃ­deo sobre X"*, *"vÃ­deo pra Shorts/Reels"*, *"mini tutorial em vÃ­deo"* ou *"vÃ­deo do INEMA"*.

### PrÃ©-requisitos

Roda 100% local, sem chave de API. VocÃª precisa de:

- **Node 22+** e **FFmpeg** (no git-bash, use `ffmpeg -nostdin`).
- **Chrome headless** do HyperFrames: `npx hyperframes browser ensure`.
- **Voz `bella` (inemavox):** narraÃ§Ã£o via `inemavox/tts_direct.py` (`--engine chatterbox-vc`, ref `bella.wav`). **Fallback Kokoro** (`pip install kokoro-onnx soundfile`, voz `pf_dora`) sÃ³ se o inemavox falhar.
- DiagnÃ³stico: `npx hyperframes doctor`.

### O que a Skill faz (pipeline HTML â†’ MP4)

1. **Plano de cenas** â€” o nÂº de cenas sai do **assunto**, nÃ£o Ã© fixo: cada beat do arco vira uma cena (range 4â€“12 + CTA). Default ~1:40â€“2:00; se o usuÃ¡rio fixar o nÃºmero, ele manda.
2. **Roteiro** â€” `SCRIPT.md` com as cenas do plano, do primeiro princÃ­pio ao avanÃ§ado, narraÃ§Ã£o curta por cena.
3. **Projeto** â€” `npx hyperframes init` + house style dark premium.
4. **Fontes** â€” baixa `.woff2` locais (Sora / Inter / JetBrains Mono).
5. **NarraÃ§Ã£o** â€” gera os WAVs por cena com a voz `bella` (inemavox); Kokoro `pf_dora` Ã© fallback.
6. **ComposiÃ§Ã£o** â€” gerador **data-driven** (`SCENES[]` â†’ N dinÃ¢mico) monta HTML + animaÃ§Ã£o GSAP, com **mid-scene activity** (cÃ¢mera Ken Burns embutida = anti-slideshow) e a **CTA anexada automaticamente** como Ãºltima cena. Timing Ãºnico = Ã¡udio batido com a animaÃ§Ã£o.
7. **ValidaÃ§Ã£o** â€” `hyperframes lint` + `inspect` (0 erros de layout).
8. **Render** â€” gera **16:9** e **9:16** em `--quality high`, sempre terminando com a **cena de CTA do INEMA.CLUB**.

> Detalhes completos no [`skill/video-explicativo/SKILL.md`](skill/video-explicativo/SKILL.md) e nas
> [`references/`](skill/video-explicativo/references/) (pipeline, house style, gotchas).

### Stack
- **HyperFrames** â€” HTML â†’ MP4 via Chrome headless + FFmpeg
- **inemavox** â€” voz `bella` (clone chatterbox-vc), narraÃ§Ã£o PT-BR local Â· **Kokoro** `pf_dora` como fallback
- **GSAP** â€” animaÃ§Ã£o das cenas

---

## ðŸŽ¨ CustomizaÃ§Ã£o (o que estÃ¡ fixo na Skill)

A Skill tem **padrÃµes fixos** (CTA do INEMA.CLUB, paleta dark Ã¢mbar, voz, formatos). HÃ¡ dois nÃ­veis pra mudar:

- **Por vÃ­deo** (recomendado): a Skill copia o template como `build-index.mjs` no seu projeto â€” edite lÃ¡, sem alterar a Skill.
- **Mudar o padrÃ£o de todos os vÃ­deos**: edite os arquivos da Skill em
  [`skill/video-explicativo/scripts/`](skill/video-explicativo/scripts/) (e reinstale).

> Arquivo principal: [`scripts/composition-template.mjs`](skill/video-explicativo/scripts/composition-template.mjs)
> (no projeto, vira `build-index.mjs`).

### 1. Mudar a CTA (INEMA.CLUB â†’ outra marca/URL)

A CTA Ã© a **Ãºltima cena**, definida em `const CTA` no `composition-template.mjs` e anexada automaticamente (`ALL = [...SCENES, CTA]`). Edite o `html` dela:

```js
const CTA = {
  audio: 3.840,                 // duraÃ§Ã£o real do WAV da narraÃ§Ã£o da CTA
  caption: "Mais conteÃºdo em suamarca.com",
  html: (p) => `
    <div class="cta-eyebrow" id="${p}-eye">CONTINUA EM</div>
    <div class="cta-brand" id="${p}-brand"><span class="b1">SUA</span><span class="bdotsep">.</span><span class="b2">MARCA</span></div>
    <div class="rule center" id="${p}-rule"></div>
    <div class="cta-url mono" id="${p}-url"><span class="cta-globe">ðŸŒ</span>suamarca.com</div>
    ...`,
  anim: (at, p) => [ /* tweens GSAP da CTA */ ],
};
```

- `b1` = parte clara (creme), `b2` + `bdotsep` = parte Ã¢mbar (accent). Edite o texto dentro dos spans.
- `cta-eyebrow` = "CONTINUA EM"; `cta-url` = a URL exibida.
- **NarraÃ§Ã£o da CTA**: Ã© o Ãºltimo `sN.wav` do projeto â€” edite o `.txt` correspondente e gere o WAV de novo (expanda a URL pra fala, ex.: "sua marca ponto com"), e ajuste `CTA.audio` com a duraÃ§Ã£o real. Veja [`scripts/narration-template.sh`](skill/video-explicativo/scripts/narration-template.sh).
- **Estilo da CTA**: regras CSS `.cta-eyebrow` / `.cta-brand` / `.cta-url` (tamanho da marca, glow etc.) no mesmo arquivo.
- Para **remover** a CTA: troque `const ALL = [...SCENES, CTA]` por `const ALL = [...SCENES]`. (O padrÃ£o da casa Ã© **sempre manter** a CTA.)

### 2. Cores / paleta (dark premium Ã¢mbar)

Tokens CSS no topo do `composition-template.mjs` (bloco `:root`):

```css
--bg:#0D1321; --bg2:#1D2D44; --bg3:#3E5C76;
--fg:#F0EBD8; --muted:#748CAB;
--accent:#FFC300; --accent2:#FCA311; --code:#2EC4B6;
```

Troque `--accent`/`--accent2` para mudar a cor de destaque em todas as cenas. ReferÃªncia completa em
[`references/house-style.md`](skill/video-explicativo/references/house-style.md).

### 3. Fontes

PadrÃ£o: **Sora** (tÃ­tulos), **Inter** (corpo), **JetBrains Mono** (cÃ³digo/URLs), baixadas localmente por
[`scripts/fetch-fonts.mjs`](skill/video-explicativo/scripts/fetch-fonts.mjs). Edite a lista nesse script
para trocar as famÃ­lias (use sempre fontes locais `@font-face` â€” Google Fonts CDN some no render).

### 4. Voz da narraÃ§Ã£o (TTS)

Default = **voz `bella` (inemavox)**; **Kokoro `pf_dora` Ã© fallback** automÃ¡tico. Definida em
[`scripts/narration-template.sh`](skill/video-explicativo/scripts/narration-template.sh) (variÃ¡veis no topo):

```bash
# bella (inemavox) â€” grava generated.wav, o template renomeia para sN.wav:
python3 ~/projetos/inemavox/tts_direct.py --text "$(cat txt/s$i.txt)" --lang pt \
  --engine chatterbox-vc --ref ~/projetos/timesmkt3/media/voice-refs/bella.wav --outdir /tmp/tts_s$i
# fallback (sÃ³ se o inemavox falhar):
npx -y hyperframes tts "txt/s$i.txt" --voice pf_dora --speed 0.98 --output "audio/s$i.wav"
```

Para trocar a voz: mude `BELLA_REF` (outra referÃªncia de clone) ou `ENGINE`; o fallback usa `KOKORO_VOICE`.

### 5. Formato (16:9 / 9:16)

Controlado pela flag `--vertical` ao rodar o gerador (`1920Ã—1080` vs `1080Ã—1920`):

```bash
node build-index.mjs            # 16:9
node build-index.mjs --vertical # 9:16
```

Ajustes especÃ­ficos do vertical ficam nas regras CSS `body.v ...`.

### 6. NÃºmero de cenas (dinÃ¢mico)

NÃ£o hÃ¡ nÃºmero fixo: o nÂº de cenas = nÂº de itens no array `SCENES[]` do gerador. O assunto define quantas (range 4â€“12 + CTA); se vocÃª pedir um nÃºmero especÃ­fico, ele Ã© respeitado. A CTA Ã© sempre anexada no fim (`ALL = [...SCENES, CTA]`) â€” nÃ£o conta no total.

### 7. MÃºsica de fundo (opcional)

Defina o caminho de um leito sonoro no topo do gerador (fica baixo, o vÃ­deo inteiro):

```js
const MUSIC = "assets/audio/bg.mp3"; // null = sem mÃºsica
const MUSIC_VOL = 0.14;              // 0â€“1 (deixe a narraÃ§Ã£o na frente)
```

Use um arquivo **â‰¥ a duraÃ§Ã£o do vÃ­deo**. VÃ­deo/imagem como b-roll: ver [`references/clips-midia.md`](skill/video-explicativo/references/clips-midia.md).

### 8. Movimento / animaÃ§Ã£o

O movimento Ã© um **estilo definido**: toda cena compÃµe a partir do vocabulÃ¡rio `M.*` no gerador (`reveal`, `sweep`, `type`, `float`, `pulse`, `glow`, `countUp`â€¦), com **mid-scene activity** (cÃ¢mera Ken Burns) e deslocamentos que se ajustam ao formato (9:16 mais contido). Para mudar o "jeito" do movimento, ajuste os presets em `M` â€” nÃ£o escreva tweens soltos. CatÃ¡logo e princÃ­pios em [`references/motion.md`](skill/video-explicativo/references/motion.md).

---

## ðŸŽ“ O curso

Curso completo, no formato **INEMA.CLUB**, que ensina a Skill de ponta a ponta.

**ðŸŒ Acesse:** https://inematds.github.io/skill-video-explicativo/

### As 4 trilhas

| Trilha | Tema | O que cobre |
|--------|------|-------------|
| ðŸ§© **T1 â€” Skills no Claude Code** | Fundamentos | O que Ã© uma Skill, o `SKILL.md`, `name` + `description`, divulgaÃ§Ã£o progressiva, onde vivem |
| ðŸŽ¬ **T2 â€” Pipeline: HTML â†’ MP4** | PrÃ¡tico | Setup, roteiro, narraÃ§Ã£o TTS (Kokoro), composiÃ§Ã£o em GSAP, lint/inspect, render 16:9 e 9:16 |
| ðŸ”§ **T3 â€” Por dentro do skill** | AvanÃ§ado | Estrutura do skill, house style dark premium, o gerador `build-index.mjs`, gotchas de produÃ§Ã£o |
| ðŸ’¡ **T4 â€” AplicaÃ§Ãµes & Prompts** | PrÃ¡tico | YouTube, Shorts, onboarding, aulas, lanÃ§amentos + biblioteca de prompts prontos |

---

## ðŸ—‚ï¸ Estrutura do repositÃ³rio

```
index.html                 # Landing do curso
curso/
  index.html               # Ãndice das trilhas
  trilha1..4/
    index.html             # Ãndice da trilha (mapa + mÃ³dulos)
    modulo-X-Y.html        # PÃ¡ginas de mÃ³dulo (17 no total)
skill/video-explicativo/   # A Skill, navegÃ¡vel (SKILL.md + scripts + references)
downloads/                 # video-explicativo.zip (a Skill empacotada)
```

---

ConteÃºdo do [**INEMA.CLUB**](https://inema.club) Â· 2026

<!-- inema-backlink:v1 -->
## Mais no INEMA.CLUB

- [Ficha completa deste curso](https://www.inema.club/cursos/129-hyperframes-videos-explicativos-com-claude-code/)
- [Guia: como aprender inteligÃªncia artificial](https://www.inema.club/aprender-inteligencia-artificial/)
- [Todos os cursos](https://www.inema.club/cursos/)
<!-- /inema-backlink:v1 -->

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>