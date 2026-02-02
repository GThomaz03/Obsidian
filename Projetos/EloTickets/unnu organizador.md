# unnu - Documentação Técnica e Especificação de Produto

**Versão do Documento:** 1.0.0  
**Status:** Protótipo Navegável (MVP)  
**Plataforma:** Web Mobile-First (PWA)

---

## 1. Visão Geral do Projeto

### 1.1. Objetivo

O **unnu** é uma plataforma SaaS mobile-first projetada para democratizar a gestão de eventos educacionais e de networking. O foco é empoderar "pequenos gigantes" — palestrantes, especialistas em finanças, mentores e criadores de conteúdo — oferecendo ferramentas profissionais de bilhetagem, gestão e engajamento que geralmente são restritas a grandes produtores.

### 1.2. Problema Solucionado

Pequenos organizadores sofrem com a complexidade de plataformas tradicionais (muitas opções irrelevantes, design datado, foco em desktop) ou a falta de profissionalismo de soluções caseiras (Pix manual, listas em planilhas). O unnu resolve isso simplificando a jornada com UX guiada e design moderno.

### 1.3. Contexto de Uso

O aplicativo é utilizado em movimento.

- **O Organizador** usa para verificar vendas, fazer check-in na porta do evento e criar eventos rapidamente pelo celular.
    
- **O Participante** usa para descobrir eventos, apresentar o ingresso (QR Code) e acessar materiais pós-evento.
    

### 1.4. Diferenciais

- **Super App Único:** Um único aplicativo comporta os perfis de Comprador e Organizador, com alternância fluida de contexto.
    
- **Criação Guiada (Wizard):** Um fluxo de criação de eventos que educa o organizador sobre melhores práticas enquanto ele preenche os dados.
    
- **Integração Financeira Transparente:** Visão clara de saldo e antecipação (simulação de integração Asaas).
    
- **Busca Inteligente (unnu smart):** Interface de busca baseada em linguagem natural/chat.
    

---

## 2. Conceito do Produto

### 2.1. Ideia Central

"Conectando Experiências". O app funciona como um hub onde a criação de um evento é tão simples quanto um post de rede social, mas com a robustez de um sistema financeiro.

### 2.2. Fluxo Macro

1. **Entrada:** Login unificado com seletor de perfil (Toggle Switch).
    
2. **Organizador:** Dashboard -> Criação (Manual ou Guiada) -> Gestão (Check-in/Notificações) -> Finanças.
    
3. **Participante:** Home (Busca IA/Feed) -> Compra -> Ingresso (QR Code) -> Conteúdo Pós-Evento.
    

---

## 3. Perfis de Usuário

### 3.1. Visitante (Guest)

- **Permissões:** Visualizar lista de eventos, buscar eventos, ver detalhes públicos de eventos e perfis de organizadores.
    
- **Limitações:** Não pode comprar, não pode favoritar, não tem acesso a créditos de IA.
    
- **Ações de Bloqueio:** Ao tentar comprar ou favoritar, um modal (AuthRequiredBlocker) intercepta a ação solicitando login.
    

### 3.2. Participante (Attendee)

- **Permissões:** Todas do Visitante + Comprar ingressos, acessar carteira de ingressos, favoritar eventos, usar busca inteligente (créditos limitados), acessar materiais pós-evento.
    
- **Dados:** Possui lista de interesses (tags) e histórico de compras.
    

### 3.3. Organizador (Organizer)

- **Permissões:** Criar/Editar eventos, gerenciar lotes, validar ingressos (Scanner), enviar notificações push, visualizar dashboard financeiro, gerenciar equipe, editar perfil público da empresa.
    
- **Transição:** Pode alternar para a visão de Participante através do menu de Perfil.
    

---

## 4. Arquitetura Geral

### 4.1. Stack Tecnológico

- **Frontend:** React 19 (Hooks, Functional Components).
    
- **Estilização:** Tailwind CSS (Utility-first).
    
