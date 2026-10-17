---
marp: true
theme: aifa
paginate: true
lang: pt-BR
title: AI-Friendly Architecture
description: Preparando sistemas reais para agentes de IA
author: Luiz Schons
footer: 'AI-Friendly Architecture · luizschons.com'
---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# AI-Friendly Architecture

Preparando sistemas reais para agentes de IA

<br>

**Luiz Schons**

<!--
Apresentação baseada na série de 9 artigos em luizschons.com/series/ai-friendly-architecture.
-->

---

<div class="about">
<div>

# Luiz Schons

<p class="role">Senior Software Engineer @ <b>PicPay</b></p>

<div class="chips">
<span>Guarapuava, Paraná, Brasil</span>
<span>DevParaná</span>
<span>UTFPR</span>
<span>PHP</span>
</div>

<p class="site">luizschons.com</p>

</div>
<img class="photo" src="assets/luiz.jpg" alt="Luiz Schons">
</div>

---

<div class="qr-slide">
<div>

<p class="kicker-dark">Esta palestra é baseada na minha série de artigos</p>

# AI-Friendly Architecture

<p class="muted">9 artigos</p>

<p class="site">luizschons.com/series/ai-friendly-architecture</p>

</div>
<figure class="qr">
<img src="assets/qr-serie.svg" alt="QR code da série">
<figcaption>Escaneie para ler a série</figcaption>
</figure>
</div>

---

<!-- _class: statement -->

> Seu software está pronto para ser **entendido por uma IA**?

<!--
Abrir com a pergunta. Deixar a plateia pensar alguns segundos.
-->

---

<!-- _class: statement -->

> A IA é a **pessoa nova** do time.

<!--
A analogia que guia a palestra inteira.
- Ela não conhece o domínio, os serviços, as decisões passadas nem onde ficam as respostas.
- Com conhecimento bem documentado, encontra respostas sozinha e ganha autonomia.
- Quando depende de uma pessoa específica, de contexto informal ou da memória de alguém, trabalha pior.
- O que ajuda a pessoa nova também ajuda o agente, e vice-versa.
-->

---

## O caminho

<div class="chips accent">
<span>1 · Pronto para IA?</span>
<span>2 · Código ≠ contexto</span>
<span>3 · Observabilidade</span>
<span>4 · Skills</span>
<span>5 · Guardrails</span>
<span>6 · Context Skills</span>
<span>7 · Agentes, tools e MCP</span>
<span>8 · Do épico à produção</span>
<span>9 · Observando agentes</span>
</div>

<!--
Nove partes, uma por artigo da série. Começamos com o problema e terminamos medindo o próprio agente.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 1</div>

# Seu software está pronto?

---

## O problema nem sempre é o modelo

<div class="compare">
<div class="compare-row">
<div class="compare-in"><span>Troca o modelo</span><span>Troca o prompt</span></div>
<span class="compare-arrow">→</span>
<div class="compare-ctx bad">Contexto ruim</div>
<span class="compare-arrow">→</span>
<div class="compare-out bad">Decisão fraca</div>
</div>
<div class="compare-row">
<div class="compare-in"><span>Mesmo modelo</span><span>Mesmo prompt</span></div>
<span class="compare-arrow">→</span>
<div class="compare-ctx good">Contexto bom</div>
<span class="compare-arrow">→</span>
<div class="compare-out good">Decisão assertiva</div>
</div>
</div>

<p class="muted">Muitas vezes, o problema é o <b>sistema</b>.</p>

<!--
Leitura do desenho: na linha de cima, trocamos o modelo e reescrevemos o prompt, mas o contexto continua ruim, e a decisão continua fraca. Na de baixo, mantemos o mesmo modelo e o mesmo prompt, só melhoramos o contexto, e a decisão fica assertiva. O gargalo está no sistema de contexto, não no modelo.

Como chegamos aqui:
1. Aprendemos a escalar sistemas.
2. Aprendemos a modularizar código.
3. Separamos responsabilidades, distribuímos serviços, organizamos times por contexto.
4. Agora: construir software que agentes de IA também entendam, não só pessoas.

A explicação comum para erros de IA é "o modelo não é bom o suficiente". Às vezes é verdade.
Mas sistemas são desenhados para quem JÁ tem contexto. Um dev da casa sabe:
- onde está a documentação relevante;
- quais decisões estão em ADRs antigas;
- qual serviço é dono de cada responsabilidade;
- onde olhar num incidente;
- quais métricas indicam problema real.
O agente não sabe nada disso por padrão. Contexto espalhado, escondido ou inconsistente derruba a qualidade da resposta.
-->

---

## O que o código mostra

<div class="cols">
<div class="card good">

### Mostra
- Implementação
- Estrutura
- O que existe hoje

</div>
<div class="card bad">

### Não mostra
- Por quê
- Trade-offs
- O que foi descartado
- Produção

</div>
</div>

<!--
Armadilha: "a IA lê código, então entende o sistema".
O código não mostra: por que a decisão foi tomada, qual problema de negócio resolve, quais trade-offs foram aceitos, quais partes são sensíveis, o que foi tentado e descartado, como se comporta em produção.
Um sistema maduro vive além do repo: docs, observabilidade, incidentes, tickets, ADRs, dashboards, memória do time.
-->

---

## AI-friendly é contexto…

<div class="chips accent">
<span>acessível</span>
<span>estruturado</span>
<span>confiável</span>
<span>conectado</span>
<span>verificável</span>
</div>

<br>

<p class="muted">…não expor tudo para a IA.</p>

<!--
Definição: organizar sistemas para que agentes consigam DESCOBRIR, CORRELACIONAR e APLICAR contexto com o mínimo de atrito.
Ter a informação não basta: ela precisa ser encontrável e combinável.
-->

---

## Cinco características

<div class="cols5">
<div class="card big">Docs vivas</div>
<div class="card big">Decisões registradas</div>
<div class="card big">Observabilidade</div>
<div class="card big">Contexto por domínio</div>
<div class="card big">Ferramentas conectadas</div>
</div>

