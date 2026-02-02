# unnu - Documentação Técnica Unificada e Especificação de Produto

**Versão do Documento:** 1.0.0 (Consolidada)  
**Status:** Definição Completa de Arquitetura e Produto  
**Plataforma:** Web Mobile-First (PWA) / Single Page Application

---

## 1. Visão Geral do Sistema Completo

### 1.1. Objetivo Global

O **unnu** é um "Super App" SaaS mobile-first projetado para conectar "Pequenos Gigantes" (organizadores independentes, mentores e criadores de conteúdo) a profissionais em busca de conhecimento e conexões.

A plataforma unifica duas frentes em uma única aplicação:

1. **Gestão Profissional (SaaS):** Democratiza ferramentas de bilhetagem, gestão financeira e controle de acesso para organizadores que hoje dependem de soluções caseiras.
    
2. **Experiência e Networking (Social):** Oferece aos participantes uma jornada fluida de descoberta via IA e um sistema de matchmaking profissional gamificado.
    

### 1.2. Problema Solucionado

Resolve a fricção existente no mercado de eventos de médio porte:

- **Para o Organizador:** Elimina a complexidade de plataformas enterprise e o amadorismo de planilhas/Pix manuais, oferecendo um fluxo guiado e financeiro integrado.
    
- **Para o Participante:** Elimina a dificuldade de encontrar eventos relevantes e resolve a ineficiência do networking tradicional ("troca de cartões"), substituindo-o por conexões inteligentes baseadas em interesses mútuos.
    

### 1.3. Conceito Central

**"Conectando Experiências"**. O sistema opera sob um modelo de **App Único Híbrido**. O mesmo código e URL atendem a ambos os perfis, com uma alternância de contexto imediata (Toggle Switch).

---

## 2. Papéis e Perfis de Usuário

O sistema baseia-se em uma identidade única. Um usuário pode ser apenas Participante ou acumular a função de Organizador.

### 2.1. Visitante (Guest)

- **Descrição:** Usuário não autenticado.
    
- **Permissões:** Visualizar lista pública de eventos, usar busca (limitada), ver detalhes de eventos e perfis públicos de organizadores.
    
- **Bloqueios:** Ao tentar comprar, favoritar ou acessar networking, é interceptado pelo componente AuthRequiredBlocker.
    

### 2.2. Participante (Attendee)

- **Descrição:** Usuário logado focado em consumo.
    
- **Permissões:**
    
    - Comprar ingressos e acessar Carteira Digital (QR Codes).
        
    - Usar o **unnu smart** (Busca via IA) com créditos renováveis.
        
    - Acessar materiais pós-evento.
        
    - **Networking:** Criar perfil temporário por evento, usar o sistema de "Match" (Swipe) e Chat.
        
    - Favoritar eventos e gerenciar dados de pagamento.
        

### 2.3. Organizador (Organizer)

- **Descrição:** Usuário com perfil "Business" ativado.
    
- **Permissões:**
    
    - Todas as permissões de Participante (via troca de perfil).
        
    - **Gestão de Eventos:** Criar (Wizard Guiado), Editar, Cancelar, Publicar.
        
    - **Financeiro:** Dashboard de vendas, saldo, antecipação (simulação Asaas), gestão de contas bancárias.
        
    - **Operacional:** Check-in via Scanner (Câmera), Gestão de Lotes/Cupons, Envio de Push Notifications.
        
    - **Equipe:** Gerenciar permissões de staff (ADM, Gestor, Validador).
        

---

## 3. Arquitetura Geral do Sistema

### 3.1. Stack Tecnológico

- **Frontend:** React-native.
    
- **Linguagem:** TypeScript.
    
- **Estilização:** Tailwind CSS (Utility-first).
    
- **Ícones:** Lucide React.
    
- **Gráficos:** Recharts.
    
- **Gerenciamento de Estado:** React useState / Context API (sem Redux para manter leveza).
    

### 3.2. Padrões Arquiteturais