- **Ícones:** Lucide React.
    
- **Gráficos:** Recharts.
    
- **Roteamento:** Gerenciamento de estado local via useState na raiz (App.tsx), simulando um roteador SPA.
    

### 4.2. Padrões Arquiteturais

- **Mobile-First Container:** A aplicação inteira é encapsulada em um container centralizado com largura máxima (max-w-md), simulando a viewport de um celular mesmo em desktops.
    
- **View-Based State Routing:** A navegação não utiliza URL routing tradicional (como react-router), mas sim uma variável de estado view (ViewState) que renderiza condicionalmente os componentes.
    
- **Mock Data Service:** Todos os dados (eventos, tickets, usuários) são servidos por services/mockData.ts e manipulados em memória.
    

### 4.3. Dependências e Integrações (Simuladas)

- **Asaas (Financeiro):** O app simula uma integração profunda com a API do Asaas para saldo, extrato e antecipação.
    
- **Câmera/Scanner:** Simulação de interface de câmera para leitura de QR Code.
    

---

## 5. Telas do Aplicativo (UI/UX)

Abaixo, a descrição detalhada de cada tela mapeada no ViewState.

### 5.1. Autenticação e Onboarding

#### **Splash Screen (splash)**

- **Visual:** Fundo roxo (Secondary), logo "unnu" branco centralizado, tagline "Conectando Experiências".
    
- **Animação:** Glow effect (blur) em verde (Primary) ao fundo.
    
- **Comportamento:** Transição automática após 2 segundos para attendee_home.
    

#### **Login (auth)**

- **Componentes:**
    
    - Header com **Toggle Switch** grande e animado: Alterna entre ícone de Usuário (Participante) e Maleta (Organizador). O fundo do switch muda de cor (Verde para user, Roxo para organizer).
        
    - Inputs: Email e Senha.
        
    - Botão "Entrar" (Variante muda conforme perfil).
        
    - Botão "Ativar FaceID" (apenas participante).
        
    - Login Social: Google, Apple/LinkedIn.
        
- **Lógica:** O toggle define o role inicial.
    

#### **Cadastro (register)**

- **Estrutura:** Wizard de 3 passos com barra de progresso superior.
    
    1. **Seleção de Perfil:** Cards grandes selecionáveis (Participante ou Organizador).
        
    2. **Dados Básicos:** Upload de foto (simulado), Nome, Email, Senha.
        
    3. **Personalização:**
        
        - Participante: Seleção de tags de interesse (Finanças, Tech, etc.).
            
        - Organizador: Confirmação de conta empresarial.
            

---

### 5.2. Visão do Participante

#### **Home (attendee_home)**

- **Estado Inicial (initial):**
    
    - Header: "unnu smart".
        
    - Créditos de IA: Card exibindo 3/3 créditos (se logado).
        
    - **Barra de Busca:** Input grande estilo chat/textarea ("Ex: Quero ir em um evento de marketing...").
        
    - Sugestões Rápidas: Chips com ícones (Eventos Tech, Networking Grátis, etc.).
        
- **Estado Resultados (results):**
    
    - Header Compacto: Botão voltar, ícone de Alertas (UX Alerts) e Notificações (Push).
        
    - Filtros Horizontais: Todos, Hoje, Amanhã, Presencial, Online.
        
    - Chat Interface: Exibe o prompt do usuário e a resposta da IA ("Encontrei X eventos...").
        
    - Lista de Cards de Eventos.
        
- **Card de Evento:**
    
    - Imagem 16:9 com gradiente.
        
    - Badges sobre a imagem: Categoria, "Melhor Match" (se aplicável), Preço ("A partir de").
        
    - Botão de Favoritar (Coração) flutuante.
        
    - Rodapé: Título, Data/Hora, Local, Público Alvo (com ícone Target), Avatares de participantes (+X pessoas).
        

#### **Detalhes do Evento (event_details)**