<!--
1. Docs vivas: parte do sistema, atualizadas; não um cemitério de páginas.
2. Decisões registradas: ADRs, RFCs e notas guardam o PORQUÊ.
3. Observabilidade como fonte de verdade: logs, traces e métricas explicam o comportamento.
4. Contexto por domínio: cada contexto com docs, regras e referências próprias.
5. Ferramentas conectadas: acesso a observabilidade, backlog, repo, incidentes, deploys.
-->

---

## Incidente: latência na API

<div class="cols">
<div class="card bad">

### Sem contexto
- Logs errados
- Dashboards sem sentido
- Hipóteses genéricas

</div>
<div class="card good">

### Com contexto
- Serviço → docs
- Sinais → decisões
- Hipóteses focadas

</div>
</div>

<!--
SEM arquitetura AI-friendly, o agente depende de sorte: procura logs no lugar errado, lê dashboards sem saber quais métricas importam, perde tempo e sugere hipóteses genéricas.
COM contexto organizado:
1. Identifica o serviço afetado
2. Consulta a doc do domínio
3. Correlaciona sinais de observabilidade
4. Encontra decisões de arquitetura relacionadas
5. Foca nas hipóteses mais prováveis
Não é mágica: o sistema foi preparado para a IA trabalhar com menos atrito.
Onde começar: incidentes, onboarding e mudanças em áreas críticas.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 2</div>

# Código não é contexto

---

<!-- _class: statement -->

> Ter informação **não é** ter contexto.

<!--
Para entender um sistema, a pessoa nova precisa de: código, documentação, decisões de arquitetura, tickets e épicos, incidentes anteriores, dashboards e métricas, conversas com o time.
Na maioria das empresas a informação EXISTE, mas está DESCONECTADA: código num repo, decisões numa wiki, tickets em outro sistema, incidentes na plataforma de operação, e o contexto-chave na cabeça de poucas pessoas.
-->

---

## Context Architecture

<div class="cols">
<div class="card bad">

### Não é
- Um documento gigante
- Tudo num vector DB

</div>
<div class="card good">

### É
- Caminhos entre fontes
- Menos esforço para achar

</div>
</div>

<!--
Definição: uma forma de organizar e CONECTAR as fontes de conhecimento que explicam um sistema.
Perguntas que ela deve responder:
- Pelo que esse serviço é responsável?
- Quem depende dele?
- Por que foi construído assim?
- Qual regra de negócio se aplica?
- Como sabemos que funciona?
- O que aconteceu da última vez que isso mudou?
Não remove o raciocínio: reduz o esforço de achar a informação certa.
-->

---

## Uma rede, não um lugar

<svg class="network" viewBox="-10 0 1100 440" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Rede de fontes de contexto conectadas">
<defs><marker id="arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#3b5bdb"/></marker></defs>
<line x1="500" y1="110" x2="730" y2="40" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="110" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="110" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="730" y2="400" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="40" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="220" x2="730" y2="400" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="40" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="400" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="960" y1="220" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="960" y1="220" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="220" x2="960" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="157" y1="220" x2="199" y2="220" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="340" y1="190" x2="432" y2="144" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="357" y1="220" x2="649" y2="220" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="340" y1="250" x2="432" y2="296" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<rect x="-2" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="80" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Tarefa</text>
<rect x="198" y="192.0" width="164" height="56" rx="14" fill="#3b5bdb" stroke="#3b5bdb" stroke-width="1.5"/>
<text x="280" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="700" fill="#ffffff">Domínio</text>
<rect x="418" y="82.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="500" y="118" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Serviços</text>
<rect x="418" y="302.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="500" y="338" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Docs</text>
<rect x="648" y="12.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="48" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Decisões</text>
<rect x="648" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Código</text>
<rect x="648" y="372.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="408" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Tickets</text>
<rect x="878" y="82.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="118" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Dashboards</text>
<rect x="878" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Incidentes</text>
<rect x="878" y="302.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="338" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Runbooks</text>
</svg>

<!--
Uma página única "Como a empresa funciona" fica desatualizada rápido.
Cada fonte fica onde funciona melhor. O que muda: LINKS CLAROS entre elas.
No diagrama: a tarefa leva ao domínio, e o domínio é a porta de entrada para serviços, código e docs. A partir daí, tudo se referencia: a decisão explica o código, o dashboard aponta para o runbook, o incidente liga o código ao dashboard, o ticket cita a decisão. Nada está centralizado; tudo é alcançável.
"O objetivo não é centralizar conhecimento. É torná-lo fácil de encontrar."
-->

---

## Exemplo: pagamentos

```text
payments-service/
├── README.md     → responsabilidade + Architecture Hub
├── context.md    → conceitos e fronteiras
└── links.md      → onde vive cada coisa
```

<!--
- README.md: explica a responsabilidade do serviço e linka o Architecture Hub.
- context.md: conceitos de negócio e fronteiras do domínio.
- links.md: índice confiável. APONTA, não copia.
Fora do repo: ADRs na ferramenta de arquitetura, runbooks na plataforma de docs, dashboards na ferramenta de observabilidade.
O agente não lê tudo: começa pelo mapa do domínio e segue só os links relevantes para a tarefa.
-->

---

## Todo contexto precisa de dono

<div class="cols4">
<div class="card big">Quem?</div>
<div class="card big">Quando?</div>
<div class="card big">Origem?</div>
<div class="card big">Ainda vale?</div>
</div>

<!--
Documentação sem dono vira documentação abandonada. Decisão sem data nem autor vira regra sem motivo.
- Quem é responsável por esse contexto?
- Quando foi atualizado?
- Qual sistema é a fonte original?
- Como sabemos que ainda é válido?
"Contexto sem sinal de validade pode ser tão perigoso quanto nenhum contexto."
ADRs: o código mostra o resultado; a ADR mostra o porquê (problema, opções, critérios, trade-offs, resultado esperado). Sem isso, o agente propõe algo elegante que ignora uma restrição de negócio ou operação.
-->

---

<!-- _class: statement -->

> Se a resposta é **“pergunta pro fulano”**, o problema é de arquitetura.

