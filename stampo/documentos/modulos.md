# Stampo — Mapeamento de módulos (protótipo front-end)

Fonte: `Itech/stampo/front-end` (React 19 + Vite + TanStack Router/Query/Table + shadcn/ui + Tailwind 4 + Zod + React Hook Form + React Flow).

## Descrição geral do sistema

O Stampo é um sistema de controle de produção para uma confecção/estamparia. O cadastro base define **o que** é produzido (produtos e suas variedades), **por onde** passa (etapas da produção) e **para quem** (clientes). A **Ordem de Serviço (OS)** junta tudo: um cliente pede uma lista de produtos/variedades com quantidades, e cada variedade carrega o seu próprio fluxo de etapas. A OS percorre um ciclo de status controlado (aberta, em andamento, pausada, finalizada, cancelada), com histórico de quem mudou e quando. A **Esteira de produção** mostra, peça a peça, em que etapa cada item da OS está, quem está na fila de cada setor e o progresso geral.

Base técnica já existente no protótipo:

- Camada de dados por service com contrato único de CRUD (`list/get/create/update/remove`), alternando entre `localStorage` (protótipo) e API REST real (`VITE_DATA_SOURCE=api`). Os endpoints esperados já estão definidos.
- Componentes globais padronizados: tabela com ordenação, menu de ações por linha, diálogo de exclusão, formulários reutilizados na criação e na edição.
- Layout com sidebar, tema claro/escuro, telas de login/cadastro/recuperação de senha (visuais, sem autenticação real).

## Módulos de negócio

### 1. Clientes
`/clients` · `src/features/clients` · API `/clients`

Cadastro de clientes que fazem pedidos.
- Listagem em tabela com ordenação, edição e exclusão com confirmação.
- Formulário de criação/edição: nome, tipo de pessoa (física/jurídica) e documento.
- CPF ou CNPJ conforme o tipo, com máscara e validação de dígitos verificadores.

### 2. Etapas
`/stages` · `src/features/stages` · API `/stages`

Cadastro das etapas/setores da produção (ex: Criação, Impressão, Bordado, Acabamento, Embalagem).
- Listagem, criação, edição e exclusão.
- Cadastro simples (nome); as etapas são globais e reutilizadas nos fluxos das variedades.

### 3. Produtos
`/products` · `src/features/products` · API `/products`, `/products/items`

Catálogo de produtos.
- Listagem, criação, edição e exclusão.
- Formulário: nome e itens que compõem o produto (seleção múltipla a partir de `/products/items`).
- Acesso às variedades de cada produto.

### 4. Variedades e fluxo de produção
`/products/$productId/varieties` · `src/features/products/varieties` · API `/products/:id/varieties`

Variações de um produto (ex: tamanho, modelo) e o caminho que cada uma faz na produção.
- Listagem por produto, criação, edição e exclusão; navegação de volta ao produto.
- Formulário: nome e preço (opcional, em reais).
- **Editor visual de fluxo** (React Flow): arrastar etapas cadastradas para um quadro e ligá-las a partir de um nó "Início", definindo a sequência de produção da variedade.
- Validação do fluxo ao salvar: ao menos uma etapa, início conectado, sequência linear (sem ramificação) e nenhuma etapa solta.
- O fluxo é salvo em dois formatos: a sequência de etapas (para uso na produção) e o layout visual (para remontar o editor).

### 5. Ordem de Serviço
`/service-orders` · `src/features/service-orders` · API `/service-orders`

Pedido de produção de um cliente.
- Listagem com status em destaque, criação, edição e exclusão.
- Formulário: nome da OS, cliente e lista de itens.
- Itens adicionados por modal: produto, variedade do produto e quantidade.
- **Controle de status** com transições permitidas: Em aberto → Em andamento ou Cancelada; Em andamento → Pausada ou Finalizada; Pausada → Em andamento. Finalizada e Cancelada são finais.
- **Histórico de status**: cada mudança registra status anterior, novo status, usuário e data, consultável em modal.

### 6. Esteira de produção (teste)
`/production-line` · `src/features/production-line` · sem service (dados fixos)

Prova de conceito do acompanhamento da produção em tempo real.
- Usa uma OS de exemplo com três itens, cada um com o fluxo de etapas da sua variedade.
- Cada unidade vira uma peça que percorre as etapas; cada etapa processa uma peça por vez e as demais aguardam em fila.
- Visual por setor: peça em processamento com progresso e tempo restante, fila de espera e peças concluídas.
- Painel geral: peças concluídas/total, tempo decorrido/total, progresso por item com as etapas já cumpridas.
- Controles de simulação: iniciar, pausar, reiniciar e velocidade (1x, 5x, 20x).
- Hoje é simulação com tempo fixo por etapa; no sistema real dependerá do apontamento nos setores.

## Módulos de apoio (herdados do template, ainda não adaptados)

- **Dashboard** (`/`): tela de exemplo do template (gráficos e "vendas recentes"), sem dados do Stampo.
- **Autenticação** (`/sign-in`, `/sign-up`, `/forgot-password`, `/otp`): telas prontas, sem integração com backend.
- **Configurações** (`/settings`, `/settings/notifications`): perfil e notificações, do template.
- **Usuários, Tarefas, Apps, Chats** (`/users`, `/tasks`, `/apps`, `/chats`): páginas de exemplo do template, fora do menu. Não fazem parte do escopo.

## O que o protótipo ainda não cobre

Itens previstos em `sistema-controle-producao.html` sem tela no protótipo, a considerar no orçamento:

- Backend/API e banco de dados (o front só fala com `localStorage`).
- Autenticação real, usuários e permissões por perfil/setor.
- Fila de produção por setor e apontamento de início/fim de etapa (tablet ou QR code).
- Montagem (junção dos itens que compõem um produto).
- Conclusão da OS com cálculo de tempo real e rastreabilidade por peça.
- Dashboard com indicadores reais de produção.
- Tratamento de esquecimentos e erros de apontamento.