- **Header:** Imagem full-width com botões de Voltar e Share sobrepostos. Título e Categoria flutuando na parte inferior da imagem.
    
- **Conteúdo:**
    
    - Card do Organizador (clicável -> vai para perfil público).
        
    - Descrição do evento.
        
    - Localização e Link de Mapa.
        
    - **Seleção de Ingressos:** Lista agrupada por Lotes. Cada tier mostra Título, Preço e Status (Disponível/Esgotado).
        
    - Benefícios Inclusos: Checklist dinâmico baseado no ingresso selecionado.
        
- **Sticky Footer:** Preço total e botão "Comprar Ingresso".
    

#### **Ingresso / Ticket (TicketDetail)**

- **Visual:** Fundo cinza claro.
    
- **Header:** Imagem do evento com info resumida.
    
- **QR Code:** Botão flutuante ou área destacada para abrir o código.
    
- **Info Contextual:** Clima (sol/chuva), Tipo de Local (Indoor/Outdoor), Método de Acesso (Catraca/Lista).
    
- **Abas/Seções:** Cronograma do evento e Materiais Pré-Evento (Download).
    

#### **Pós-Evento (AttendeePostEvent)**

- **Objetivo:** Listar materiais disponibilizados após o fim do evento.
    
- **Lista:** Cards com ícone do tipo (Vídeo, PDF, Link), Título e Botão de Download.
    

#### **Perfil Público do Organizador (attendee_organizer_profile)**

- **Header:** Capa e Avatar da empresa.
    
- **Info:** Bio, botão "Seguir" e "Contato".
    
- **Lista:** Todos os eventos ativos desse organizador.
    

---

### 5.3. Visão do Organizador

#### **Dashboard (organizer_dashboard)**

- **Header:** "Dashboard", Avatar da empresa.
    
- **Filtros:** Dropdown de Evento (Todos ou Específico), Filtro de Período (7 dias, 30 dias, Custom).
    
- **KPI Cards:**
    
    - Faturamento (Roxo): Valor total e Receita Líquida.
        
    - Vendas (Branco): Quantidade e tag de demanda.
        
- **Gráfico:** Gráfico de barras (Recharts) mostrando vendas por dia/semana.
    
- **Breakdown:** Lista de vendas por tipo de ingresso ou por evento, com barra de progresso percentual.
    

#### **Meus Eventos (organizer_events)**

- **Header:** Botão "+" para criar evento.
    
- **Filtro:** Abas (Ativos, Rascunhos, Passados).
    
- **Lista:** Cards de eventos com miniatura, título, data, e métricas rápidas (participantes e receita acumulada). Se a lista estiver vazia, exibe "Empty State".
    
- **Create Mode Select (organizer_create_mode_select):** Tela intermediária para escolher entre "Criação Guiada" (Compass) ou "Manual" (Edit3).
    

#### **Assistente de Criação (OrganizerGuidedCreation / EventCreationWizard)**

- **Layout:** Barra de progresso no topo. Rodapé fixo com navegação (Voltar/Próximo).
    
- **Conteúdo:** Título grande da etapa.
    
- **Área Educacional (Diferencial):** Box com gradiente contendo "Por que isso importa?" e "Dica da unnu" (ícone Zap/Raio), explicando o impacto comercial do campo atual.
    
- **Etapas:**
    
    1. Imagem de Capa.
        
    2. Nome do Evento.
        
    3. Categorias/Tags.
        
    4. Público Alvo (Input com contador de caracteres).
        
    5. Data e Hora.
        
    6. Localização.
        
    7. Descrição.
        
    8. Cronograma (opção de pular).
        
    9. Materiais Extras (Pré/Pós evento).
        
    10. Lotes e Ingressos (Gestão de tiers, preços e benefícios).
        
    11. Canais de Suporte.
        
    12. Agendamento de Publicação.
        
    13. Resumo e Checklist.
        

#### **Gestão de Evento (organizer_event_detail)**

