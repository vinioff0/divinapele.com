# Divina Pele · Dra. Letícia Sebem — site institucional (demonstração)

> **Projeto de demonstração comercial.** Não é o site oficial da clínica. O conteúdo
> ainda precisa ser validado pela cliente (veja [docs/CONTEUDO-A-CONFIRMAR.md](docs/CONTEUDO-A-CONFIRMAR.md)).
> Em modo demonstração, o site não é indexado por buscadores.

## Stack

- **Next.js 16** (App Router, páginas 100% estáticas) + **React 19** + **TypeScript**
- CSS Modules + tokens em `src/app/globals.css` (sem frameworks de UI)
- `next/font` (Bodoni Moda, Manrope, Pinyon Script) e `next/image` (AVIF/WebP automáticos)
- Nenhuma dependência além de `next`, `react` e `react-dom`

## Estrutura

```
src/
  app/
    layout.tsx          # fontes, metadados, SEO, JSON-LD
    page.tsx            # composição das seções
    globals.css         # tokens de design, botões, animações de revelação
    privacidade/        # aviso de privacidade (provisório)
    robots.ts sitemap.ts icon.svg
  components/           # uma pasta de responsabilidade por seção (+ .module.css)
  content/site.ts       # TODO o conteúdo editável (contato, procedimentos, cursos, vídeos…)
public/
  images/clinica/       # fotos reais da clínica (fornecidas)
  images/brand/         # fotos públicas da Dra. e logotipo do Instituto
  images/stock/         # imagens ilustrativas CC0 (procedimentos)
  videos/               # destino dos vídeos do Higgsfield
docs/
  CONTEUDO-A-CONFIRMAR.md
  HIGGSFIELD.md
```

**Para editar textos, telefone, endereço, horários ou procedimentos, altere apenas
`src/content/site.ts`.** Cada dado traz o status `VERIFICADO`, `A CONFIRMAR` ou `PROVISÓRIO`
e a fonte.

## Rodar localmente

Requer Node.js 20.9 ou superior.

```bash
npm install
npm run dev        # http://localhost:3000
npm run build      # build de produção
npm start          # serve o build
```

## Publicar na Vercel

1. Crie um repositório no GitHub e envie o projeto:
   ```bash
   git init
   git add .
   git commit -m "Site Divina Pele (demonstração)"
   git branch -M main
   git remote add origin https://github.com/<usuario>/<repositorio>.git
   git push -u origin main
   ```
2. Em [vercel.com/new](https://vercel.com/new), importe o repositório. A Vercel detecta o
   Next.js automaticamente; não é preciso configurar build.
3. Em **Settings → Environment Variables**, defina:
   | Variável | Demonstração | Produção (após aprovação) |
   |---|---|---|
   | `NEXT_PUBLIC_SITE_URL` | `https://<projeto>.vercel.app` | `https://draleticiasebem.com` |
   | `NEXT_PUBLIC_DEMO_MODE` | `true` | `false` |
4. Deploy. Com `NEXT_PUBLIC_DEMO_MODE=true`, o site envia `noindex` e o `robots.txt`
   bloqueia buscadores.

Alternativa sem GitHub: `npx vercel` na pasta do projeto.

## Vídeos (Higgsfield)

Os vídeos **ainda não foram gerados**. As três áreas já estão preparadas, e hoje exibem as
fotos como fallback. Para ativar:

1. Gere os vídeos com os prompts de [docs/HIGGSFIELD.md](docs/HIGGSFIELD.md).
2. Otimize (exemplo com ffmpeg, 1920px, sem áudio):
   ```bash
   ffmpeg -i entrada.mp4 -an -vf "scale=1920:-2" -c:v libx264 -crf 24 -preset slow -movflags +faststart public/videos/hero.mp4
   ffmpeg -i entrada.mp4 -an -vf "scale=1920:-2" -c:v libvpx-vp9 -crf 34 -b:v 0 public/videos/hero.webm
   ```
3. Em `src/content/site.ts`, preencha `videos.hero.src = { mp4: "/videos/hero.mp4", webm: "/videos/hero.webm" }`
   (e o mesmo para `transition` e `experience`).

O componente `MediaLoop` já cuida de `autoplay`, `muted`, `loop`, `playsInline`, poster de
fallback, pausa fora da tela, `prefers-reduced-motion` e modo de economia de dados.

## Créditos de imagem

- Fotos da clínica e da Dra. Letícia: material da cliente e do site público draleticiasebem.com.
  É preciso confirmar a autorização de uso.
- Ilustrações de procedimentos: [StockSnap](https://stocksnap.io), licença CC0
  (IDs: L98SVEZOVY, YGQRGVJ3MW, FRDOUEB81I, VTD0ZZ6HOV, RTIJANUFFS).