<!--
O teste da pessoa nova: pegue uma tarefa real. Alguém recém-chegado consegue descobrir…
- qual parte do sistema precisa mudar?
- quem é dono do domínio?
- quais decisões restringem a solução?
- como verificar que funcionou?
- onde investigar se falhar?
O mesmo teste vale para agentes. Conte os passos de uma tarefa pequena: muitos sistemas abertos ou pessoas consultadas = problema de arquitetura de contexto, não só de documentação.
Contexto segue o fluxo: tarefa → domínios → decisões → docs junto do código → observabilidade pós-deploy → aprendizados na base. Sem burocracia.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 3</div>

# Observabilidade também é contexto

---

## Intenção vs. realidade

<div class="cols">
<div class="card accent big">Código<br><span class="muted small">o que deveria acontecer</span></div>
<div class="card good big">Produção<br><span class="muted small">o que está acontecendo</span></div>
</div>


<!--
Produção revela: config alterada, dependência mais lenta, volume inesperado, regra que falha só com certas combinações de dados.
Monitoramento pergunta "algo está errado?". Observabilidade pergunta "por que o sistema está se comportando assim?".
Cada sinal conta uma parte:
- Logs: eventos e detalhes de execução
- Métricas: tendências, volumes, mudanças
- Traces: o caminho de uma requisição entre serviços
- Eventos de deploy: quando uma mudança chegou em produção
- Dados de negócio: impacto nos usuários
Conectados, permitem formar e testar uma hipótese.
Onde vive: o repositório guarda instrumentação, nomes de sinais e LINKS para dashboards e runbooks. A plataforma guarda o histórico.
-->

---

## Incidente = investigação de contexto

<div class="cols3">
<div class="card big">Quando?</div>
<div class="card big">Onde?</div>
<div class="card big">Quem foi afetado?</div>
<div class="card big">O que mudou?</div>
<div class="card big">Qual dependência?</div>
<div class="card big">Qual impacto?</div>
</div>

<!--
Cenário: a latência da API começa a subir. Quem tem experiência sabe onde olhar. Quem é novo, ou um agente, não.
1. Quando o problema começou?
2. Qual serviço deu o primeiro sinal?
3. Afetou todos os usuários ou um fluxo?
4. Houve deploy ou mudança de config perto?
5. Qual dependência ficou mais lenta?
6. Afetou alguma métrica de negócio?
Nenhum dashboard sozinho responde tudo.
-->

---

## Logs que contam história

```json
{
  "event": "payment_processing_failed",
  "order_id": "ord_8412",
  "error_code": "timeout",
  "request_id": "req_a91f",
  "timestamp": "2026-09-14T13:02:11Z"
}
```

<!--
"Error processing payment" só ajuda quem já conhece o código.
- request_id liga eventos entre serviços
- order_id mostra impacto numa transação
- error_code permite agrupar falhas
- timestamp permite comparar com deploys e mudanças de infra
Métricas também precisam de significado: request_count sem unidade, origem, filtros e o que é "normal" leva o agente a achar a métrica certa e interpretar errado.
Traces ligam o mapa de arquitetura a execuções reais: onde está a latência, qual chamada repete, onde ocorreu o primeiro erro.
-->

---

<!-- _class: statement -->

> `200 OK` não significa que o **negócio** está funcionando.

<!--
Erro comum: olhar só CPU, memória, taxa de erro e latência.
- Uma resposta de sucesso pode esconder uma etapa de negócio falhando.
- Poucos erros podem atingir clientes-chave.
- Uma fila lenta pode destruir a experiência.
Conecte eventos de negócio: pagamentos aprovados, pedidos concluídos, documentos processados, transações com ajuda manual.
Nem todo alerta técnico é incidente de negócio; nem todo problema de negócio aparece como erro técnico.
Depois do deploy: quais métricas observar, qual comportamento é esperado, como o antigo se compara ao novo. O agente ajuda a escrever E a verificar a mudança.
-->

---

## Comece por um fluxo

<ol class="steps">
<li>Um fluxo crítico</li>
<li>Correlation ID</li>
<li>Logs, métricas e traces</li>
<li>O que é <b>normal</b>?</li>
</ol>

<!--
Não precisa instrumentar tudo:
1. Escolha um fluxo importante para o negócio
2. Garanta um correlation ID
3. Estruture os logs principais
4. Crie métricas com nomes e significados claros
5. Verifique se os traces passam por todos os serviços
6. Conecte deploys, incidentes e mudanças de config
7. Documente o que é normal e o que é anomalia
Objetivo: não dashboards bonitos, mas um sistema que conta a própria história.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 4</div>

# Skills

---

<!-- _class: statement -->

> Mais contexto **não é** melhor contexto.

<!--
Reação comum: dar TUDO ao agente de uma vez: docs, padrões, regras, runbooks, dashboards.
O agente precisa descobrir sozinho o que importa, o que é confiável, o que está relacionado e o que ignorar. Resultado: ruído.
É o velho erro de concentrar responsabilidades demais num lugar só.
-->

---

## O que é uma skill?

<div class="flow">
<div>Conhecimento</div><span>+</span>
<div>Workflow</div><span>+</span>
<div>Fontes</div><span>+</span>
<div>Limites</div><span>=</span>
<div class="hl">Skill</div>
</div>

<!--
Uma unidade de conhecimento e fluxo para UM contexto. Pode explicar: conceitos do domínio, quais perguntas fazer primeiro, quais fontes usar, como interpretar dados, limites e regras de segurança, como apresentar resultados.
Ex.: skill de investigação de incidentes: identificar serviço → reconstruir timeline → checar sinais → separar fatos de suposições → registrar evidências.
Ela sabe ONDE buscar e COMO usar, sem guardar cada log.
Exemplos: Datadog, Argo, Databricks, New Relic, incidentes, performance, mudanças em produção.
-->

---

## Os novos microsserviços?

<div class="cols">
<div class="card">

### Era dos microsserviços

<div class="flow">
<div>Monolito</div><span>→</span>
<div class="hl">Serviços por domínio</div>
</div>

</div>
<div class="card accent">

### Era dos agentes

<div class="flow">
<div>Prompt gigante</div><span>→</span>
<div class="hl">Skills por contexto</div>
</div>

</div>
</div>

<br>

<p class="muted">Mesmo objetivo: nenhuma peça com responsabilidade demais.</p>