- **Mobile-First Container:** A aplicação é encapsulada em um container centralizado (max-w-md), simulando a viewport de um celular mesmo quando acessada via desktop.
    
- **View-Based State Routing:** Não utiliza roteamento de URL tradicional (como react-router) para navegação interna de abas. Utiliza uma variável de estado view (ViewState) no componente raiz App.tsx para renderizar condicionalmente as telas, simulando a performance de um app nativo.
    
- **Mock Data Service:** Camada de serviço (services/mockData.ts) que centraliza todas as operações de dados, permitindo fácil substituição por API real no futuro.
    

### 3.3. Estrutura de Diretórios Unificada

codeText

```
/
├── App.tsx             # Roteador de Estado & Controller Global
├── types.ts            # Definições de Tipos (Unificado)
├── services/
│   └── mockData.ts     # Dados: Eventos, Users, Tickets, Invoices
├── components/
│   ├── Common.tsx      # UI Kit: Buttons, Inputs, Cards, Badges
│   ├── Layout.tsx      # Wrapper com BottomNavigation condicional
│   └── Modals/         # AuthBlocker, ScannerResult, Payment
└── views/
    ├── auth/           # Login, Register, Splash
    ├── attendee/       # Home (AI), Wallet, Networking, Details
    └── organizer/      # Dashboard, Wizard, Scanner, Finance
```

---

## 4. Experiência Geral do Usuário

### 4.1. Jornada Cruzada

1. **Criação:** O Organizador usa o Wizard Educativo para criar e publicar um evento.
    
2. **Descoberta:** O Participante usa o unnu smart (Chat IA) para encontrar esse evento baseado em intenção ("quero aprender sobre finanças").
    
3. **Compra:** O Participante adquire o ingresso. O Organizador vê a venda em tempo real no Dashboard Financeiro.
    
4. **Engajamento (Pré-Evento):** 24h antes, o Participante configura seu perfil de Networking e começa a dar "Match" com outros inscritos.
    
5. **Acesso:** Na porta, o Participante mostra o QR Code na Carteira. O Organizador usa o Scanner do app para validar.
    
6. **Pós-Evento:** O Organizador libera materiais (PDFs). O Participante acessa na aba "Histórico".
    

---

## 5. Telas do Sistema (Consolidado)

### 5.1. Telas Compartilhadas e Autenticação

- **Splash Screen (splash):** Fundo Roxo (Secondary), Logo Branco, Glow Verde (Primary). Duração 2s.
    
- **Login (auth):**
    
    - **Destaque:** Toggle Switch grande no topo (User ↔ Organizer). Muda a cor do tema (Verde/Roxo).
        
    - **Ações:** Login Social, Email/Senha, Link para cadastro.
        
- **Cadastro (register):** Wizard de 3 passos.
    
    1. Seleção de Perfil (Card Gigante).
        
    2. Dados Pessoais/Foto.
        
    3. Personalização: Tags de interesse (Participante) OU Dados da Empresa (Organizador).
        

### 5.2. Telas do Participante

- **Home (attendee_home):**
    
    - **Header:** "unnu smart", Créditos de IA (ex: 3/3), Botão de Alertas e Novidades.
        
    - **Busca IA:** Input estilo Chat ("Ex: Eventos de tech amanhã...").
        
    - **Resultados:** Lista de EventCard com filtros (Hoje, Amanhã, Presencial).
        
- **Detalhes do Evento (event_details):** Hero image, Título, Info Organizador, Seleção de Lotes (Tiers), Benefícios, Botão Sticky de Compra.
    
- **Checkout (checkout):** Resumo do pedido, Seleção Pagamento (Pix/Cartão), Totalizador.
    
- **Carteira (wallet):**
    
    - **Abas:** Próximos (Valid) / Histórico (Past).
        
    - **Ingresso Detalhado:** QR Code (brilho máximo), Info de Clima, Traje, Mapa.
        
