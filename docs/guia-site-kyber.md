# Guia de Sites Kyber Tech

Padrão para todo site entregue pela Kyber. Vale para site estático (HTML/CSS puro, como as prévias) e para projetos em Next.js. Serve como checklist: antes de entregar, tudo aqui precisa estar batido.

> Regra de ouro: nada de inventar. Depoimento, número, cliente ou métrica só se for real e autorizado.

---

## 1. Regras da casa (invioláveis)

- [ ] **Zero travessão** (`—`) em qualquer lugar (texto, título, `alt`, meta, comentário). Use vírgula, dois-pontos, parênteses ou reescreva. Em títulos/separadores use `·` ou `|`.
- [ ] Tudo em **PT-BR**. `<html lang="pt-BR">`.
- [ ] Tom direto e específico, sem buzzword ("premium", "soluções de ponta a ponta", "de verdade").
- [ ] Conteúdo nunca depende de JS para aparecer (o texto principal vem no HTML).

---

## 2. Selo no rodapé (obrigatório em TODO site)

- Texto: **Desenvolvido por Kyber Tech**
- Link: `https://somoskyber.com.br` · abre em **nova aba** · `rel="noopener"`
- Discreto no rodapé, mas presente. É assinatura + backlink para o SEO da Kyber.

**HTML:**
```html
<p class="rodape-credito">
  <a href="https://somoskyber.com.br" target="_blank" rel="noopener">Desenvolvido por Kyber Tech</a>
</p>
```

**Next.js (no `Footer.jsx`):**
```jsx
<a href="https://somoskyber.com.br" target="_blank" rel="noopener noreferrer" className="rodape-credito">
  Desenvolvido por Kyber Tech
</a>
```

---

## 3. SEO (checklist)

- [ ] **1 `<h1>` por página** e hierarquia de headings correta (`h1` > `h2` > `h3`, sem pular).
- [ ] **`<title>` único** por página, ~60 caracteres, com a palavra-chave e a cidade.
- [ ] **`<meta name="description">`** ~150 a 160 caracteres, convidativa.
- [ ] **`<link rel="canonical">`** apontando para a URL final da própria página.
- [ ] **Open Graph** completo: `og:title`, `og:description`, `og:image` (URL **absoluta**), `og:url`, `og:type`, `og:locale`.
- [ ] **`og:image` no tamanho certo**: 1200×630 px, URL absoluta, e conferir que retorna 200 depois de publicado.
- [ ] **Twitter Cards** (opcional, mas fica pronto): `twitter:card` = `summary_large_image`, mais `twitter:title`, `twitter:description`, `twitter:image`. X/Twitter não lê Open Graph.
- [ ] **Favicon + apple-touch-icon + `theme-color`** (aba do navegador, atalho no celular, barra colorida no mobile).
- [ ] **`alt` descritivo** em toda imagem (descreve a cena + contexto, sem encher de palavra-chave).
- [ ] **SEO local**: cidade (e bairro quando fizer sentido) no título, no texto e no schema. Uma página por serviço/bairro quando o volume justificar.
- [ ] **URLs limpas**: minúsculas, com hífen, sem acento (`/portas-de-aluminio-goiania`).
- [ ] **`sitemap.xml`** e **`robots.txt`** publicados.
- [ ] **JSON-LD `LocalBusiness`** com dados reais (ver abaixo).

**Bloco de `<head>` (base para colar e ajustar):**
```html
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Serviço em Cidade | Nome da Empresa</title>
<meta name="description" content="Frase clara do que a empresa faz, para quem e onde. Chamada para orçamento pelo WhatsApp." />
<meta name="theme-color" content="#0f1b3d" />
<link rel="canonical" href="https://dominio-do-cliente.com.br/" />
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />
<meta property="og:type" content="website" />
<meta property="og:locale" content="pt_BR" />
<meta property="og:title" content="Nome da Empresa | Serviço em Cidade" />
<meta property="og:description" content="Mesma ideia da description, focada em vender o clique." />
<meta property="og:image" content="https://dominio-do-cliente.com.br/img/capa.png" />
<meta property="og:url" content="https://dominio-do-cliente.com.br/" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Nome da Empresa | Serviço em Cidade" />
<meta name="twitter:description" content="Mesma ideia da description." />
<meta name="twitter:image" content="https://dominio-do-cliente.com.br/img/capa.png" />
```

