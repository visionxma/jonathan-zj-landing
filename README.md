# Landing Page — Jonatan

Clone da estrutura da **`gidogreen.com/grupo-gg/`**, com a copy e as fotos adaptadas
para o Jonatan. Os valores de cor, tipografia, espaçamento e raio foram extraídos do
`post-816.css` da própria página deles, usando os overrides do breakpoint mobile
(`max-width:767px`) — que é o que se aplica na coluna de 480px.

Seções, na ordem: hero → o que rola lá dentro → uma comunidade que já tá em campo →
começar é simples → bora pra dentro? → FAQ → barra legal.

| | valor do gg |
|---|---|
| Fundo escuro | `#0F1610` |
| Fundo verde | `#114F2A` |
| FAQ | `#040E08`, card em gradiente `#004E23 → #021008` |
| Cards claros | `#F4F4F4`, título preto, descrição `#114F2A` |
| Cards escuros | `#0A351B`, borda `#B0B4B0`, título laranja |
| Títulos | Bebas Neue, `#FF9F39` |
| Corpo | Inter, `#B0B4B0` |
| Botão | `#1446FF`, Inter 18px 700 uppercase, raio 12px, `#EBEEF2` |
| Carrossel | borda `#9FE500`, bolinhas `#FF9F39` |

## Arquivos

```
.
├── index.html              ← página inteira (HTML + CSS + JS embutidos)
├── assets/img/             ← hero em 3 tamanhos (jpg + webp)
└── README.md
```

O hero é servido em `hero-800/1400/2000` (`.webp` com `.jpg` de fallback), gerados a
partir do `assets/img/hero.jpg`. Para regerar depois de trocar a foto:

```bash
cd assets/img
for w in 800 1400 2000; do
  sips --resampleWidth $w hero.jpg --setProperty format jpeg \
       --setProperty formatOptions 82 --out hero-$w.jpg
  cwebp -q 80 hero-$w.jpg -o hero-$w.webp
done
```

As 14 fotos originais estão em `_originais/`. Nada da página aponta pra lá — é só
backup, pra fonte não existir só numa máquina.

Não há build, dependência nem framework. É só subir esta pasta em qualquer
hospedagem estática (GitHub Pages, Hostinger, Vercel, Netlify, cPanel, S3…).

## Testar localmente

Abrir `index.html` direto no navegador já funciona. Ou:

```bash
python3 -m http.server 8901
# http://localhost:8901
```

## Ajustes rápidos

| O que mudar | Onde |
|---|---|
| **Link do grupo** | `index.html`, no fim do arquivo: `var GRUPO_WHATSAPP = "..."` — vale para todos os 4 botões de uma vez |
| Cores e fonte | bloco `:root` no `<style>` (`--p1`, `--p2`, `--bg`, `--font`) |
| Largura da coluna | `--col` no `:root` |
| Textos | direto no HTML, tudo em português e comentado |
| Foto do hero | trocar `hero.png` e regerar (comando acima) |


## Rastreio

### Pixel & Link Manager

Instalado no topo do `<head>`, **antes do GTM** — é o primeiro script da página.
Ele faz duas coisas:

1. Busca no Supabase os pixels cadastrados para o domínio e injeta cada um
   (Facebook, GA, GTM, TikTok ou custom).
2. Intercepta todo link de WhatsApp/Telegram e troca pelo rotacionador
   (`whatsapp-redirect`), registrando o clique.

O casamento é por **domínio + caminho**. O domínio de produção já está cadastrado:

```
jonatan.jogadorpro.com   → site id 356ede0d-0215-428a-a021-463795bf14e7
```

Se trocar de domínio, cadastre o novo na aba "Grupos" do painel, exatamente como
aparece na barra de endereço (sem `https://`, sem barra final).

**Proteção anti-cache (não remova):** as duas consultas usam `cache: 'no-store'`,
header `Cache-Control: no-cache` e um parâmetro `&_=timestamp`; o mesmo parâmetro vai
no redirect. Sem isso o Safari/iOS serve link de grupo antigo do cache.

**Ordem dos eventos no clique do CTA:** o handler da página roda primeiro e empurra
`join_group` pro dataLayer; só depois o interceptador dá `preventDefault` e abre o
rotacionador. Os dois convivem porque, num mesmo elemento, os listeners disparam na
ordem em que foram registrados — o da página é registrado no parse, o do
interceptador só depois que o fetch resolve.

### Google Tag Manager

Container `GTM-PQGGFP2K`, script no `<head>` e `<noscript>` após o `<body>`.
Todo clique em CTA empurra:

```js
{ event: 'join_group', method: 'whatsapp', cta: 'hero' | 'final' }
```

No GTM, crie um acionador **Evento personalizado** com nome `join_group`.

> Atenção: se você também cadastrar o `GTM-PQGGFP2K` como pixel do tipo
> `google_tag_manager` no painel, o container vai carregar duas vezes e os eventos
> contam dobrado. Deixe em só um dos dois lugares.

## Pontos que dependem de você

- **Números do hero** (`+15 anos`, `04 países`, `30`) e a bio dos clubes foram escritos a
  partir das fotos (Internacional, Criciúma, FC Lviv, Metalist, Catar). Confirme com o
  Jonatan antes de publicar.
- **Links do rodapé** (Termos, Privacidade, Jogo responsável) estão como `#`.
- Não há formulário de captura de e-mail/telefone: o botão leva direto pro WhatsApp.
  O modelo tem um modal antes do redirect — dá pra adicionar se quiser gerar lista.
