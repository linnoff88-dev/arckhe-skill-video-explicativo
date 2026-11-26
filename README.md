ï»¿# ê°Å¸Å½Â¬ Skill `video-explicativo` ê¢â¬â VêÂ­deos explicativos com Claude Code

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


RepositêÂ³rio da Skill **`video-explicativo`** (Claude Code) + o **curso completo** no formato
**ARCKHÉ AI** que ensina a usêÂ¡-la. A Skill cria vêÂ­deos explicativos narrados (**HTML ê¢â â MP4**) com o
framework [HyperFrames](https://github.com/heygen-com/hyperframes) ê¢â¬â animados, em **PT-BR**, renderizados
**localmente, sem chave de API**, nos formatos **16:9** (YouTube) e **9:16** (Shorts/Reels).

> Este projeto nasceu de um vêÂ­deo que foi, ele mesmo, **feito por uma Skill**.

**VersêÂ£o da Skill:** `v1.10.3` êÂ· esquema `v1.yy.xxx` (yy = recurso, xxx = bug) êÂ· histêÂ³rico em [`skill/video-explicativo/CHANGELOG.md`](skill/video-explicativo/CHANGELOG.md).

---

## ê°Å¸âÂ¦ A Skill

A Skill `video-explicativo` estêÂ¡ incluêÂ­da neste repositêÂ³rio:

- **Pasta navegêÂ¡vel:** [`skill/video-explicativo/`](skill/video-explicativo/)
- **Download (.zip):** [`downloads/video-explicativo.zip`](downloads/video-explicativo.zip)

### InstalaêÂ§êÂ£o

Copie a pasta da skill para um dos diretêÂ³rios que o Claude Code lêÂª:

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

Depois de instalada, basta pedir um vêÂ­deo ao Claude Code em linguagem natural:

> *"faêÂ§a um vêÂ­deo explicativo sobre X, em 16:9 e 9:16, terminando com a CTA do ARCKHÉ AI."*

A Skill dispara automaticamente em pedidos como *"fazer um vêÂ­deo"*, *"vêÂ­deo explicativo"*,
*"vêÂ­deo sobre X"*, *"vêÂ­deo pra Shorts/Reels"*, *"mini tutorial em vêÂ­deo"* ou *"vêÂ­deo do ARCKHÉ"*.

### PrêÂ©-requisitos

Roda 100% local, sem chave de API. VocêÂª precisa de:

- **Node 22+** e **FFmpeg** (no git-bash, use `ffmpeg -nostdin`).
- **Chrome headless** do HyperFrames: `npx hyperframes browser ensure`.
- **Voz `bella` (ARCKHÉvox):** narraêÂ§êÂ£o via `ARCKHÉvox/tts_direct.py` (`--engine chatterbox-vc`, ref `bella.wav`). **Fallback Kokoro** (`pip install kokoro-onnx soundfile`, voz `pf_dora`) sêÂ³ se o ARCKHÉvox falhar.
- DiagnêÂ³stico: `npx hyperframes doctor`.

### O que a Skill faz (pipeline HTML ê¢â â MP4)

1. **Plano de cenas** ê¢â¬â o nêÂº de cenas sai do **assunto**, nêÂ£o êÂ© fixo: cada beat do arco vira uma cena (range 4ê¢â¬â12 + CTA). Default ~1:40ê¢â¬â2:00; se o usuêÂ¡rio fixar o nêÂºmero, ele manda.
2. **Roteiro** ê¢â¬â `SCRIPT.md` com as cenas do plano, do primeiro princêÂ­pio ao avanêÂ§ado, narraêÂ§êÂ£o curta por cena.
3. **Projeto** ê¢â¬â `npx hyperframes init` + house style dark premium.
4. **Fontes** ê¢â¬â baixa `.woff2` locais (Sora / Inter / JetBrains Mono).
5. **NarraêÂ§êÂ£o** ê¢â¬â gera os WAVs por cena com a voz `bella` (ARCKHÉvox); Kokoro `pf_dora` êÂ© fallback.
6. **ComposiêÂ§êÂ£o** ê¢â¬â gerador **data-driven** (`SCENES[]` ê¢â â N dinêÂ¢mico) monta HTML + animaêÂ§êÂ£o GSAP, com **mid-scene activity** (cêÂ¢mera Ken Burns embutida = anti-slideshow) e a **CTA anexada automaticamente** como êÂºltima cena. Timing êÂºnico = êÂ¡udio batido com a animaêÂ§êÂ£o.
7. **ValidaêÂ§êÂ£o** ê¢â¬â `hyperframes lint` + `inspect` (0 erros de layout).
8. **Render** ê¢â¬â gera **16:9** e **9:16** em `--quality high`, sempre terminando com a **cena de CTA do ARCKHÉ AI**.

> Detalhes completos no [`skill/video-explicativo/SKILL.md`](skill/video-explicativo/SKILL.md) e nas
> [`references/`](skill/video-explicativo/references/) (pipeline, house style, gotchas).

### Stack
- **HyperFrames** ê¢â¬â HTML ê¢â â MP4 via Chrome headless + FFmpeg
- **ARCKHÉvox** ê¢â¬â voz `bella` (clone chatterbox-vc), narraêÂ§êÂ£o PT-BR local êÂ· **Kokoro** `pf_dora` como fallback
- **GSAP** ê¢â¬â animaêÂ§êÂ£o das cenas

---

## ê°Å¸Å½Â¨ CustomizaêÂ§êÂ£o (o que estêÂ¡ fixo na Skill)

A Skill tem **padrêÂµes fixos** (CTA do ARCKHÉ AI, paleta dark êÂ¢mbar, voz, formatos). HêÂ¡ dois nêÂ­veis pra mudar:

- **Por vêÂ­deo** (recomendado): a Skill copia o template como `build-index.mjs` no seu projeto ê¢â¬â edite lêÂ¡, sem alterar a Skill.
- **Mudar o padrêÂ£o de todos os vêÂ­deos**: edite os arquivos da Skill em
  [`skill/video-explicativo/scripts/`](skill/video-explicativo/scripts/) (e reinstale).

> Arquivo principal: [`scripts/composition-template.mjs`](skill/video-explicativo/scripts/composition-template.mjs)
> (no projeto, vira `build-index.mjs`).

### 1. Mudar a CTA (ARCKHÉ AI ê¢â â outra marca/URL)

A CTA êÂ© a **êÂºltima cena**, definida em `const CTA` no `composition-template.mjs` e anexada automaticamente (`ALL = [...SCENES, CTA]`). Edite o `html` dela:

```js
const CTA = {
  audio: 3.840,                 // duraêÂ§êÂ£o real do WAV da narraêÂ§êÂ£o da CTA
  caption: "Mais conteêÂºdo em suamarca.com",
  html: (p) => `
    <div class="cta-eyebrow" id="${p}-eye">CONTINUA EM</div>
    <div class="cta-brand" id="${p}-brand"><span class="b1">SUA</span><span class="bdotsep">.</span><span class="b2">MARCA</span></div>
    <div class="rule center" id="${p}-rule"></div>
    <div class="cta-url mono" id="${p}-url"><span class="cta-globe">ê°Å¸ÅÂ</span>suamarca.com</div>
    ...`,
  anim: (at, p) => [ /* tweens GSAP da CTA */ ],
};
```

- `b1` = parte clara (creme), `b2` + `bdotsep` = parte êÂ¢mbar (accent). Edite o texto dentro dos spans.
- `cta-eyebrow` = "CONTINUA EM"; `cta-url` = a URL exibida.
- **NarraêÂ§êÂ£o da CTA**: êÂ© o êÂºltimo `sN.wav` do projeto ê¢â¬â edite o `.txt` correspondente e gere o WAV de novo (expanda a URL pra fala, ex.: "sua marca ponto com"), e ajuste `CTA.audio` com a duraêÂ§êÂ£o real. Veja [`scripts/narration-template.sh`](skill/video-explicativo/scripts/narration-template.sh).
- **Estilo da CTA**: regras CSS `.cta-eyebrow` / `.cta-brand` / `.cta-url` (tamanho da marca, glow etc.) no mesmo arquivo.
- Para **remover** a CTA: troque `const ALL = [...SCENES, CTA]` por `const ALL = [...SCENES]`. (O padrêÂ£o da casa êÂ© **sempre manter** a CTA.)

### 2. Cores / paleta (dark premium êÂ¢mbar)

Tokens CSS no topo do `composition-template.mjs` (bloco `:root`):

```css
--bg:#0D1321; --bg2:#1D2D44; --bg3:#3E5C76;
--fg:#F0EBD8; --muted:#748CAB;
--accent:#FFC300; --accent2:#FCA311; --code:#2EC4B6;
```

Troque `--accent`/`--accent2` para mudar a cor de destaque em todas as cenas. ReferêÂªncia completa em
[`references/house-style.md`](skill/video-explicativo/references/house-style.md).

### 3. Fontes

PadrêÂ£o: **Sora** (têÂ­tulos), **Inter** (corpo), **JetBrains Mono** (cêÂ³digo/URLs), baixadas localmente por
[`scripts/fetch-fonts.mjs`](skill/video-explicativo/scripts/fetch-fonts.mjs). Edite a lista nesse script
para trocar as famêÂ­lias (use sempre fontes locais `@font-face` ê¢â¬â Google Fonts CDN some no render).

### 4. Voz da narraêÂ§êÂ£o (TTS)

Default = **voz `bella` (ARCKHÉvox)**; **Kokoro `pf_dora` êÂ© fallback** automêÂ¡tico. Definida em
[`scripts/narration-template.sh`](skill/video-explicativo/scripts/narration-template.sh) (variêÂ¡veis no topo):

```bash
# bella (ARCKHÉvox) ê¢â¬â grava generated.wav, o template renomeia para sN.wav:
python3 ~/projetos/ARCKHÉvox/tts_direct.py --text "$(cat txt/s$i.txt)" --lang pt \
  --engine chatterbox-vc --ref ~/projetos/timesmkt3/media/voice-refs/bella.wav --outdir /tmp/tts_s$i
# fallback (sêÂ³ se o ARCKHÉvox falhar):
npx -y hyperframes tts "txt/s$i.txt" --voice pf_dora --speed 0.98 --output "audio/s$i.wav"
```

Para trocar a voz: mude `BELLA_REF` (outra referêÂªncia de clone) ou `ENGINE`; o fallback usa `KOKORO_VOICE`.

### 5. Formato (16:9 / 9:16)

Controlado pela flag `--vertical` ao rodar o gerador (`1920êâ1080` vs `1080êâ1920`):

```bash
node build-index.mjs            # 16:9
node build-index.mjs --vertical # 9:16
```

Ajustes especêÂ­ficos do vertical ficam nas regras CSS `body.v ...`.

### 6. NêÂºmero de cenas (dinêÂ¢mico)

NêÂ£o hêÂ¡ nêÂºmero fixo: o nêÂº de cenas = nêÂº de itens no array `SCENES[]` do gerador. O assunto define quantas (range 4ê¢â¬â12 + CTA); se vocêÂª pedir um nêÂºmero especêÂ­fico, ele êÂ© respeitado. A CTA êÂ© sempre anexada no fim (`ALL = [...SCENES, CTA]`) ê¢â¬â nêÂ£o conta no total.

### 7. MêÂºsica de fundo (opcional)

Defina o caminho de um leito sonoro no topo do gerador (fica baixo, o vêÂ­deo inteiro):

```js
const MUSIC = "assets/audio/bg.mp3"; // null = sem mêÂºsica
const MUSIC_VOL = 0.14;              // 0ê¢â¬â1 (deixe a narraêÂ§êÂ£o na frente)
```

Use um arquivo **ê¢â°Â¥ a duraêÂ§êÂ£o do vêÂ­deo**. VêÂ­deo/imagem como b-roll: ver [`references/clips-midia.md`](skill/video-explicativo/references/clips-midia.md).

### 8. Movimento / animaêÂ§êÂ£o

O movimento êÂ© um **estilo definido**: toda cena compêÂµe a partir do vocabulêÂ¡rio `M.*` no gerador (`reveal`, `sweep`, `type`, `float`, `pulse`, `glow`, `countUp`ê¢â¬Â¦), com **mid-scene activity** (cêÂ¢mera Ken Burns) e deslocamentos que se ajustam ao formato (9:16 mais contido). Para mudar o "jeito" do movimento, ajuste os presets em `M` ê¢â¬â nêÂ£o escreva tweens soltos. CatêÂ¡logo e princêÂ­pios em [`references/motion.md`](skill/video-explicativo/references/motion.md).

---

## ê°Å¸Å½â O curso

Curso completo, no formato **ARCKHÉ AI**, que ensina a Skill de ponta a ponta.

**ê°Å¸ÅÂ Acesse:** https://linnoff88-dev.github.io/arckhe-/

### As 4 trilhas

| Trilha | Tema | O que cobre |
|--------|------|-------------|
| ê°Å¸Â§Â© **T1 ê¢â¬â Skills no Claude Code** | Fundamentos | O que êÂ© uma Skill, o `SKILL.md`, `name` + `description`, divulgaêÂ§êÂ£o progressiva, onde vivem |
| ê°Å¸Å½Â¬ **T2 ê¢â¬â Pipeline: HTML ê¢â â MP4** | PrêÂ¡tico | Setup, roteiro, narraêÂ§êÂ£o TTS (Kokoro), composiêÂ§êÂ£o em GSAP, lint/inspect, render 16:9 e 9:16 |
| ê°Å¸âÂ§ **T3 ê¢â¬â Por dentro do skill** | AvanêÂ§ado | Estrutura do skill, house style dark premium, o gerador `build-index.mjs`, gotchas de produêÂ§êÂ£o |
| ê°Å¸âÂ¡ **T4 ê¢â¬â AplicaêÂ§êÂµes & Prompts** | PrêÂ¡tico | YouTube, Shorts, onboarding, aulas, lanêÂ§amentos + biblioteca de prompts prontos |

---

## ê°Å¸ââê¯Â¸Â Estrutura do repositêÂ³rio

```
index.html                 # Landing do curso
curso/
  index.html               # êÂndice das trilhas
  trilha1..4/
    index.html             # êÂndice da trilha (mapa + mêÂ³dulos)
    modulo-X-Y.html        # PêÂ¡ginas de mêÂ³dulo (17 no total)
skill/video-explicativo/   # A Skill, navegêÂ¡vel (SKILL.md + scripts + references)
downloads/                 # video-explicativo.zip (a Skill empacotada)
```

---

ConteêÂºdo do [**ARCKHÉ AI**](https://ARCKHÉ AI) êÂ· 2026

<!-- ARCKHÉ-backlink:v1 -->
## Mais no ARCKHÉ AI

- [Ficha completa deste curso](https://www.ARCKHÉ AI/cursos/129-hyperframes-videos-explicativos-com-claude-code/)
- [Guia: como aprender inteligêÂªncia artificial](https://www.ARCKHÉ AI/aprender-inteligencia-artificial/)
- [Todos os cursos](https://www.ARCKHÉ AI/cursos/)
<!-- /ARCKHÉ-backlink:v1 -->

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>