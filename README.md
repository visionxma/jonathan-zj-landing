# Landing Page — Jonathan ZJ

Squeeze page de uma tela para o grupo gratuito de WhatsApp: hero de tela cheia com a
foto ao fundo, card de perks, CTA e rodapé legal.

O sistema visual vem da **`gidogreen.com/grupo-gg/`**: fundo verde quase preto
(`#040C0D`), superfície `#114F2A`, títulos em laranja `#FF9F39`, CTA azul `#1446FF`
com borda lima `#9FE500` e ícones de círculo laranja com glifo verde `#035128`.
A tipografia **não** é a deles: uma família só (Archivo), usando o eixo de largura
variável para os títulos condensados em caixa-alta.

O layout é travado numa **coluna de 480 px** (`--col`), então a página é idêntica no
celular e no desktop — no desktop ela aparece centralizada, no formato de celular.

## Arquivos

```
.
├── index.html              ← página inteira (HTML + CSS + JS embutidos)
├── assets/img/             ← hero em 3 tamanhos (jpg + webp)
└── README.md
```

O hero é servido em `hero-800/1400/2000` (`.webp` com `.jpg` de fallback), gerados a
partir do `hero.png` original de 9,6 MB — que fica de fora do repositório via
`.gitignore`, mas continua na sua pasta local. Para regerar depois de trocar o PNG:

```bash
cd assets/img
for w in 800 1400 2000; do
  sips --resampleWidth $w hero.png --setProperty format jpeg \
       --setProperty formatOptions 82 --out hero-$w.jpg
  cwebp -q 80 hero-$w.jpg -o hero-$w.webp
done
```

As 14 fotos originais continuam na pasta do projeto, caso queira trocar o hero.

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

O **Google Tag Manager** (`GTM-PQGGFP2K`) já está instalado: o script no topo do
`<head>` e o `<noscript>` logo depois do `<body>`.

Todo clique em CTA empurra um evento pro `dataLayer`:

```js
{ event: 'join_group', method: 'whatsapp', cta: 'header' | 'hero' | 'barra-fixa' }
```

No GTM, crie um **acionador do tipo Evento personalizado** com o nome `join_group`.
O campo `cta` diz qual botão converteu — use como variável da camada de dados se
quiser separar por posição.

Os cliques também disparam `fbq('track','Lead')` e `gtag('event','join_group')` **se**
esses pixels estiverem na página. Você pode disparar os dois pelo próprio GTM em vez
de colar as tags no HTML.

## Pontos que dependem de você

- **Números do hero** (`+15 anos`, `04 países`, `30`) e a bio dos clubes foram escritos a
  partir das fotos (Internacional, Criciúma, FC Lviv, Metalist, Catar). Confirme com o
  Jonathan antes de publicar.
- **Links do rodapé** (Termos, Privacidade, Jogo responsável) estão como `#`.
- Não há formulário de captura de e-mail/telefone: o botão leva direto pro WhatsApp.
  O modelo tem um modal antes do redirect — dá pra adicionar se quiser gerar lista.
