# Landing Page — Jonathan ZJ

Landing de captação para o grupo gratuito de WhatsApp, com a mesma estrutura do modelo
`gidogreen.com/grupo-gg/` (hero → benefícios → prova social → passos → CTA → FAQ → legal).

## Arquivos

```
.
├── index.html              ← página inteira (HTML + CSS + JS embutidos)
├── assets/img/             ← hero (3 tamanhos, jpg + webp) e a foto de jogo
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

As outras 13 fotos originais continuam na pasta do projeto, caso queira trocar alguma.

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
| Cores | bloco `:root` no `<style>` (`--lime`, `--gold`, `--wa`, `--bg`) |
| Texto de qualquer seção | direto no HTML, tudo em português e comentado por seção |
| Fotos | trocar os arquivos em `assets/img/` mantendo os nomes |
| Bio / clubes / números | seção `<!-- CREDENCIAL -->` |

## Pixel / rastreio

Os botões já disparam `fbq('track','Lead')` e `gtag('event','join_group')` **se** o pixel
estiver na página. Para ativar, cole o script do Meta Pixel / GA4 no `<head>`.
Se for usar o mesmo Pixel & Link Manager (Supabase) dos outros projetos, é só colar
aquele bloco no `<head>` — não conflita com nada aqui.

## Pontos que dependem de você

- **Números do hero** (`+15 anos`, `04 países`, `30`) e a bio dos clubes foram escritos a
  partir das fotos (Internacional, Criciúma, FC Lviv, Metalist, Catar). Confirme com o
  Jonathan antes de publicar.
- **Links do rodapé** (Termos, Privacidade, Jogo responsável) estão como `#`.
- Não há formulário de captura de e-mail/telefone: o botão leva direto pro WhatsApp.
  O modelo tem um modal antes do redirect — dá pra adicionar se quiser gerar lista.
