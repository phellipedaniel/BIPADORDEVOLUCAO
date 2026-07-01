# Planejamento Técnico — Bipador de Devolução (REVERSA CD)

> **Objetivo deste documento:** guiar o desenvolvimento do **Bipador de Devolução** para que ele adote o **mesmo padrão de design do Controle de Estoque · Petlove**, garantindo coerência visual entre as ferramentas do CD. A funcionalidade atual do Bipador é mantida — o foco é a camada de design (tokens, tipografia, componentes, tema).

- **Ferramenta:** Bipagem de volumes da logística reversa (devoluções) nos CDs.
- **Referência de design:** Controle de Estoque · Petlove (Caramelo Design System).
- **Idioma:** Português do Brasil (pt-BR).
- **Stack:** HTML + CSS + JavaScript vanilla, single-file, backend em Google Apps Script (Sheets). Sem build.

---

## 1. Contexto

### 1.1 Estado atual (as-is)
O Bipador hoje funciona, mas usa uma linguagem visual **divergente** do restante das ferramentas: tema escuro "tech" (fundo `#0f1117`, azul `#4f8ef7`), fontes **Inter + JetBrains Mono**, cantos de 10 px e sombras neutras/frias. Não usa o Caramelo.

O Controle de Estoque, por outro lado, já segue o **Caramelo Design System (Petlove)**: roxo da marca, neutros "caramelo" quentes, fontes **Gooper + Inter + Roboto Mono**, cards arredondados, botões pill, tema claro/escuro e o logo Petlove no topo.

### 1.2 Problema
As duas ferramentas convivem na rotina do mesmo operador de CD, mas parecem produtos de empresas diferentes. Isso quebra a confiança, aumenta a curva de aprendizado e foge da identidade Petlove.

### 1.3 Meta
Reescrever a camada de apresentação do Bipador para que um operador que conhece o Controle de Estoque **reconheça imediatamente** o Bipador como parte da mesma família — sem alterar seu comportamento funcional.

---

## 2. Princípio norteador — coerência visual

> **Regra de ouro:** tudo que for visível (cor, tipo, forma, sombra, espaçamento, componente, voz) deve vir do Caramelo e replicar 1:1 o que o Controle de Estoque já usa. Nada de cor, fonte ou componente inventado fora do sistema.

Quando houver dúvida entre "o que o Bipador faz hoje" e "o que o Controle de Estoque faz", **o Controle de Estoque vence** na questão de aparência.

---

## 3. Design tokens (fonte única da verdade)

Substituir integralmente o `:root` atual do Bipador pelos tokens abaixo. São os mesmos valores usados no Controle de Estoque.

### 3.1 Tema claro (padrão)
```css
:root, [data-theme="light"] {
  --bg: #F4ECDD;            /* fundo geral, neutro caramelo quente */
  --panel: #FFFFFF;         /* superfície de cards */
  --panel-2: #FBF7F0;       /* superfície secundária / campos */
  --line: #EBE2D3;          /* hairline de borda */
  --line-soft: #F1EADD;     /* divisórias suaves */
  --ink: #322D25;           /* texto principal */
  --ink-2: #675D4C;         /* texto secundário */
  --ink-3: #93846C;         /* texto terciário / labels */
  --accent: #4E2096;        /* roxo da marca */
  --accent-soft: #F1EAFB;   /* tint do roxo */
  --primary: #4E2096;       /* ação primária */
  --primary-strong: #3A1574;/* hover da ação primária */
  --on-primary: #FFFFFF;
  --c-cancel: #EA534A;      /* vermelho (perigo / excluir / heart) */
  --ok: #49A971;            /* verde sucesso */
  --focus-ring: rgba(78,32,150,.18);
  --chip-bg: rgba(50,45,37,.05);
  --scheme: light;
  --radius: 16px;
  --shadow: 0 1px 2px rgba(60,35,18,.05), 0 12px 32px rgba(60,35,18,.08);
}
```

### 3.2 Tema escuro
```css
[data-theme="dark"] {
  --bg: #1A1426;
  --panel: #241C33;
  --panel-2: #2C2340;
  --line: #392E50;
  --line-soft: #2E2542;
  --ink: #F4EFFA;
  --ink-2: #C2B6D6;
  --ink-3: #897E9E;
  --accent: #B79BE8;
  --accent-soft: #2E2348;
  --primary: #8B5CF6;
  --primary-strong: #7C4DEE;
  --on-primary: #FFFFFF;
  --c-cancel: #F2655C;
  --ok: #4FD1A1;
  --focus-ring: rgba(183,155,232,.22);
  --chip-bg: rgba(255,255,255,.05);
  --scheme: dark;
  --radius: 16px;
  --shadow: 0 1px 0 rgba(255,255,255,.03) inset, 0 12px 40px rgba(0,0,0,.45);
}
```