**JSON-LD (preencher só com dado real; remover `aggregateRating` se não houver avaliação verdadeira):**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Nome da Empresa",
  "image": "https://dominio-do-cliente.com.br/img/capa.png",
  "description": "O que a empresa faz, para quem e onde.",
  "telephone": "+55-62-90000-0000",
  "url": "https://dominio-do-cliente.com.br/",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Goiânia",
    "addressRegion": "GO",
    "addressCountry": "BR"
  },
  "areaServed": ["Goiânia", "Aparecida de Goiânia"],
  "sameAs": ["https://www.instagram.com/perfil_do_cliente"]
}
</script>
```

---

## 4. Performance (checklist)

- [ ] Imagens em **.webp** otimizadas (~1200 px de largura para capas), com `width`, `height` e `decoding="async"`.
- [ ] **Imagem do hero (LCP) NÃO usa `loading="lazy"`.** Ela carrega eager, com `fetchpriority="high"` (e vale um `<link rel="preload">`). O `loading="lazy"` é só para imagens **abaixo da dobra**. Lazy no hero atrasa o LCP e derruba o Lighthouse.
- [ ] Cuidado com distorção: se o CSS define só `width`, garanta `height: auto` (senão a altura do atributo "vaza" e estica a imagem). O par `width`/`height` também evita salto de layout (CLS).
- [ ] Fontes self-host ou com `preconnect`, e `font-display: swap` (não deixar texto invisível enquanto a fonte carrega). Nada de peso desnecessário.
- [ ] Mobile-first, **sem overflow horizontal** (testar em ~360 px).
- [ ] Alvo no Lighthouse: **90+** em Performance, SEO e Boas Práticas.

**Hero (exemplo):**
```html
<link rel="preload" as="image" href="/img/hero.webp" fetchpriority="high" />
<img src="/img/hero.webp" alt="..." width="1600" height="900" fetchpriority="high" decoding="async" />
```

---

## 5. Conversão e WhatsApp (checklist)

- [ ] CTA claro **acima da dobra** (o que fazer: "Pedir orçamento").
- [ ] Botão de **WhatsApp com mensagem pré-preenchida**.
- [ ] Conferir o **número** (fácil errar um dígito): todos os botões apontam para o mesmo número certo.
- [ ] **Celular sempre com o 9** depois do DDD (`55` + DDD + `9` + número). Erro clássico: número sem o nono dígito.
- [ ] **Clicar no link e testar** antes de entregar: tem que abrir a conversa certa com o texto já preenchido.

**Link do WhatsApp:**
```html
<a href="https://wa.me/5562900000000?text=Ol%C3%A1!%20Vim%20pelo%20site%20e%20quero%20um%20or%C3%A7amento."
   target="_blank" rel="noopener">Pedir orçamento no WhatsApp</a>
```
Formato: `https://wa.me/55DDDNUMERO?text=<mensagem com URL-encode>`.

---

## 6. Acessibilidade (checklist)

- [ ] Contraste de texto suficiente e **foco visível** nos links/botões.
- [ ] Link **"pular para o conteúdo"** no topo (útil para teclado e leitor de tela).
- [ ] Menu mobile com `aria-expanded`, fecha no `Esc` e gerencia foco.
- [ ] Respeitar `prefers-reduced-motion` (desligar animações pesadas).
- [ ] `alt` em imagens e `label` em campos de formulário.

---

## 7. Perfil de Empresa no Google (Google Business Profile)

Para negócio local, o perfil no Google/Maps costuma trazer tanto cliente quanto o site (é o "pacote de mapas" que aparece antes dos resultados normais). Faz parte da entrega ajudar o cliente aqui.

- [ ] Cliente **reivindica ou cria** o Perfil de Empresa no Google.
- [ ] **Categoria principal correta** (e secundárias quando fizer sentido).
- [ ] **NAP consistente**: nome, endereço e telefone **idênticos** no perfil, no site e no JSON-LD. Divergência confunde o Google e derruba o ranqueamento local.
- [ ] **Link do site** e **botão de WhatsApp/telefone** no perfil.
- [ ] **Horário de funcionamento** e **área de atendimento** preenchidos.
- [ ] **Fotos reais** (fachada, equipe, trabalhos) e atualização periódica.
- [ ] **Pedir avaliações** aos clientes satisfeitos (prova social + SEO local).

---

## 8. Analytics e Search Console

- [ ] **GA4** instalado (medir visitas e cliques no WhatsApp/CTA como conversão).
- [ ] Domínio verificado no **Google Search Console** e **sitemap submetido** (acompanhar indexação e buscas).
- [ ] Se usar cookies de analytics, alinhar com a política de privacidade (seção 9).

---

## 9. LGPD e privacidade

- [ ] Se o site tem **formulário** ou **analytics/cookies**, publicar uma **página de política de privacidade** (`/privacidade`).
- [ ] **Aviso de cookies** quando houver rastreamento não essencial.
- [ ] Formulário: deixar claro para que serve o dado e proteger contra spam (ex.: honeypot). Definir para onde a mensagem vai (e-mail do cliente).

---

## 10. Domínio, hospedagem e entrega