<!--
A analogia: na época dos microsserviços, quebramos o monolito e separamos os serviços por DOMÍNIO. Agora, na era dos agentes, fazemos o mesmo com o conhecimento: separamos as skills por CONTEXTO.
O intuito é o mesmo: não dar responsabilidade excessiva a um único fluxo, seja um microsserviço, seja uma skill. Cada um com seu vocabulário, regras, dados, contratos e dono.
Tecnicamente, skill não é microsserviço (não é runtime, não tem API, não guarda dados). A semelhança está na decomposição.
Em vez de um agente que sabe um pouco de tudo: um agente com visão geral que carrega o contexto especializado quando precisa.
"Se aprendemos a decompor sistemas por contexto, por que continuamos entregando contexto para a IA em blocos gigantes?"
-->

---

<!-- _class: statement -->

> A skill dá **profundidade**. O agente faz a **coordenação**.

---

## Especializado ≠ isolado

<div class="flow">
<div>Observabilidade</div><span>+</span>
<div>Delivery</div><span>+</span>
<div>Dados</div><span>→</span>
<div class="hl">Hipótese</div>
</div>

<!--
Uma investigação começa em observabilidade, passa por delivery e termina em dados.
- Observabilidade: comportamento anômalo
- Delivery: uma mudança recente
- Dados: um pipeline alterado
Conclusão do agente: "erros subiram após a mudança; o serviço passou a receber dados com outra estrutura; as falhas se concentram em pedidos criados depois do update."
Ainda precisa ser verificada, mas é muito melhor do que uma lista de fatos soltos.
-->

---

## Uma skill bem delimitada

<div class="cols4">
<div class="card big">Contexto</div>
<div class="card big">Objetivo</div>
<div class="card big">Fontes</div>
<div class="card big">Limites</div>
</div>

<!--
Vago: "Investigar problemas em produção". Sem ponto de partida, sem fontes, sem como ranquear hipóteses.
Exemplo bom: investigação de latência no domínio de pedidos.
- Contexto: APIs e serviços de pedidos
- Objetivo: identificar causas prováveis com base em evidências
- Fontes: logs, métricas e traces; catálogo de serviços; deploys recentes; queries read-only; comparação com período anterior
- Workflow: serviço/endpoint → quando começou → latência/erros/volume → traces e dependências → mudanças recentes → dados sem modificar → registrar fatos, causas e lacunas
- Limites: sem alterar dados, sem comandos de escrita, nada sensível no output, escalar se a evidência for insuficiente
Capacidades: search_logs, query_metrics, get_trace, list_recent_changes, query_readonly_database. Cada uma com sua política de acesso.
-->

---

<!-- _class: statement -->

> A skill **guia**. O guardrail **garante**.

<!--
A skill deve dizer: o que é read-only, o que precisa de aprovação, quais dados podem ser expostos, quando a evidência é insuficiente, quando passar para um humano.
MAS limites não podem existir só como instrução. Guardrails fixos checam permissões, argumentos, ambiente e impacto ANTES da ação.
Nunca assuma que o agente vai seguir uma regra de segurança sozinho.
Como começar: pegue um problema recorrente e observe o expert. Quais perguntas vêm primeiro? Quais fontes? Como interpreta? Que hipóteses descarta? Quando escala? Que output é útil?
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 5</div>

# Guardrails e fitness functions

---

<!-- _class: statement -->

> Prompt **orienta**. Não **protege**.

<!--
"Use a API de produção só para leitura." "Nunca acesse endpoints de admin."
Regras no prompt não são barreira: o agente recebe docs novas, combina fontes, lê o código; o texto pode ter instruções conflitantes.
Checagens dentro da skill ou do MCP têm a mesma fraqueza.
MCP conecta agentes a sistemas, mas não é um limite de segurança. A autorização vive no serviço que protege o recurso.
-->

---

## Até leitura é risco

<div class="cols3">
<div class="card big">Recursos alcançáveis</div>
<div class="card big">Dados retornados</div>
<div class="card big">Conexões descobríveis</div>
</div>

<!--
Exemplo: token que só permite GET em produção. Parece seguro.
Mas lendo código, docs e contratos, o agente descobre rotas admin, parâmetros internos, relações entre recursos e caminhos indiretos até dados fora da investigação.
Em vez de "GET sim, POST não", pergunte: quais serviços? quais rotas? quais campos? quais registros? por quanto tempo vale a credencial? como cada acesso é auditado?
-->

---

## Contexto sem abrir produção

<div class="cols3">
<div class="card accent big">Token escopado</div>
<div class="card accent big">Dados derivados</div>
<div class="card accent big">Role mínima</div>
</div>

<!--
1. Token de escopo limitado: principal = agente de investigação; recurso = API de observabilidade de pedidos; operações de leitura específicas; exclui PII e dados de cartão; expira em 15 minutos. O serviço receptor valida o escopo.
2. Dados derivados: eventos, métricas, logs sanitizados vão para réplica/lake/warehouse com retenção, anonimização, filtro de campos e atraso.
3. Banco com menor privilégio: role dedicada; SELECT só em service_health e request_summary; REVOKE em billing e customers. Views escondem colunas sensíveis.
"Não confie no agente para evitar tabelas perigosas. O banco bloqueia."
-->

---

## Preparar ≠ executar

<div class="flow">
<div>Sinais</div><span>→</span>
<div>Agente investiga</div><span>→</span>
<div>PR + change request</div><span>→</span>
<div class="wait">Humano aprova</div><span>→</span>
<div class="ok">Pipeline aplica</div>
</div>

<!--
Guardrail: mecanismo FORA das intenções do agente que valida, limita ou bloqueia. Vive em identidade, rede, API, banco, sistema de arquivos ou aprovação. Não é um "if" no prompt.
Exemplo CPU: pods sem CPU → agente correlaciona e forma hipótese → prepara mudança no repo de infra, abre PR e change request → pessoa responsável revisa e aprova → pipeline aplica.
O agente NÃO tem a credencial de execução.
Outros limites: paths, destinos de rede, tamanho de query, tempo de execução, nº de registros, tipos de dados.
-->

---

## Fitness functions

