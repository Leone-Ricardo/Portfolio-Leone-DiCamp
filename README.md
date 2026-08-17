# Leone DiCamp — Portfólio | One Solutions

Site institucional de página única (single-page) para apresentação profissional de Leone DiCamp, desenvolvedor full-stack e gestor de redes sociais atuando sob a marca **One Solutions** em Campo Grande, Zona Oeste do Rio de Janeiro.

Conceito visual: **caderno de esboço renascentista** — tipografia serifada itálica, paleta de pergaminho, ilustração técnica em SVG e microinterações que reforçam a metáfora de "projeto sendo desenhado antes de ser codificado".

---

## Sumário

- [Stack e dependências](#stack-e-dependências)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Como rodar localmente](#como-rodar-localmente)
- [Deploy](#deploy)
- [Seções do site](#seções-do-site)
- [Sistema de design](#sistema-de-design)
- [Efeitos e interações](#efeitos-e-interações)
- [Acessibilidade](#acessibilidade)
- [SEO e metadados](#seo-e-metadados)
- [Checklist antes de publicar](#checklist-antes-de-publicar)
- [Roadmap sugerido](#roadmap-sugerido)
- [Créditos](#créditos)

---

## Stack e dependências

Arquivo único, sem build step, sem framework e sem dependências de pacotes:

| Camada | Tecnologia |
|---|---|
| Markup | HTML5 semântico |
| Estilo | CSS puro (custom properties / variáveis CSS), sem pré-processador |
| Interatividade | JavaScript vanilla (ES5+), sem bibliotecas externas |
| Tipografia | Google Fonts — `Playfair Display`, `Space Grotesk`, `JetBrains Mono` |
| Hospedagem original | [CodePen](https://codepen.io/leone-ricardo/pen/019fd939-222b-7057-9f0f-2dad9a46bf39) |

Não há `package.json`, bundler ou etapa de compilação — o arquivo `index.html` é o site completo e pode ser aberto diretamente no navegador ou hospedado em qualquer servidor estático.

## Estrutura do projeto

```
.
├── index.html      # Site completo (HTML + CSS + JS inline)
└── README.md        # Este arquivo
```

Todo o CSS vive dentro de `<style>` no `<head>` e todo o JS vive dentro de `<script>` antes do fechamento de `</body>`. Essa escolha foi intencional para manter o projeto portável (um único arquivo, fácil de subir em qualquer host estático como Vercel, Netlify, GitHub Pages ou CodePen).

## Como rodar localmente

Não é necessário nenhum passo de instalação. Duas opções:

**Opção 1 — Abrir direto**
```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

**Opção 2 — Servidor local (recomendado para testar fontes/CORS corretamente)**
```bash
# Python
python3 -m http.server 8000

# Node (sem instalar nada globalmente)
npx serve .
```
Depois acesse `http://localhost:8000`.

## Deploy

O arquivo é 100% estático, então qualquer um destes serve:

- **Vercel**: arraste a pasta no dashboard, ou `vercel --prod` via CLI
- **Netlify**: drag-and-drop na área de deploy manual, ou conecte o repositório
- **GitHub Pages**: ative Pages apontando para a branch/pasta que contém o `index.html`

Se o domínio final for `leonedicamp.com.br`, lembre de manter consistentes:
- `<link rel="canonical">`
- as tags `og:url`
- o campo `url` no JSON-LD (`ProfessionalService`)

## Seções do site

| Âncora | Conteúdo |
|---|---|
| `#topo` | Hero — nome, tagline, CTAs principais e ilustração técnica animada |
| `#sobre` | Estatísticas rápidas (experiência, base, marca, foco) + texto de posicionamento |
| `#robustos` | Cards de sistemas em produção (Underground RJ, SaaS multi-tenant, assistente WhatsApp) |
| `#visuais` | "Pranchas" — explorações visuais sem backend, usadas como vitrine de direção estética |
| `#contato` | CTA final com WhatsApp e e-mail |

## Sistema de design

Definido via CSS custom properties em `:root`, facilitando reskin sem tocar no restante do CSS:

```css
--ink:          #17130F   /* texto principal */
--ink-soft:     rgba(23,19,16,0.72)
--ink-faint:    rgba(23,19,16,0.45)
--parchment:    #EFE7D3   /* fundo base */
--parchment-light: #F8F3E4
--gold:         #B9863B   /* acento secundário */
--vermillion:   #9C4432   /* acento primário / CTA de destaque */
--slate:        #2E3B38
--line:         rgba(23,19,16,0.14)
--radius:       4px
```

**Tipografia:**
- `Playfair Display` (itálico) — títulos e nome, tom editorial/renascentista
- `Space Grotesk` — corpo de texto, moderno e neutro
- `JetBrains Mono` — labels, tags, dados técnicos (reforça o lado "dev")

Para trocar a paleta ou tipografia do site inteiro, edite apenas o bloco `:root` e os `<link>` de fontes no `<head>`.

## Efeitos e interações

| Efeito | Onde | Como funciona |
|---|---|---|
| **Intro "caneta escrevendo"** | Carregamento da página | Overlay em tela cheia revela "Leone DiCamp" via `clip-path` animado, com uma ponta (`.intro-nib`) simulando o traço da caneta. Dispara uma única vez por sessão (`sessionStorage`) e é pulado inteiramente se `prefers-reduced-motion: reduce` estiver ativo. |
| **Sketch técnico do hero** | `#topo` | Ilustração SVG com `stroke-dasharray`/`stroke-dashoffset` que se desenha após o intro terminar, sincronizada via JS. |
| **Menu mobile (drawer)** | Header, ≤720px | Botão hambúrguer que vira X, abre um painel full-screen com os links em `Playfair Display` itálico. Fecha com ESC, clique no link, clique novamente ou redimensionamento da janela. Trava o scroll do body enquanto aberto. |
| **Scroll reveal** | Cards e pranchas | `IntersectionObserver` adiciona a classe `.in` com leve stagger (`i * 90ms`) conforme os elementos entram na viewport. |
| **Grão de fundo** | Página toda | `radial-gradient` repetido em baixa opacidade, dá textura de papel sem custo de imagem. |

Todos os efeitos respeitam `@media (prefers-reduced-motion: reduce)`, que desativa transições, animações e o scroll suave.

## Acessibilidade

- `skip-link` funcional para pular direto ao conteúdo (`#conteudo`)
- Contraste de texto verificado sobre o fundo pergaminho
- `:focus-visible` customizado com contorno em `--vermillion`
- Overlay de intro marcado como `aria-hidden="true"` (puramente decorativo, não interfere em leitores de tela)
- Botão do menu mobile com `aria-expanded` e `aria-label` dinâmicos (Abrir/Fechar menu)
- Navegação principal com `aria-label` descritivo

## SEO e metadados

Já incluídos no `<head>`:
- Meta `description` e `keywords` direcionadas para o mercado local (Campo Grande / Rio de Janeiro)
- Open Graph completo (`og:title`, `og:description`, `og:url`, `og:locale`, `og:site_name`)
- Twitter Card (`summary_large_image`)
- **JSON-LD** com `@type: ProfessionalService`, incluindo `founder`, `areaServed` e `description`

**Pendências a preencher quando disponíveis:**
- `sameAs` no JSON-LD está vazio — adicionar URLs de Instagram/LinkedIn do One Solutions ajuda o Google a conectar as entidades
- Nenhuma imagem `og:image` definida — vale gerar uma arte 1200×630px com a identidade do site para preview em redes sociais e WhatsApp

## Checklist antes de publicar

- [ ] Confirmar que `https://leonedicamp.com.br/` (canonical, OG, JSON-LD) é o domínio final
- [ ] Adicionar `og:image` (1200×630px)
- [ ] Preencher `sameAs` no JSON-LD com redes sociais ativas
- [ ] Trocar os placeholders de `#visuais` por imagens reais dos projetos (school demo, cardápio digital, identidade DiCamp)
- [ ] Adicionar analytics (GA4, Plausible ou similar) para medir conversão dos CTAs de WhatsApp
- [ ] Testar o menu mobile e o efeito de intro em pelo menos um device iOS e um Android reais
- [ ] Rodar Lighthouse (Performance / SEO / Acessibilidade / Boas práticas)

## Roadmap sugerido

Ideias já discutidas para próximas iterações, em ordem de custo/benefício:

1. Substituir os blocos de placeholder da seção "Projetos Visuais" por screenshots reais com moldura de navegador/celular
2. CTA individual nos cards de "Projetos Robustos" (ex.: "ver case →")
3. Formulário de contato como alternativa ao WhatsApp
4. Contadores animados nos stats da seção "Sobre"
5. Tilt 3D sutil nos cards ao passar o mouse

## Créditos

- Desenvolvido por **Leone DiCamp** — One Solutions
- Fontes: [Playfair Display](https://fonts.google.com/specimen/Playfair+Display), [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk), [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) via Google Fonts
- Protótipo original: [CodePen](https://codepen.io/leone-ricardo/pen/019fd939-222b-7057-9f0f-2dad9a46bf39)

---

**Contato:** [WhatsApp](https://wa.me/5521967156104) · leoneonix688@gmail.com