- **Modos de Visão:** Toggle entre "Gestão" e "Visão Pública" (Preview do participante).
    
- **Header:** Imagem com ações (Editar, Ver como Público).
    
- **Ações Rápidas (Grid):** Lista de Participantes, Push Notifications, Link de Compartilhamento.
    
- **Card de Desempenho:** Ingressos vendidos, Faturamento, Barra de meta atingida.
    
- **Seção Condicional (Passado):** Se o evento passou, exibe gestão de "Materiais Pós-Evento" (Upload de arquivos/links).
    
- **Seção Condicional (Ativo):** Exibe lista de Lotes e Disponibilidade.
    

#### **Scanner / Check-in (organizer_scanner)**

- **Visual:** Simulação de Câmera (Background escuro com blur e viewport central).
    
- **Estados:**
    
    - Scanning: Animação de laser vermelho.
        
    - Preview: Modal com dados do participante (Nome, Foto, Tipo Ingresso, CPF). Ações: "Validar Acesso" ou "Cancelar".
        
    - Feedback:
        
        - Sucesso (não explícito, reseta scanner).
            
        - Erro/Usado: Tela cheia vermelha ou laranja com motivo ("Já utilizado", "Inválido").
            
- **Manual:** Opção de selecionar evento (organizer_checkin_select_event) e ver lista (organizer_checkin_list).
    
    - Lista permite busca, filtro e exportação (Excel/PDF).
        

#### **Finanças (organizer_finance)**

- **Card Asaas:** Design imitando cartão de crédito azul/gradiente.
    
    - Exibe: Saldo Disponível, Futuro, Limite de Saque Mensal (Barra de progresso).
        
    - Ações: "Acessar Internet Banking".
        
- **Extrato:**
    
    - Filtros: Status (Todos, Aprovados, Pendentes), Data.
        
    - Lista de transações (Ícone verde para Pix/Pago, Laranja para Pendente).
        
- **Detalhe da Transação (Modal):** Valor líquido, Data, ID, Método, Comprador e breakdown de taxas (Taxa da plataforma 10%).
    

#### **Perfil e Configurações (organizer_profile & subs)**

- **Menu Principal:**
    
    - Dados da Empresa (organizer_profile_edit): Editor de formulário.
        
    - Perfil Público (organizer_profile_public): Editor visual com capa, avatar, bio e links sociais.
        
    - Cupons (organizer_coupons): Lista e editor de cupons (Percentual/Fixo, Limite por CPF, Tickets aplicáveis).
        
    - Minha Equipe (organizer_profile_team): Lista de membros com papéis (ADM, Gestor, Validador). Modal de convite e alteração de permissão.
        
    - Contas Bancárias (organizer_profile_payouts).
        
    - Configurações (organizer_profile_settings): Toggles de notificação e segurança.
        

---

## 6. Fluxos do Usuário

### 6.1. Fluxo Principal: Compra de Ingresso (Participante)

1. Abre o app (Guest/Attendee).
    
2. Na Home, clica em um evento recomendado ou busca via "unnu smart".
    
3. Visualiza detalhes (EventDetails).
    
4. Clica em "Comprar Ingresso".
    
5. Seleciona o tipo de ingresso (Tier).
    
6. Se não logado -> AuthRequiredBlocker -> Login/Cadastro -> Retorna.
    
7. Confirmação de compra (Simulada).
    
8. Redirecionamento para o Ingresso/QR Code.
    

### 6.2. Fluxo Principal: Criação de Evento (Organizador)

1. Login como Organizador.
    
2. Dashboard -> Aba Eventos -> Botão "+".
    
3. Escolhe "Criação Guiada" (Compass).
    
4. Passa pelos 13 passos do Wizard, lendo as dicas e preenchendo inputs.
    
5. No último passo, revisa o Checklist.
    
6. Clica em "Salvar Rascunho" (Eventos nascem como rascunho até a data de publicação).
    