<p class="muted">Uma regra de arquitetura que vira <b>teste no pipeline</b>.</p>

<br>

<div class="flow">
<div>Orders</div><span>→</span>
<div class="hl">Shared</div><span>←</span>
<div>Billing</div>
</div>

<br>

<div class="card bad"><h3>Proibido</h3>Orders importar código interno de Billing</div>

<!--
Fitness functions vêm da arquitetura evolutiva: transformar uma característica importante da arquitetura em uma checagem OBJETIVA e CONTÍNUA.
Podem ser: teste de arquitetura, regra de pipeline, análise estática, consulta de metadados, checagem periódica.
Não usam IA para decidir: a mesma entrada sempre gera o mesmo resultado.
O objetivo não é julgar se a resposta do agente "parece boa", e sim verificar que a arquitetura continua com as propriedades que o time valoriza.
Exemplo do artigo: dois contextos, Orders e Billing. Regra: Orders não pode importar internos de Billing; os dois conversam por um contrato público (Shared).
-->

---

## A regra em código

```yaml
# deptrac.yaml
deptrac:
  paths: [./src]
  layers:
    - name: Orders
      collectors: [{ type: classLike, value: 'App\\Orders\\.*' }]
    - name: Billing
      collectors: [{ type: classLike, value: 'App\\Billing\\.*' }]
    - name: Shared
      collectors: [{ type: classLike, value: 'App\\Shared\\.*' }]
  ruleset:
    Orders:  [Shared]
    Billing: [Shared]
```

<!--
Deptrac: ferramenta de análise estática para PHP que define camadas e as dependências permitidas entre elas.
- paths: onde analisar (src).
- layers: cada contexto é identificado por um padrão de nome de classe.
- ruleset: Orders só pode depender de Shared; Billing só pode depender de Shared. Qualquer outra dependência entre contextos é violação.
No pipeline: vendor/bin/deptrac analyse --config-file=deptrac.yaml
-->

---

## O agente quebra a regra

<div class="cols wide">
<div>

```php
namespace App\Orders;

use App\Billing\Internal\InvoiceCalculator;

final class RefundOrder
{
    // ...
}
```

</div>
<div>

```text
$ vendor/bin/deptrac analyse

Orders → Billing
  App\Orders\RefundOrder
  depende de
  App\Billing\Internal\...

Violations: 1
✕ pipeline falhou
```

</div>
</div>

<p class="muted">O PR falha, mesmo com o prompt pedindo o contrário.</p>

<!--
Cenário: o agente implementa algo em Orders e, para "resolver rápido", importa uma classe interna de Billing.
O prompt pedia para respeitar a fronteira. Não importa: o Deptrac roda no pipeline e o PR falha.
A saída na tela é resumida/ilustrativa; o ponto é que a violação é detectada de forma determinística.
Deptrac não lê prompt, não interpreta intenção, não julga qualidade de código. Só checa dependências contra as regras que o time definiu.
É por isso que a fitness function fecha a camada de proteção: prompts e skills orientam; ela verifica a regra automaticamente a cada PR.
-->

---

## Camadas de proteção

<div class="flow">
<div>Prompt & skill<br><span class="small muted">orienta</span></div><span>→</span>
<div>Permissões<br><span class="small muted">limita acesso</span></div><span>→</span>
<div>Guardrails<br><span class="small muted">bloqueia ações</span></div><span>→</span>
<div class="hl">Fitness functions<br><span class="small">verifica a cada PR</span></div>
</div>

<!--
AI-friendly não é deixar o agente fazer mais. É dar contexto sem transformar cada fonte em nova superfície de ataque.
Permissões: identidade, menor privilégio, escopo, auditoria, revogação.
Fitness functions: rodam no pipeline e verificam a cada PR que as regras de arquitetura continuam valendo, mesmo meses depois e mesmo com o código mudando.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 6</div>

# Context Skills

---

## O monolito de contexto

<div class="cols middle">
<div class="card big" style="font-family: 'JetBrains Mono', monospace">AGENTS.md</div>
<div class="chips">
<span>arquitetura</span><span>domínios</span><span>padrões</span><span>deploy</span><span>observabilidade</span><span>regras</span><span>runbooks</span><span>testes</span><span>todos os times</span>
</div>
</div>

<!--
Primeiro passo comum: um AGENTS.md central com tudo.
Benefício: tira o conhecimento da cabeça das pessoas.
Problema: cresce rápido. Fica difícil saber o que vale para esta tarefa, quem atualiza cada parte, qual instrução tem prioridade.
A causa não é o arquivo central: é misturar o conhecimento de todos os domínios e ferramentas sem fronteiras.
Lição dos microsserviços: bounded context = vocabulário, regras, responsabilidades, contratos, dados e dono. "Transação" significa uma coisa em pagamentos e outra em pedidos.
-->

---

## Um agente, vários contextos

<div class="cols3">
<div class="card big">Pagamentos</div>
<div class="card big">Delivery</div>
<div class="card big">Observabilidade</div>
</div>

<br>

<p class="muted">Microsserviços separam <b>software</b>. Context Skills separam <b>conhecimento</b>.</p>

<!--
- Pagamentos: estados de transação, políticas de reembolso, integração com provedor.
- Delivery: fluxo de deploy, ambientes, aprovações.
- Observabilidade: investigações com logs, métricas e traces.
Skill NÃO é serviço de produção, não tem API de negócio, não é fronteira de runtime, não armazena dados. Os dados ficam nos sistemas donos.
Documentação explica um assunto; a Context Skill explica como TRABALHAR com ele. É um contrato de trabalho.
Exemplo do agente conectando: falhas de pagamento subindo → Payments → Observability → Delivery → Data Platform → hipótese → reportar o que é confirmado e o que é incerto.
-->

---

## O que uma Context Skill sabe

<div class="cols5">
<div class="card big">Vocabulário</div>
<div class="card big">Responsabilidades</div>
<div class="card big">Contratos</div>
<div class="card big">Fontes de verdade</div>
<div class="card big">Limites</div>
</div>