- **Networking (networking):**
    
    - **Setup:** Foto, Bio, Cargo e **Tags de Interesse** (Máx 5, proibido cor roxa nas tags).
        
    - **Discovery:** Interface de Swipe (Tinder-style) com Cards de perfis.
        
    - **Chat:** Lista de Matches e janelas de conversa.
        
- **Perfil (profile):** Avatar, Dados, Configurações e Banner para "Virar Organizador".
    

### 5.3. Telas do Organizador

- **Dashboard (organizer_dashboard):**
    
    - **Filtros:** Período (7/30 dias).
        
    - **KPIs:** Faturamento Bruto/Líquido, Qtd Vendas.
        
    - **Gráficos:** Barras de vendas diárias.
        
- **Meus Eventos (organizer_events):** Lista (Ativos, Rascunhos, Passados). Botão "+" flutuante.
    
- **Assistente de Criação (organizer_wizard):** Fluxo de 13 passos.
    
    - Diferencial: Box lateral com Dicas Educacionais ("Por que pedir essa info?").
        
- **Gestão de Evento (organizer_event_detail):** Edição, Gestão de Lotes, Acesso a Lista de Participantes.
    
- **Scanner / Check-in (organizer_scanner):**
    
    - **Interface:** Simulação de Câmera com Laser Vermelho.
        
    - **Feedback:** Modal de Participante (Foto/Nome) -> Validar ou Recusar (Vermelho se duplicado).
        
- **Finanças (organizer_finance):**
    
    - **Simulação Asaas:** Cartão virtual azul, Saldo Disponível/Futuro, Extrato de Transações, Solicitação de Saque.
        
- **Perfil da Empresa (organizer_profile):** Edição de Bio Pública, Gestão de Equipe e Cupons.
    

---

## 6. Funcionalidades Completas do Sistema

### 6.1. Funcionalidades do Organizador

1. **Criação Guiada (Wizard):** Criação passo-a-passo com dicas de contexto (copywriting/vendas).
    
2. **Bilhetagem Avançada:** Gestão de Lotes (Tiers), Controle de Estoque, Virada de Lote manual/automática.
    
3. **Cupons de Desconto:** Percentual/Fixo, com travas por CPF ou quantidade.
    
4. **Check-in Profissional:** Scanner de QR Code e Lista Manual com busca/exportação.
    
5. **Gestão Financeira:** Visualização de saldo, split de pagamentos (taxa plataforma), extrato detalhado.
    
6. **Gestão de Equipe:** Convite de membros com permissões granulares (ADM, Gestor, Validador).
    
7. **Push Notifications:** Envio de mensagens em massa para participantes de um evento.
    
8. **Página Pública:** Perfil da organizadora com bio, redes sociais e lista de eventos ativos.
    

### 6.2. Funcionalidades do Participante

1. **Busca Semântica (IA):** Busca por intenção em linguagem natural.
    
2. **Carteira Digital:** Armazenamento offline-first de ingressos e QR Codes.
    
3. **Networking Gamificado:**
    
    - Perfil contextual (específico para o evento).
        
    - Matching baseado em tags (Swipe).
        
    - Chat P2P (liberado apenas após Match mútuo).
        
4. **Favoritos:** Salvar eventos de interesse.
    
5. **Informações Contextuais:** Previsão do tempo e dicas de acesso no ingresso.
    
6. **Materiais Pós-Evento:** Acesso a biblioteca de arquivos disponibilizados pelo organizador.
    

### 6.3. Compartilhadas e Segurança

1. **Identidade Única:** Login único para todos os perfis.
    
2. **Auth Blockers:** Modais que impedem ações críticas de visitantes.
    
3. **Compartilhamento:** Integração com API nativa de share.
    

---

## 7. Fluxos do Sistema

### 7.1. Fluxo de Criação (Org)

Dashboard -> Botão "+" -> Escolha (Guiado/Manual) -> Wizard (13 Passos: Dados, Imagens, Ingressos, Agenda) -> Revisão -> Publicar (Status: Active).

### 7.2. Fluxo de Compra (Part)