### 3.3 Cores de feedback (Caramelo)
| Uso | Cor |
|---|---|
| Sucesso | `#49A971` |
| Alerta | `#FFA840` |
| Perigo | `#EA534A` |
| Informação | `#0A60D8` |

> **Sombras são marrom-quente** (`rgba(60,35,18,…)`), nunca cinza/preto neutro — mantém a paleta "caramelo" coesa.

---

## 4. Cores das filiais (harmonizar com o Caramelo)

Hoje o Bipador usa azul/verde/âmbar puros para PETPE/PETGO/PITAJ. Manter a lógica de "cor por filial", mas trocar por tons compatíveis com a paleta, usando sempre o padrão *cor + tint 12–14%*:

| Filial | Local | Cor sugerida | Tint |
|---|---|---|---|
| **PETPE** | Pernambuco | `#4E2096` (roxo marca) | `rgba(78,32,150,.12)` |
| **PETGO** | Goiás | `#49A971` (verde) | `rgba(73,169,113,.14)` |
| **PITAJ** | Itajaí | `#FFA840` (âmbar) | `rgba(255,168,64,.14)` |

Aplicar da mesma forma que o Controle de Estoque aplica cor por tipo de ocorrência (borda esquerda de 3 px no card + tag com `color` + `background` tint).

---

## 5. Tipografia

Trocar **JetBrains Mono → Roboto Mono** e introduzir **Gooper** para títulos.

| Papel | Família | Peso | Onde usar |
|---|---|---|---|
| Display / títulos | **Gooper** (fallback Inter) | 700 | `h1` do topo, títulos de card (`<h2>`), números grandes de KPI/stat |
| Corpo / UI | **Inter** | 400–600 | textos, labels, botões, formulários |
| Mono | **Roboto Mono** | 400–500 | códigos de barras, relógio, horários, valores tabulares |

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;450;500;600;700&family=Roboto+Mono:wght@400;500&display=swap" rel="stylesheet">
```
```css
@font-face { font-family:"Gooper"; src:url("fonts/Gooper-SemiBold.otf") format("opentype"); font-weight:600; }
@font-face { font-family:"Gooper"; src:url("fonts/Gooper-Bold.otf") format("opentype"); font-weight:700; }
@font-face { font-family:"Gooper"; src:url("fonts/Gooper-Black.otf") format("opentype"); font-weight:800 900; }
```
> Copiar os arquivos `Gooper-*.otf` para uma pasta `fonts/` no repositório do Bipador (mesmos arquivos do Controle de Estoque).

**Escala:** 12 / 14 / 16 / 22 / 24 / 32 / 48. Texto mínimo de UI: 12 px.

---

## 6. Forma, espaçamento e movimento

- **Raio:** cards `16 px` (`var(--radius)`); campos/pills menores `9–12 px`; **botões, chips e busca totalmente pill (`999px`)**.
- **Espaçamento:** grade base de 4 px (4 / 8 / 12 / 16 / 24 / 32 / 48).
- **Bordas:** hairline de 1 px (`--line`) em cards e divisórias; foco = borda `--accent` + anel `--focus-ring`.
- **Movimento:** transições suaves de `.12–.2s`; botão encolhe para `scale(.98)` no clique; cards sobem 1 px no hover. Sem bounce/efeitos chamativos.

---

## 7. Componentes (mapear Bipador → padrão Controle de Estoque)

Reconstruir cada elemento do Bipador reaproveitando as classes/estilos do Controle de Estoque:

| Elemento no Bipador | Componente-padrão a usar |
|---|---|
| Topo (`.sidebar` / header) | `.top` + `.brand` com **logo Petlove SVG** + `.brand-divider` + título Gooper + `.clock` com `.pulse` |
| Botões de ação (Exportar, Sair) | `.btn` pill — `.btn-primary` / `.btn-ghost` / `.btn-icon` |
| Alternância de tela (Bipagem / Dashboard) | `.tabs` + `.tab` (borda inferior ativa em `--accent`) |
| Cartões (form, tabela, KPIs) | `.card` + `.card-h` (eyebrow Roboto Mono + `<h2>` Gooper) + `.card-b` |
| Métricas laterais / KPIs | `.stat` / `.stat-sm` (número em Gooper) |
| Filtros de filial/busca | `.filter-bar` + `.filter-pill` + `.search-wrap` |
| Linhas de registro (últimas bipagens / tabela) | `.rec` com borda esquerda colorida por filial + `.tag` + `.planta-chip` + `.rec-time` mono |
| Input de código de barras | `input` Caramelo + classe `.mono` (Roboto Mono) |
| Mensagens de status | `.toast` (`.ok` / `.err`) |
| Modal de exclusão | mesmo padrão de card + confirmação inline dos `.rec-del` / `.btn-clear-all` |
| Telas de Login e Entrada | `.card` centralizado + campos e botões Caramelo |

### 7.1 Logo Petlove
Substituir o ícone genérico atual pelo **SVG do wordmark Petlove** (copiar do Controle de Estoque, com o coração em `#EA534A`) dentro de `.brand > .brand-logo`, seguido de `.brand-divider` e do título "Bipagem de Volumes / REVERSA CD".