<!--
1. Vocabulário: transação, autorização, captura, reembolso.
2. Responsabilidades: qual serviço faz o quê, quem é dono, quem decide.
3. Contratos: APIs, eventos e schemas; o que exige versionamento.
4. Fontes de verdade: onde estão dados atuais, docs, sinais de produção.
5. Limites: read-only, o que precisa de aprovação, quando parar e pedir ajuda.
Ex.: numa investigação de reembolso, distinguir transação AUTORIZADA de CAPTURADA antes de qualquer hipótese.
Os limites precisam estar alinhados com a segurança: a skill guia, mas não é a única proteção.
-->

---

<!-- _class: statement -->

> 10 skills para uma pergunta simples? **Fronteira errada.**

<!--
Decompor não é criar centenas de skills minúsculas.
Uma boa fronteira agrupa o que: MUDA junto, é USADO junto, tem DONO relacionado.
Teste: se esse contexto muda, quem precisa revisar? Se a resposta envolve times muito diferentes, a fronteira está grande demais.
Comece grande: Observabilidade, Delivery, Dados, Pagamentos, Pedidos, Segurança. Divida (ex.: incidentes, performance, capacidade) só quando houver necessidade real.
Fronteiras observáveis: registre qual contexto, quais fontes e quais regras guiaram cada resposta.
-->

---

## Se todo mundo é dono, ninguém é

```yaml
name: payments-context
owner: payments-team
review_frequency: quarterly
sources:
  - payment-service
  - payment-documentation
  - payment-dashboards
```

<!--
Se ninguém atualiza a skill quando a API muda, o agente trabalha com conhecimento velho.
O arquivo não resolve a manutenção sozinho, mas torna a responsabilidade visível.
Onde vive: com o código do time, num repo de plataforma ou num catálogo. Deixe claro: dono, quais agentes usam, fontes externas, versionamento, revisão e permissões.
Observabilidade → time de plataforma, sem cópias de dados de produção. Pagamentos → time do domínio, apontando para ADRs, contratos e runbooks.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 7</div>

# Agentes, skills, tools e MCP

---

## O loop do agente

<div class="flow">
<div>Objetivo</div><span>→</span>
<div>Contexto</div><span>→</span>
<div>Decide</div><span>→</span>
<div>Usa tools</div><span>→</span>
<div>Verifica</div><span>↺</span>
</div>

<!--
Agente: recebe um objetivo, checa o contexto, decide os passos e usa capacidades externas.
Sai do loop quando: atingiu o objetivo, falta informação, ou uma pessoa precisa decidir.
Criar um agente não é só escolher um modelo. Um bom agente responde:
1. Que problema resolve? 2. Quando usar? 3. Que contextos conhece? 4. Que ações pode executar? 5. Quais exigem aprovação? 6. Como apresenta resultados?
Exemplo OpenCode: .opencode/agents/ com mode primary, edit negado, skill incident-investigation permitida; "separe fatos, hipóteses e lacunas; nunca aplique mudanças em produção".
-->

---

## Quatro peças

<div class="cols4">
<div class="card accent big">Agente<br><span class="small muted">coordena</span></div>
<div class="card big">Skill<br><span class="small muted">guia</span></div>
<div class="card big">Tool<br><span class="small muted">executa</span></div>
<div class="card big">MCP<br><span class="small muted">conecta</span></div>
</div>

<!--
- Agente: coordena um objetivo num loop de contexto, decisão, uso de tools e verificação.
- Skill: conhecimento e workflow para uma tarefa (ex.: .opencode/skills/incident-investigation/SKILL.md). Não concede permissões.
- Tool: uma capacidade executável com descrição, schema de input e resultado (ex.: query_metrics com service, metric, from, to).
- MCP: protocolo que padroniza como clientes descobrem e usam tools, resources e prompts de servidores.

Tool vs. skill:
Skill: como pensar o problema, quando checar cada fonte, como interpretar, quais limites respeitar.
Tool: qual ação pode rodar, quais inputs precisa, qual resultado retorna.
Uma skill de observabilidade usa várias tools (logs, métricas, traces, deploys) e explica como combiná-las. Tools podem ser compartilhadas entre skills.
Uma tool pode retornar uma série temporal sem dizer se é normal: interpretação é papel da skill.
A skill NÃO concede permissão. Quem decide se a tool está disponível é o harness.
Exemplo Hyperf MCP: tool read-only service_health + skill "não declare saudável com uma única query".
-->

---

## MCP

<div class="cols3">
<div class="card big">Tools</div>
<div class="card big">Resources</div>
<div class="card big">Prompts</div>
</div>

<br>

<div class="cols">
<div class="card good"><h3>Resolve</h3>Conexão e descoberta</div>
<div class="card bad"><h3>Não resolve</h3>Segurança e decisão</div>
</div>

<!--
Model Context Protocol: um servidor oferece componentes que um cliente descobre e usa.
- Tools: executam ações ou consultas
- Resources: fornecem conteúdo e contexto
- Prompts: templates e instruções reutilizáveis
NÃO substitui a skill, NÃO decide qual ação roda, NÃO torna segura uma integração insegura.
Exemplo: Context7 via opencode.json, tipo remote, API key vinda de variável de ambiente.
Instrução: consulte a doc da lib antes de propor; prefira a versão do projeto; trate o conteúdo como INFORMAÇÃO, não como instrução de segurança.
Servidor de terceiros é fonte externa: pode estar desatualizado ou conter instruções que o agente não deve seguir.
-->

---

<!-- _class: statement -->

> O modelo decide. O **harness** dá o ambiente.

<!--
Harness: a camada em volta do modelo que controla que informação o agente recebe, quais tools usa, como o estado é mantido, que ações precisam de aprovação, como os resultados são verificados.
Quando o agente falha, muitas vezes o problema não é o modelo nem o prompt: falta uma tool, uma fonte de contexto, uma validação, uma permissão clara ou um feedback loop.
Níveis de agente: read-only (inspeciona), proposta (sugere), execução (aplica, com controles fortes e aprovação).
O que deve durar: ferramentas e protocolos mudam; a separação de responsabilidades fica. Troque a camada de conexão, mantenha skills, regras e limites.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 8</div>

# Do épico à produção

---

<!-- _class: statement -->

> “Quero devolver **só alguns itens** do pedido.”