- [ ] **Domínio no nome do cliente** (CPF/CNPJ dele), sempre.
- [ ] **HTTPS** ativo.
- [ ] Escolher **um host canônico** (com ou sem `www`) e redirecionar o outro para ele.
- [ ] **Página 404** personalizada (com caminho de volta para a home).
- [ ] Deploy na infraestrutura padrão (ex.: Vercel). **Linux é case-sensitive**: nomes de arquivo e caminhos de imagem têm que bater maiúsculas/minúsculas exatamente.

---

## 11. Fluxo de prévia (antes de ir para o domínio do cliente)

Enquanto o domínio do cliente não está no ar, a prévia fica em `somoskyber.com.br/previa/<slug>`.

- [ ] Publicar a pasta em `public/previa/<slug>/` (index.html + img/).
- [ ] Imagens do corpo em **caminho absoluto** (`/previa/<slug>/img/...`), para carregarem com ou sem barra final.
- [ ] `canonical`, `og:url` e `og:image` apontando para a URL da prévia (não para o domínio do cliente, que ainda não existe).
- [ ] **`<meta name="robots" content="noindex, follow" />`** na prévia: ela abre por link, mas não é indexada sob o somoskyber.
- [ ] Adicionar o card da prévia no índice `public/previa/index.html`.
- [ ] Depois de publicar, colar a URL no **Facebook Sharing Debugger** e dar "Scrape Again", para o WhatsApp puxar a `og:image` nova (senão o primeiro preview vem sem imagem).

**Ao migrar para o domínio do cliente:** trocar `canonical`, `og:url` e `og:image` para o domínio real, **remover o `noindex`**, publicar `sitemap.xml` e `robots.txt`, conferir o selo "Desenvolvido por Kyber Tech" no rodapé, e rodar o Sharing Debugger de novo na URL definitiva.

---

## 12. SEO da Kyber Tech (o retorno de cada site entregue)

Todo site que a Kyber entrega também trabalha pelo SEO da própria Kyber. Como aproveitar sem exagerar (link manipulado o Google pune):

- [ ] **Selo com âncora de marca, não de palavra-chave.** O rodapé usa "Desenvolvido por Kyber Tech" (o nome), nunca algo como "criação de sites em Goiânia". Link de marca em muitos rodapés é natural; a mesma âncora cheia de palavra-chave espalhada por vários sites parece esquema de link e pode penalizar os dois lados.
- [ ] **Um link por site**, para a home (`somoskyber.com.br`), no rodapé. Não repetir em várias páginas com âncoras diferentes.
- [ ] **Adicionar o cliente ao portfólio da Kyber** quando o site estiver no ar no domínio dele: incluir em `/portfolio` com link para o site real. Mostra trabalho real (autoridade/E-E-A-T) e cria uma relação de duas vias.
- [ ] **Pedir avaliação no Google** ao cliente satisfeito, para o perfil da Kyber (prova social + SEO local da Kyber).
- [ ] **Não** copiar textos, `alt` ou JSON-LD da Kyber para o site do cliente. O schema e o conteúdo são do cliente; a Kyber aparece só no selo. Conteúdo duplicado entre domínios atrapalha os dois.
- [ ] Quando fizer sentido e o cliente topar, combinar uma menção espontânea (ex.: um "quem fez nosso site" no story), sempre natural.

---

## 13. Checklist final (antes de entregar)

- [ ] Zero travessão no site inteiro
- [ ] Selo **Desenvolvido por Kyber Tech** no rodapé (link de marca para somoskyber.com.br, nova aba)
- [ ] Cliente entrará no `/portfolio` da Kyber quando o site subir no domínio dele
- [ ] `title`, `description`, `canonical`, Open Graph e JSON-LD preenchidos e com dados reais
- [ ] Favicon, apple-touch-icon e `theme-color` presentes
- [ ] og:image 1200×630 (absoluta) e cache do WhatsApp atualizado no Sharing Debugger
- [ ] 1 `<h1>` por página e headings em ordem
- [ ] `alt` em todas as imagens
- [ ] Hero eager com `fetchpriority="high"`; lazy só abaixo da dobra
- [ ] WhatsApp com número certo (celular com o 9) e mensagem pré-preenchida, link testado
- [ ] Mobile sem overflow horizontal (testado em ~360 px)
- [ ] Imagens `.webp` otimizadas, com `width`/`height`/`decoding` e `loading` onde couber
- [ ] Lighthouse 90+ (Performance, SEO, Boas Práticas)
- [ ] Perfil de Empresa no Google orientado (NAP igual ao do site)
- [ ] GA4 e Search Console configurados (sitemap submetido)
- [ ] Política de privacidade publicada se houver formulário ou analytics
- [ ] HTTPS, host canônico (www ou sem www) e domínio no nome do cliente