### 7.2 Tema claro/escuro
Adotar o mesmo botão de tema (`#btn-theme`, ícones sol/lua) e persistência em `localStorage` (chave `tema-petlove`, compartilhada entre as ferramentas para manter a preferência do usuário). Aplicar via `data-theme` no `<html>`.

---

## 8. Iconografia

- Usar **ícones de linha** (stroke, `currentColor`), 24×24, `stroke-width` 1.6–2 — idênticos ao Controle de Estoque.
- O **coração** é o único símbolo de marca; usar `#EA534A`.
- **Sem emojis** na interface de trabalho (exceto um 🐾 opcional em confirmação de sucesso, com moderação).
- Não desenhar ícones novos à mão fora do padrão; reaproveitar os SVGs já existentes.

---

## 9. Voz e conteúdo (pt-BR)

- Tom acolhedor e direto; "você" informal.
- **Sentence case** em botões, títulos e labels — não Title Case; MAIÚSCULAS só em eyebrows/rótulos minúsculos (ex.: código de filial `PETPE`).
- Botões orientados a ação: *Registrar*, *Exportar CSV*, *Iniciar bipagem*, *Entrar*, *Sair*.
- Números pt-BR: data `dd/mm/aaaa`, hora `HH:MM:SS`.

---

## 10. Escopo funcional (preservar)

Nada abaixo muda de comportamento — apenas de aparência.

**Autenticação e sessão**
- Login por usuário/senha; usuários por filial (`REVERSAPE`→PETPE, `REVERSAGO`→PETGO, `REVERSASC`→PITAJ) e um usuário **mestre** (`REVERSAMESTRE`) com acesso a todas as filiais e ao filtro de filial.
- Auto-login por sessão salva em `localStorage`; ação de **Sair** limpa a sessão.

**Entrada / seleção de filial**
- Usuário normal: filial definida pelo login.
- Mestre: escolhe a filial na tela de entrada.
- Captura de **geolocalização** (endereço detectado) na entrada e por bipe.

**Bipagem**
- Campo de operador + campo de código de barras; cada bipe (ou Enter) registra **+1 volume**.
- Tabela de registros: #, filial, código, operador, data/hora, localização, excluir.
- Filtro por código; filtro por filial (só mestre); busca; contador de registros.
- **Exportar CSV**.

**Dashboard**
- KPIs do período (volumes, filiais ativas, operadores ativos, ritmo/última hora, pico do dia).
- Comparação entre filiais (gráfico + tabela: volumes, % total, vol/h, último bipe).
- Ranking e detalhe por operador.
- Evolução temporal (série por filial) e **heatmap hora × dia da semana**.
- Filtros: período (hoje / 7d / 30d / mês) e filial.

---

## 11. Regras de negócio

| Perfil | Filial | Pode bipar em | Vê registros | Filtro de filial |
|---|---|---|---|---|
| `REVERSAPE` | PETPE | PETPE | próprios da filial | não |
| `REVERSAGO` | PETGO | PETGO | próprios da filial | não |
| `REVERSASC` | PITAJ | PITAJ | próprios da filial | não |
| `REVERSAMESTRE` | — | qualquer filial | todos | sim |

- Geolocalização: solicitar permissão; degradar com elegância se negada (estado de erro no `.geo-box`).
- Cada leitura = 1 volume; não deduplicar códigos (é contagem de volumes, não de SKUs únicos).

---

## 12. Estrutura de dados

**Backend:** Google Apps Script (`APPS_SCRIPT_URL`) gravando em Google Sheets.