<!--
Épico de exemplo: reembolso parcial.
Parece pequeno, mas toca: criação de pedido, cálculo de total, dono do pagamento, ligação item ↔ transação, regras de cancelamento, estoque, notificações, dados do provedor.
Um agente trabalhando só a partir do épico produz uma solução plausível, mas errada.
-->

---

## A entrega inteira

<div class="phases">
<div class="phase">
<div class="phase-title">Entender</div>
<div class="phase-step">Questionar</div>
<div class="phase-step">Spike</div>
<div class="phase-step">Decisão</div>
</div>
<div class="phase">
<div class="phase-title">Planejar</div>
<div class="phase-step">Sucesso</div>
<div class="phase-step">Tarefas e testes</div>
<div class="phase-step">Sinais</div>
</div>
<div class="phase hl">
<div class="phase-title">Construir</div>
<div class="phase-step">Código</div>
</div>
<div class="phase">
<div class="phase-title">Operar</div>
<div class="phase-step">Deploy</div>
<div class="phase-step">Monitorar</div>
<div class="phase-step">Docs</div>
</div>
</div>

<p class="muted">O código é uma etapa, não a entrega.</p>

<!--
O agente participa do ciclo inteiro, não só de escrever código.
- Entender: questionar o épico, fazer o spike, registrar a decisão.
- Planejar: definir sucesso, quebrar em tarefas, definir testes, decidir sinais e alertas ANTES do código.
- Construir: o código. Repare: é uma coluna de quatro, e a menor.
- Operar: deploy com aprovação humana, monitorar contra o baseline, atualizar a documentação com o que foi aprendido.
Entender o problema, decisões, critérios de sucesso, ambientes e sinais podem exigir mais raciocínio do que codar.
-->

---

<!-- _class: statement -->

> O primeiro trabalho do agente é **perguntar**.

<!--
Questionar o épico, não criar arquivos:
- Quem pode pedir reembolso? Quais itens são elegíveis? Há prazo?
- Vários reembolsos por pedido? E se houver um em andamento?
- De onde vem o valor? E se o provedor aceita e nós falhamos?
- Quando o estoque é liberado? Como o cliente é notificado?
Questionar não bloqueia: transforma ambiguidade em decisões explícitas, cedo. Perguntas abertas vão para PO e times. Uma skill de planning guia isso.
-->

---

## Spike → decisão registrada

<div class="cols">
<div class="card">

### Spike
Evidência, não resposta polida

</div>
<div class="card accent">

### Decisão
Opções · Escolha · Riscos

</div>
</div>

<!--
Spike guiado por Context Skills (pedidos, pagamentos, observabilidade): modelo/contrato do pedido → fluxo de cancelamento → integração com provedor → testes de reembolso → eventos publicados → consumidores → sinais de produção. Reconstruir o fluxo, não só achar nomes de arquivo.
Decisão registrada (exemplo):
- Contexto: fluxo atual só reembolsa o valor total.
- Opções: novo tipo de transação; vários reembolsos na transação existente; serviço separado.
- Decisão: reutilizar a transação e rastrear o total reembolsado.
- Consequências: provedor suporta múltiplos reembolsos; eventos identificam itens; idempotência; consumidores antigos seguem funcionando.
- Riscos: concorrência; estado externo vs. interno divergindo; consumidores sem o novo formato.
-->

---

<!-- _class: statement -->

> Código existir **não é** problema resolvido.

<!--
Critérios de sucesso: itens elegíveis selecionáveis; reembolso nunca excede o pago; sem duplicidade; dados corretos no provedor; estoque atualizado; notificação clara; consumidores antigos ok; erro e latência estáveis; menos tickets; operação investigável depois.
Cuidado: menos tickets só é sucesso se o erro ficar estável. Senão o problema só ficou escondido do suporte.
Tarefas: endpoint, validação, provedor, idempotência, versionamento de eventos, consumidores, testes, QA, métricas, dashboards, runbook. O time confirma a divisão.
Testes: "o que precisa estar configurado em QA para confiarmos?" Se a resposta é "nada", o plano é fraco.
-->

---

## Observabilidade antes do código

<div class="cols">
<div class="card">

### Sinais
Sucesso · falhas · divergência

</div>
<div class="card warn">

### Dashboard sem pergunta
= decoração

</div>
</div>

<!--
Para cada feature: logs, métricas, traces, queries, sinais de sucesso, de degradação e gatilhos de rollback.
Reembolso parcial: reembolsos iniciados e confirmados, falhas do provedor, valor total, tempo até confirmação, divergência entre estado externo e interno, retries, falhas por provedor/meio de pagamento.
O agente ajuda a escrever queries e dashboards; escolher o que observar é decisão de engenharia.
Cada painel responde: está sendo usado? funciona? está mais lento? afeta outros contextos? devemos parar o rollout?
Deploy: agente prepara PR, change record, plano e riscos; humanos aprovam conforme a política; monitoramento compara com o baseline.
-->

---

## O ciclo de aprendizado

<ol class="steps">
<li>Épico → <b>perguntas</b></li>
<li>Spike → <b>decisão</b></li>
<li>Operação → <b>evidências</b></li>
<li>Docs → <b>mais contexto</b> na próxima</li>
</ol>

<!--
1. O épico gera perguntas. 2. O spike gera uma decisão. 3. A implementação gera novo comportamento. 4. A operação gera evidências. 5. A documentação registra o que foi aprendido. 6. A próxima demanda começa com mais contexto.
Skills por etapa: Planning, Architecture, Testing, Observability, Delivery. Tools executam ações concretas; MCP conecta aos sistemas.
Humanos continuam essenciais: confirmar regras, avaliar trade-offs, aprovar contratos, aceitar riscos, aprovar produção, ser donos das decisões.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 9</div>

# Observando o próprio agente

---

<!-- _class: statement -->

> Sem sinais, toda avaliação é **impressão**.

<!--
Avaliar só pelo resultado (PR criado? testes passando?) não explica o caminho:
- Quanto tempo para achar o contexto certo?
- Quantas investigações repetidas?
- Quais tools foram usadas?
- Quando precisou de aprovação?
- A causa foi o modelo, a doc, a integração ou o fluxo?
Uma sessão de agente é um fluxo distribuído: modelo, contexto, tools, permissões, sistemas externos, arquivos, testes, decisões humanas.
-->