Busca/Home -> Detalhes Evento -> Selecionar Lote -> (Login se necessário) -> Checkout (Simulado) -> Sucesso -> Redireciona para Carteira.

### 7.3. Fluxo de Check-in (Híbrido)

1. **Participante:** Abre Carteira -> Seleciona Ingresso -> Exibe QR Code.
    
2. **Organizador:** Abre Scanner -> Lê QR Code -> Vê Foto/Nome -> Confirma Entrada.
    
3. **Sistema:** Marca ticket como used.
    

### 7.4. Fluxo de Networking (Part)

Evento (Faltando <24h) -> Aba Networking -> Setup Perfil (Tags) -> Discovery (Swipe) -> Match Mútuo -> Chat Aberto.

---

## 8. Inteligência Artificial no Sistema

A IA é um pilar de UX, não apenas backend.

- **Para o Participante (unnu smart):** Atua como Concierge.
    
    - Input: Texto livre.
        
    - Processamento: Extração de entidades (Data, Categoria, Preço) e filtro no banco de dados.
        
    - Output: Resposta conversacional + Cards de Eventos.
        
    - Limitação: Sistema de créditos (ex: 3 pesquisas grátis).
        
- **Para o Organizador (Assistente):** Atua como Consultor.
    
    - Contexto: Durante o Wizard de criação.
        
    - Ação: Fornece dicas estáticas (no MVP) contextualizadas ao campo atual (ex: "Títulos com 5 a 7 palavras têm 20% mais cliques").
        

---

## 9. Modelo de Dados Unificado

### 9.1. Entidades Principais (types.ts)

#### **User**

- id, name, email, role ('attendee' | 'organizer' | 'admin').
    
- preferences (tags de interesse).
    
- organizationData (se for organizador: nome empresa, CNPJ).
    

#### **Event**

- id, title, description, date, time, location.
    
- status ('draft' | 'active' | 'past' | 'cancelled').
    
- organizerId.
    
- ticketTiers: Array de TicketTier.
    
- schedule: Array de objetos cronograma.
    
- materials: Array de Material (Pré/Pós).
    
- isNetworkingEnabled: boolean.
    
- customization: Cores/Branding.
    

#### **TicketTier** (Lote)

- id, name (ex: 1º Lote), price, quantity, sold.
    
- status ('available' | 'sold_out' | 'hidden').
    
- benefits: Lista de IDs de benefícios.
    

#### **Ticket** (Ingresso Emitido)

- id, eventId, tierId, userId.
    
- qrCodeHash (Hash único para segurança).
    
- status ('valid' | 'used' | 'cancelled').
    

#### **NetworkingProfile**

- id, userId, eventId.
    
- bio, role, company.
    
- tags: Array de strings (Max 5).
    
- matches: Array de IDs de usuários.
    

#### **Invoice** (Transação)

- id, amount, netAmount (Líquido), date, status, type (Pix/Card).
    

#### **Coupon**

- code, type (% ou fixo), value, usageLimit.
    

---

## 10. Regras de Negócio Globais

1. **Taxa de Plataforma:** O sistema retém automaticamente 10% de cada venda. O netAmount no dashboard financeiro reflete isso.
    
2. **Visibilidade de Networking:** O botão/aba de networking só ativa 24 horas antes do início do evento.
    
3. **Validação de Ingresso:**
    
    - valid -> Pode entrar (vira used).
        
    - used -> Erro: "Já utilizado".
        
    - Evento errado -> Erro: "Evento Inválido".
        
4. **Hierarquia de Equipe:**
    
    - ADM: Tudo.
        
    - Gestor: Edita eventos e vê check-in (Sem financeiro).
        
    - Validador: Apenas Scanner.
        
5. **Exclusividade de Tag:** Tags de networking não podem usar a cor roxa (reservada para elementos de sistema/branding do organizador), devendo ser cinza escuro/preto.
    

---

## 11. Segurança e Permissões