Campos por registro de bipagem:
| Campo | Tipo | Observação |
|---|---|---|
| `id` | string | timestamp + sufixo aleatório |
| `iso` | ISO datetime | ordenação/relativo |
| `data` / `hora` | string pt-BR | exibição |
| `filial` | enum | PETPE / PETGO / PITAJ |
| `codigo` | string | código de barras bipado |
| `operador` | string | nome digitado |
| `geo` | string | endereço/coords detectadas |

> Manter o CSV com cabeçalho pt-BR e BOM (`\uFEFF`) para abrir corretamente no Excel — mesmo padrão do Controle de Estoque.

---

## 13. Guia de implementação (passo a passo)

1. **Tokens:** substituir o `:root` do Bipador pelos blocos da seção 3 (claro + escuro).
2. **Fontes:** adicionar `fonts/Gooper-*.otf`, trocar o `<link>` do Google Fonts (Inter + Roboto Mono) e remover JetBrains Mono.
3. **Base:** aplicar `body` com fundo Caramelo (radiais suaves opcionais) e `font-family: "Inter"`.
4. **Topo:** inserir o logo Petlove SVG, divisória e título; adicionar botão de tema e relógio `.clock` com `.pulse`.
5. **Layout:** migrar de grid com sidebar para o container `.wrap` (máx. 1200 px) + `.tabs` (Bipagem / Dashboard) + `.card`/`.grid`, como no Controle de Estoque.
6. **Componentes:** reescrever botões como `.btn` pill, campos como inputs Caramelo, registros como `.rec`, stats como `.stat`, filtros como `.filter-pill`.
7. **Cores de filial:** aplicar a paleta da seção 4.
8. **Dashboard:** re-tematizar os gráficos (Chart.js) usando as variáveis CSS (`getComputedStyle`) para que sigam o tema ativo; cards em `.dash-card`/`.card`.
9. **Mensagens:** unificar toasts no padrão `.toast .ok/.err`.
10. **QA visual:** rodar o checklist da seção 14 lado a lado com o Controle de Estoque, nos dois temas.

---

## 14. Checklist de coerência visual

- [ ] `:root` usa exatamente os tokens Caramelo (claro + escuro).
- [ ] Nenhuma cor hardcoded fora dos tokens/paleta de filial.
- [ ] Gooper nos títulos e números grandes; Inter no corpo; Roboto Mono em códigos/horários.
- [ ] Botões, chips e busca são pill (`999px`); cards têm raio 16 px.
- [ ] Sombras marrom-quente, não cinza/preto.
- [ ] Logo Petlove no topo, com o coração `#EA534A`.
- [ ] Tema claro/escuro funcional e persistido (chave compartilhada).
- [ ] Ícones de linha `currentColor`, sem emojis no chrome.
- [ ] Copy em sentence case, pt-BR, verbos de ação.
- [ ] Estados de foco com borda `--accent` + anel `--focus-ring`.
- [ ] Bipador e Controle de Estoque, abertos lado a lado, parecem o mesmo produto.

---

## 15. Roadmap sugerido

| Fase | Entrega | Escopo |
|---|---|---|
| **F1 — Fundação** | Tokens + fontes + base | Trocar `:root`, importar Gooper/Roboto Mono, `body` Caramelo, tema claro/escuro |
| **F2 — Chrome** | Topo + navegação | Logo Petlove, `.clock`, `.tabs`, botões pill, layout `.wrap` |
| **F3 — Bipagem** | Formulário + tabela | Inputs, `.rec`, filtros, toasts, cores de filial |
| **F4 — Dashboard** | KPIs + gráficos | `.stat`/`.dash-card`, Chart.js tematizado, heatmap |
| **F5 — Polimento** | QA + acessibilidade | Checklist da seção 14, contraste, foco, responsivo |

---

## 16. Riscos e dependências

- **Fontes Gooper:** licenciadas — usar os mesmos arquivos já em uso no Controle de Estoque; não substituir por fonte aberta.
- **Google Apps Script:** o backend precisa continuar ativo; a mudança de design não toca na integração, mas revalidar após o refactor.
- **Geolocalização:** depende de permissão e HTTPS; garantir fallback visual.
- **Chart.js no tema escuro:** cores dos gráficos devem ler as variáveis CSS em runtime para acompanhar a troca de tema.
- **Compatibilidade de dados:** manter chaves de `localStorage` e formato de CSV para não quebrar históricos existentes.

---

*Documento vivo — atualizar conforme o Caramelo evoluir. A referência viva de estilo é o Controle de Estoque · Petlove.*