---

## O que medir

<div class="chips">
<span>sessões</span><span>duração</span><span>tempo em tools</span><span>tokens e custo</span><span>erros e retries</span><span>aprovações</span><span>testes</span><span>resultado</span>
</div>

<!--
Não é gravar tudo, vigiar devs ou tratar tokens como produtividade. É evidência para decisões de engenharia.
Perguntas:
- A doc nova reduziu o tempo até a primeira hipótese útil?
- A skill de investigação reduziu chamadas de tool?
- Um MCP externo adiciona latência sem melhorar o resultado?
- A aprovação vem cedo ou tarde demais?
- Uma mudança de contexto aumentou custo por informação útil ou por ruído?
-->

---

## OpenTelemetry como camada comum

<div class="flow">
<div>Ferramenta de IA</div><span>→</span>
<div>OTLP</div><span>→</span>
<div class="hl">Collector</div><span>→</span>
<div>Prometheus</div><span>→</span>
<div>Grafana</div>
</div>

<!--
Padrão aberto, open source, sem plataforma proprietária para começar.
OpenCode é o exemplo (plugin exportando via OTLP), mas vale para Claude Code, Codex, Cursor ou qualquer ferramenta com exporter, plugin ou hook.
Use conceitos estáveis no backend: session, model call, tool, approval, error, result.
Separar quem PRODUZ eventos de quem OBSERVA: troque a ferramenta sem reconstruir a observabilidade.
-->

---

## Demo: observabilidade de agentes

<div class="cols">
<figure class="qr-card">
<img src="assets/qr-demo.svg" alt="QR code da demo no GitHub">
<figcaption><b>Demo no GitHub</b><span>github.com/sschonss/</span><span>agent-observability-with-opentelemetry</span></figcaption>
</figure>
<figure class="qr-card">
<img src="assets/qr-artigo.svg" alt="QR code do artigo">
<figcaption><b>Artigo</b><span>luizschons.com/posts/</span><span>agent-observability-with-opentelemetry/</span></figcaption>
</figure>
</div>

<!--
Lab local: OpenTelemetry Collector + Prometheus + Grafana via docker compose, com um dashboard inicial de sessões de agente.
OpenCode é o exemplo de instrumentação (plugin @devtheops/opencode-plugin-otel); o backend aceita outras ferramentas.
Passos: docker compose up -d → source examples/opencode-environment.sh → opencode.
Logs e traces começam desligados porque podem conter prompts, código e argumentos sensíveis.
-->

---

## Atividade → resultado

<div class="cols">
<div class="card">

### Atividade
Tokens · tool calls · duração

</div>
<div class="card good">

### Resultado
Menos retrabalho? Melhores evidências?

</div>
</div>

<!--
Armadilha: observar só atividade. Mais mensagens ≠ mais progresso. Mais tokens = contexto melhor OU agente perdido. Mais tool calls = investigação cuidadosa OU falta de docs.
- Tokens → tarefa concluída com menos retrabalho?
- Tool calls → melhores evidências?
- Duração → mais qualidade ou só espera?
- Aprovações → limites de autonomia adequados?
- Linhas alteradas → testes e critérios atendidos?
Senão, a observabilidade vira placar de volume.
-->

---

## O contexto aparece nas métricas

<div class="cols">
<div class="card"><h3>Buscas longas</h3><span class="muted">contexto difícil de navegar</span></div>
<div class="card"><h3>Consultas repetidas</h3><span class="muted">falta uma skill</span></div>
<div class="card"><h3>Chamadas lentas</h3><span class="muted">integração ruim</span></div>
<div class="card"><h3>Aprovação a cada passo</h3><span class="muted">permissões ou fluxo</span></div>
</div>

<!--
Contexto não é só algo que damos ao agente: dá para AVALIÁ-LO pelo comportamento observado.
A telemetria fecha o ciclo entre contexto, execução e aprendizado.
-->

---

## Telemetria também é dado sensível

<div class="chips">
<span>sem prompts em métricas</span><span>sanitizar argumentos</span><span>paths não viram labels</span><span>retenção antes da coleta</span><span>cuidado com session_id</span>
</div>

<!--
Telemetria carrega regras internas em prompts, tokens em argumentos de tools, dados de clientes em resultados, código em traces, estrutura revelada por paths.
Aplicação determinística: Collector, gateway de telemetria, políticas de acesso. Credenciais fora da config versionada; menor privilégio nos destinos.
session_id: ótimo no lab para investigar uma sessão; em produção cria uma série por sessão (cardinalidade cara). Prefira métricas agregadas e traces/logs sanitizados num backend próprio.
Lab: github.com/sschonss/agent-observability-with-opentelemetry. Collector + Prometheus + Grafana com docker compose; logs e traces desligados no início. Comece pequeno e expanda.
-->

---

<!-- _class: divider -->

<div class="kicker">Fechamento</div>

# Por onde começar

---

## Segunda-feira

<ol class="steps">
<li>Um fluxo crítico</li>
<li>O teste da pessoa nova</li>
<li>Uma skill a partir de um expert</li>
<li>Limites fora do prompt</li>
<li>Medir e iterar</li>
</ol>

<!--
1. Escolha um fluxo crítico: incidentes, onboarding ou área sensível.
2. Faça o teste da pessoa nova com uma tarefa real. Crie README, context.md e links.md do domínio. Registre ADRs.
3. Instrumente o fluxo com correlation ID. Escreva uma skill observando como um expert resolve o problema.
4. Dê ao agente credenciais escopadas e coloque uma fitness function no CI.
5. Meça as sessões do agente e itere com evidência.
-->

---

<!-- _class: statement -->

> Contexto. Limites. Evidências. **Feedback.**

<!--
Agentes melhores dependem menos de adivinhar e mais de contexto, limites, evidências e feedback.
Se a arquitetura ajuda pessoas a tomar decisões melhores, ela também ajuda agentes.
-->

---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# Obrigado!

luizschons.com/series/ai-friendly-architecture

<br>

**Luiz Schons**