### 6.3. Fluxo de Check-in (Portaria)

1. Organizador clica no ícone de Scanner na Tab Bar.
    
2. Aponta câmera para QR Code.
    
3. Sistema identifica ingresso.
    
4. Exibe modal com foto e nome do participante.
    
5. Organizador clica em "Validar Acesso".
    
6. Sistema registra check-in e retorna ao modo de leitura.
    

---

## 7. Funcionalidades (Lista Completa)

### Funcionalidades Core

1. **Gestão de Identidade:** Login, Cadastro, Recuperação, Troca de Perfil (User <-> Org).
    
2. **Motor de Eventos:** CRUD completo de eventos (Título, Data, Local, Descrição, Imagens).
    
3. **Bilhetagem:** Criação de Lotes, Tipos de Ingressos, Definição de Preços e Quantidades.
    
4. **Checkout:** Simulação de compra, cálculo de total.
    
5. **Check-in:** Validação de QR Code, Lista manual, Controle de status (Válido/Usado/Inválido).
    

### Funcionalidades Organizador

1. **Wizard Educativo:** Criação passo-a-passo com dicas de contexto.
    
2. **Dashboard Financeiro:** Visão de receita bruta/líquida, integração Asaas simulada.
    
3. **Gestão de Equipe:** Adicionar validadores/gestores com permissões granulares.
    
4. **Cupons:** Criação de códigos promocionais com regras (limite de uso, CPF único).
    
5. **Notificações Push:** Agendamento de mensagens para participantes do evento.
    
6. **Materiais Pós-Evento:** Upload de arquivos para quem compareceu.
    
7. **Perfil Público:** Página personalizada da organizadora (Bio, Redes, Eventos).
    

### Funcionalidades Participante

1. **Busca Semântica (Simulada):** Interface de chat para busca de eventos.
    
2. **Carteira de Ingressos:** Visualização de QR Code e detalhes de acesso.
    
3. **Informações de Contexto:** Previsão do tempo no dia/local do evento, tipo de traje/local.
    
4. **Favoritos:** Salvar eventos para depois.
    

---

## 8. Inteligência Artificial

A IA no unnu não é apenas um backend, é parte da interface.

- **unnu smart (Busca):**
    
    - **Interface:** Chat na Home do participante.
        
    - **Comportamento:** O usuário digita uma intenção ("Quero workshops de tech amanhã"). O sistema processa (simulado via filtro de palavras-chave no código atual) e retorna resultados formatados como uma conversa, consumindo "Créditos de Pesquisa".
        
- **Criação Assistida (Wizard):**
    
    - **Interface:** Sidebar/Cards dentro do fluxo de criação.
        
    - **Conteúdo:** Dicas estáticas (no MVP) mas contextualizadas que explicam o porquê de cada campo (Ex: "Use fotos de pessoas sorrindo, aumentam a conversão em 3x").
        

---

## 9. Dados e Modelos de Dados

### Entidades Principais (types.ts)

#### **Event**

- id: string
    
- title, description, category, date, time, location: strings
    
- price: number (preço base "a partir de")
    
- status: 'active' | 'past' | 'draft'
    
- ticketTiers: Array de TicketTier
    
- schedule: Array de objetos {time, title, speaker}
    
- materials: Array de Material (Pré e Pós)
    
- customization: Cores primárias/secundárias do evento.
    

#### **TicketTier** (Lote/Ingresso)

- id, title: string
    
- batch: string (Nome do lote)
    
- price: number
    
- includedBenefits: Array de strings (IDs de benefícios)
    
- status: 'available' | 'sold_out'
    

#### **Coupon**

- code: string (Unique)
    
- type: 'percentage' | 'fixed'
    
- value: number
    
- limitByCpf: boolean (Regra de negócio)
    
- applicableTicketIds: Array de strings (quais ingressos aceitam).
    

#### **Invoice** (Transação Financeira)

- amount: number
    
- status: 'paid' | 'pending'
    