- **AuthRequiredBlocker:** Componente de HOC (Higher Order Component) que envolve botões de ação (Comprar, Networking, Like). Se !user, abre modal de login.
    
- **Isolamento de Dados:** O Organizador só vê dados financeiros dos seus próprios eventos.
    
- **QR Code Seguro:** O QR Code contém um hash encriptado, não apenas o ID sequencial, prevenindo falsificação simples.
    

---

## 12. Integrações (Simuladas no MVP)

- **Asaas (Financeiro):** Mocks de API para simular saldo, extrato e antecipação de recebíveis.
    
- **Media Devices (Scanner):** Uso de navigator.mediaDevices.getUserMedia para acesso à câmera (simulado visualmente com CSS no protótipo, mas preparado para integração real).
    
- **Mapas:** Links externos para Google Maps/Waze baseados na string de localização.
    
- **Share API:** Uso de navigator.share para compartilhar eventos.
    

---

## 13. Design System & UI

### 13.1. Paleta de Cores

- **Primary (Lime):** #C9E567 (Light), #B5DB2E (Dark). Usado para ações do Participante, sucesso e acentos.
    
- **Secondary (Deep Purple):** #503A88 (Default), #402E6D (Dark). Identidade da marca e ambiente do Organizador.
    
- **Background:** #f6f6f6 (Soft White).
    
- **Text:** #0e0e0e (Almost Black).
    

### 13.2. Componentes Globais

- **Inputs:** rounded-xl, foco com ring primário.
    
- **Botões:** h-12, rounded-xl. Variação de cor baseada no Perfil Ativo.
    
- **Cards:** Sombra suave (shadow-sm), bordas arredondadas (rounded-2xl).
    
- **Bottom Navigation:** Flutuante com efeito glassmorphism (blur), rótulos aparecem apenas no item ativo.
    

---

## 14. Glossário Unificado

- **Wizard:** Fluxo passo-a-passo de criação de evento.
    
- **Tier:** Nível de ingresso (ex: VIP, Pista).
    
- **Batch:** Lote de vendas (ex: Lote Promocional).
    
- **Match:** Conexão mútua confirmada entre dois participantes.
    
- **Push:** Notificação enviada pelo app.
    
- **PWA:** Progressive Web App (tecnologia base).
    
- **Unnu Smart:** Funcionalidade de busca via IA.
    

---

Este documento consolida todas as especificações técnicas, funcionais e visuais para o desenvolvimento completo da plataforma **unnu**.

---

### 4. Decisões de Backend Críticas (Não adie isso)

Para o sistema funcionar, você precisa definir agora:

1. **Modelo de Split de Pagamento (Asaas):**
    -  **Split na fonte (Split Payment)**. O Organizador cria uma subconta (ou conecta a conta dele) e o Asaas divide o dinheiro automaticamente na transação. Isso evita bitributação e problemas fiscais para o unnu.
        
2. **Segurança do QR Code:**
    -  O QR Code deve ser um JWT assinado (assinatura criptográfica) contendo ticket_id + timestamp + salt, gerado no backend. O Scanner valida a assinatura offline ou online.
        
3. **Estratégia de "Busca IA" (unnu smart):**
    
    - Decisão: Como lidar com buscas vagas ("algo legal pra hoje")?
        
    - Recomendação: Hybrid Search. Usar filtro SQL para datas/local (WHERE date = today) + Busca Vetorial para o tema (text-embedding). A IA (LLM) serve apenas para extrair os filtros do texto do usuário, não para fazer a busca em si.
        
4. **Networking - Retenção de Dados:**
    
    - O chat será temporário, o networking é liberado 24h antes do evento e acaba ao fim do dia do evento, o chat durará 24h após o dia do evento e depois será deletado automaticamente
5. Sistema de recomendação
	- Preciso criar um sistema de recomendação de acordo com os gostos do usuário, ele poderá editar algumas tags no seu perfil com seus gostos e também devemos considerar os ingressos que ele comprou e o eventos que ele favorita 