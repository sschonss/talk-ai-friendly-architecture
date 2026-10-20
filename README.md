# AI-Friendly Architecture — slides

**Slides online:** https://sschonss.github.io/talk-ai-friendly-architecture/  
**PDF:** https://sschonss.github.io/talk-ai-friendly-architecture/ai-friendly-architecture.pdf

Deck em [Marp](https://marp.app) baseado na série
[AI-Friendly Architecture](https://luizschons.com/series/ai-friendly-architecture/).

- `slides.md` — o deck (62 slides, em 3 atos). Cada slide tem pouco texto; o roteiro detalhado está nas **notas do apresentador** (`<!-- ... -->`).
- `themes/aifa.css` — tema claro customizado.
- `assets/` — foto e QR codes.

O deploy no GitHub Pages roda a cada push na `main` (`.github/workflows/pages.yml`).

## Comandos

```bash
npm run dev     # servidor com preview ao vivo
npm run build   # dist/index.html (modo apresentador: tecla P)
npm run pdf     # dist/ai-friendly-architecture.pdf (com notas)
npm run pptx    # dist/ai-friendly-architecture.pptx
```
