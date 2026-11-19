# Introdução passo a passo ao framework Angular

📖 **Leia o tutorial: [https://stahe.github.io/br-angular-etape-par-etape-sept-2026/](https://stahe.github.io/br-angular-etape-par-etape-sept-2026/)**

Este curso ensina como construir uma aplicação web com o framework [Angular](https://angular.dev) 22: uma **aplicação de página única** (SPA), cujas páginas são renderizadas **no navegador** a partir de dados JSON fornecidos por um servidor.

Ele dá continuidade ao curso [Introdução passo a passo ao framework web NestJS](https://stahe.github.io/br-nestjs-html-sept-2026/), oferecendo uma perspectiva diferente, e segue a estrutura da [Introdução passo a passo ao framework Vue.js](https://stahe.github.io/br-vuejs-etape-par-etape-sept-2026/): mesmo servidor, mesmas páginas, escritas no estilo Angular.

| Curso NestJS | Curso Angular |
|---|---|
| o servidor gera as páginas HTML (Handlebars) | o navegador gera as páginas (Angular) |
| o navegador exibe o que recebe | o servidor retorna apenas JSON |
| controllers, views, `res.render` | componentes, roteador, serviços |
| guards `JwtAuthGuard`, `RolesGuard` | guards do roteador (o servidor mantém os seus) |
| dicionários lidos pelo servidor | dicionários no cliente (ngx-translate); o servidor retorna apenas chaves |
| mensagem flash em um cookie | mensagem flash em um serviço baseado em signal |

As páginas, no entanto, permanecem inalteradas: são as mesmas da aplicação **RdvMedecins** já apresentada com os outros frameworks.

## A abordagem: muitos exemplos curtos e, depois, um estudo de caso

Como o Angular é mais rico em recursos que o Vue.js, o curso é estruturado em torno de **25 exemplos curtos**, cada um focado em um único conceito. Eles formam um único workspace Angular: um único comando `npm install`, seguido de `npm start <example>` para executar um deles.

| Capítulo | Conteúdo | Exemplos |
|---|---|---|
| Primeiros passos | um workspace Angular, signals (`signal`, `computed`, `effect`, `linkedSignal`), templates (`@if`, `@for`, `@let`), eventos, `ngModel`, formulários baseados em signals, validação, formulários reativos, pipes | 01–09 |
| Componentes | `input()`, `output()`, `model()`, projeção de conteúdo, ciclo de vida, serviços e injeção de dependência, diretivas, diálogo de confirmação | 10–16 |
| Roteamento | o roteador, parâmetros, query, carregamento tardio (lazy loading), guards de navegação | 17–18 |
| Observables e estado compartilhado | RxJS (`debounceTime`, `switchMap`, `toSignal`), serviços com signals, `localStorage` | 19–20 |
| Internacionalização | ngx-translate: parâmetros, plurais, datas, valores monetários | 21 |
| O servidor: uma caixa-preta | instalação do servidor JSON, sua API, 48 exemplos com `curl` | – |
| Comunicação com o servidor | `HttpClient`, proxy do `ng serve`, cookie `httpOnly`, interceptor, camada de acesso à API, `httpResource`, erros do servidor associados aos campos | 22–25 |

Cada exemplo é apresentado com seu código completo, comentado linha por linha, e uma captura de tela de sua execução.

## O servidor: uma caixa-preta

O servidor é o servidor NestJS da lição anterior, cujos controllers retornam **JSON**, o mesmo usado pelo cliente Vue.js. Esta lição o trata como uma **caixa-preta**: nós o instalamos, estudamos sua API e o consultamos com `curl`, mas não precisamos ler seu código (que é fornecido e comentado para os curiosos).

- Todos os erros têm o mesmo formato: `{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "champs": {...} }` — **chaves** de tradução, nunca o texto propriamente dito;
- autenticação por meio de um token JWT em um cookie `httpOnly` / `sameSite=strict`: o JavaScript do lado do cliente nunca vê o token;
- proteção contra CSRF: cookie `sameSite`, e todas as requisições POST devem ser em JSON;
- Um "modo de teste" para o CAPTCHA, que permite consultar a API com `curl`.


## O estudo de caso: o cliente Angular do RdvMedecins

Uma aplicação completa para **agendamento de consultas em um consultório médico**, com **todos** os arquivos (cerca de 50) listados e comentados.

- **Angular moderno**: componentes standalone, signals, detecção de mudanças sem zone.js, formulários baseados em signals com controles personalizados (`FormValueControl`), páginas carregadas sob demanda.
- **Três papéis**: `ADMIN` (gerencia médicos e clientes), `DOCTOR` (organiza a agenda e cancela consultas), `USER` (o paciente: agenda consultas para si mesmo e gerencia sua conta).
- **Privacidade**: um paciente nunca recebe os nomes dos outros pacientes — o servidor não os envia.
- **Estado completo da página na URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` reabre a janela de agendamento após pressionar F5; os botões Anterior/Próximo funcionam.
- **Validação no servidor**: os formulários exibem, abaixo de cada campo, os erros retornados pela API; bloqueio otimista, nomes duplicados, login já utilizado, etc.
- **Sessão**: restaurada após pressionar F5 (`GET /api/auth/moi`); a expiração é gerenciada em um único lugar (um interceptor HTTP, resposta 401).
- **Francês / inglês**, títulos de abas traduzidos, janela de confirmação acessível pelo teclado, Bootstrap 5.
- **Implantação**: o cliente compilado é servido pelo próprio servidor JSON (mesma origem, sem CORS).

## Tecnologias

Angular 22.2 (componentes standalone, signals, formulários baseados em signals, zoneless) · TypeScript 6.0 · Angular CLI · RxJS 7.8 · ngx-translate 18 · Bootstrap 5.3 · no servidor: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Pré-requisitos

- Noções básicas de JavaScript (ou TypeScript), HTML e do protocolo HTTP.
- Node.js 24 (ou pelo menos 22.22.3), Visual Studio Code com a extensão Angular Language Service e um servidor MySQL (por exemplo, o Laragon no Windows) para o servidor JSON. As instruções de instalação são fornecidas nos apêndices do curso.

## Autor

Este curso, seus exemplos, o servidor JSON e o estudo de caso foram escritos por **Claude**, a IA da [Anthropic](https://www.anthropic.com) (setembro de 2026), a pedido de **Serge Tahé**