- date: string
    

---

## 10. Regras de Negócio

1. **Visibilidade de Evento:** Eventos criados nascem como draft. Só aparecem na busca pública se status === 'active' ou se a data atual >= publishDate.
    
2. **Validação de Check-in:** Um ingresso used não pode ser validado novamente. Ingressos de outro evento são invalid.
    
3. **Hierarquia de Equipe:**
    
    - ADM: Acesso total (Financeiro + Eventos).
        
    - Gestor: Eventos + Check-in (Sem Financeiro).
        
    - Validador: Apenas Scanner e Lista de Presença.
        
4. **Taxa de Plataforma:** O sistema calcula automaticamente 10% de taxa sobre as vendas no Dashboard Financeiro (netRevenue = gross * 0.9).
    
5. **Restrição de Visitante:** Visitantes não logados podem ver, mas não podem interagir (comprar/favoritar).
    

---

## 11. Estados do Sistema

O controle de estado é centralizado no componente App.tsx via useState.

- **view**: Define qual tela está sendo renderizada.
    
- **role**: Define o perfil atual (guest, attendee, organizer).
    
- **selectedEvent**: Armazena o objeto do evento sendo manipulado ou visualizado.
    
- **attendeeHomeState**: Persiste a query de busca e resultados para não perder o contexto ao navegar para detalhes e voltar.
    

---

## 12. Integrações (Simuladas no MVP)

- **Asaas:** A tela OrganizerFinance renderiza dados baseados em MOCK_INVOICES, simulando uma API de gateway de pagamento.
    
- **Maps:** Links externos para Google Maps baseados no campo location.
    
- **Câmera:** A tela OrganizerScanner usa CSS para simular o feed de câmera, mas deve ser integrada à API navigator.mediaDevices.getUserMedia na implementação real.
    

---

## 13. Design System & UI

### Paleta de Cores

- **Primary (Lime):** #C9E567 (Light), #B5DB2E (Dark). Usado para acentos no modo Participante e destaques de sucesso.
    
- **Secondary (Deep Purple):** #503A88 (Default), #402E6D (Dark). Cor principal da marca e do modo Organizador.
    
- **Background:** #f6f6f6 (Soft White).
    
- **Text:** #0e0e0e (Almost Black).
    

### Componentes Globais

- **Inputs:** Bordas arredondadas (rounded-xl), foco com ring primário.
    
- **Botões:** rounded-xl, altura h-12, variantes sólidas e outline.
    
- **Cards:** Sombra suave (shadow-sm), borda fina (border-gray-100).
    

---

## 14. Glossário

- **Wizard:** Fluxo passo-a-passo de criação.
    
- **Tier:** Categoria de ingresso (ex: VIP, Inteira).
    
- **Batch:** Lote de vendas (ex: 1º Lote).
    
- **Match:** (Conceito futuro) Sistema de recomendação entre participantes.
    
- **Push:** Notificação enviada pelo organizador para o app dos participantes.
    

---

## 15. Instruções para Recriação

1. **Setup:** Iniciar projeto React com TypeScript e Tailwind CSS configurado conforme index.html.
    
2. **Types:** Copiar types.ts para definir a estrutura de dados.
    
3. **Mock Data:** Implementar services/mockData.ts com os dados fornecidos.
    
4. **Componentes Base:** Implementar Button, Input, Header, Card, Badge em components/Common.tsx.
    
5. **Layout:** Implementar AppLayout com a lógica de BottomNavigation condicional.
    
6. **Views:** Criar cada view em sua pasta correspondente (views/organizer, views/Attendee.tsx, views/Auth.tsx), seguindo a estrutura de componentes funcionais.
    
7. **Lógica Central:** Implementar o App.tsx com o switch case gigante para gerenciar o viewState.
    
8. **Estilização:** Aplicar classes Tailwind rigorosamente conforme descrito nas seções de UI (gradientes, rounded corners, shadows).