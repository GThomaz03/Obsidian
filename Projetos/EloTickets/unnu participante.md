# Especificação Técnica do Projeto: unnu (Mobile Application)

**Versão do Documento:** 1.0.0  
**Status:** Definição de Arquitetura e Design  
**Plataforma:** Web Mobile / PWA / Native Wrapper (React Ecosystem)

---

## 1. Visão Geral do Projeto

### 1.1 Objetivo do Aplicativo

O **unnu** é uma plataforma SaaS mobile-first projetada para democratizar a gestão e venda de ingressos para eventos educacionais, workshops e palestras de pequeno e médio porte. O objetivo é fornecer ferramentas profissionais (geralmente restritas a grandes produtores) para especialistas independentes (ex: finanças, consórcio, mentorias).

### 1.2 Problema Resolvido

Pequenos organizadores sofrem com ferramentas complexas demais ou informais demais (ex: gestão via WhatsApp/Planilhas). O unnu resolve a fricção de venda, a gestão de check-in e, crucialmente, o networking qualificado entre participantes, que é o maior valor agregado desses eventos.

### 1.3 Público-Alvo

1. **Organizadores:** Palestrantes, mentores, produtores de conteúdo, especialistas em nicho.
    
2. **Participantes (Attendees):** Profissionais em busca de capacitação e networking.
    

### 1.4 Contexto de Uso

O app é utilizado em dois momentos distintos:

- **Pré-evento:** Descoberta, compra, gestão de vendas e configuração do evento.
    
- **Durante o evento:** Check-in (leitura de QR), acesso ao ingresso, e networking ativo (matchmaking).
    

### 1.5 Diferenciais

- **App Único Híbrido:** O mesmo aplicativo serve para Organizador e Participante, com troca de contexto imediata.
    
- **Networking Gamificado:** Sistema estilo "Tinder" para conectar participantes com interesses profissionais mútuos.
    
- **IA Conversacional:** Busca de eventos baseada em linguagem natural.
    

---

## 2. Conceito do Produto

### 2.1 Ideia Central

"Conectar experiências". O unnu não vende apenas o ingresso, ele vende a conexão que o evento proporciona. O design é minimalista, limpo e focado em usabilidade extrema (thumb-friendly).

### 2.2 Fluxo Macro

1. Usuário faz Login/Cadastro.
    
2. Seleciona Perfil (Participante ou Organizador).
    
3. **Se Participante:** Busca eventos (IA ou filtros) -> Compra -> Acessa Carteira -> Configura Networking -> Realiza Match -> Chat.
    
4. **Se Organizador:** Dashboard de Vendas -> Criação de Evento (Wizard) -> Gestão de Lotes -> Check-in (Scanner) -> Pós-evento.
    

### 2.3 Papel da Inteligência Artificial

A IA no unnu atua como um Concierge.

- **Na Busca:** O usuário digita "Quero eventos de marketing em SP semana que vem" e o sistema interpreta a intenção para filtrar resultados.
    
- **No Networking (Futuro):** Sugestão de conexões baseada na bio e tags de interesse.
    

---

## 3. Perfis de Usuário

### 3.1 Visitante (Guest)

- **Permissões:** Visualizar lista de eventos públicos, ver detalhes do evento.
    
- **Limitações:** Não pode comprar, favoritar ou acessar networking. Bloqueio via AuthRequiredBlocker.
    

### 3.2 Participante (Attendee)

- **Permissões:**
    
    - Comprar ingressos.
        
    - Acessar carteira digital (QR Codes).
        
    - Favoritar eventos.
        
    - Configurar perfil de networking.
        
    - Interagir no sistema de Match e Chat.
        
    - Editar perfil pessoal e pagamentos.
        

### 3.3 Organizador (Organizer)

- **Permissões:**
    
    - Todas as permissões de Participante (via troca de perfil).
        
    - Criar, Editar e Cancelar eventos.
        
    - Visualizar Dashboard financeiro e de vendas.
        
    - Gerenciar Lotes de ingressos.
        
    - Realizar Check-in (Scanner de QR Code).
        
    - Enviar notificações Push para participantes.
        
    - Gerenciar equipe (Staff).
        

---

## 4. Arquitetura Geral

### 4.1 Frontend

- **Framework:** React 19.
    
- **Linguagem:** TypeScript.
    
- **Estilização:** Tailwind CSS (Utility-first).
    
- **Ícones:** Lucide React.
    
- **Gráficos:** Recharts.
    
- **Estrutura:** Single Page Application (SPA) simulando navegação nativa.
    

### 4.2 Gerenciamento de Estado

- **Local:** useState, useRef para interações de UI imediatas.
    
- **Global (Conceitual):** O app utiliza um estado elevado no App.tsx para controlar a View atual (currentView), o Papel (role) e dados de sessão.
    

### 4.3 Padrões de Design

- **Componentização Atômica:** Botões, Inputs, Cards e Badges centralizados em components/Common.tsx.
    
- **View Controller:** O arquivo App.tsx atua como um roteador central que renderiza a tela baseada no estado view.
    
- **Layout Wrapper:** AppLayout envolve as telas para fornecer a barra de navegação inferior (Bottom Nav) condicionalmente.
    

---

## 5. Telas do Aplicativo (UI/UX Detalhado)

### 5.1 Telas Comuns (Auth)

#### 5.1.1 Splash Screen

- **Visual:** Fundo cor secundária (Roxo), logo centralizado, efeito de "glow" (blur) com cor primária (Lima).
    
- **Comportamento:** Exibição por 2 segundos, transição automática para Home ou Login.
    

#### 5.1.2 Login Screen

- **Componentes:**
    
    - Toggle Switch no topo: Alternar entre "Para Você" (Participante) e "Para Organizadores".
        
    - Inputs: Email e Senha.
        
    - Botões Sociais: Google, Apple/LinkedIn.
        
    - CTA Primário: "Entrar".
        
- **Comportamento:** O toggle altera o texto de boas-vindas e o fluxo de destino pós-login.
    

#### 5.1.3 Register Screen (Wizard 3 Passos)

- **Passo 1:** Escolha de Perfil (Cards grandes selecionáveis: Participante ou Organizador).
    
- **Passo 2:** Dados Básicos (Foto, Nome, Email, Senha).
    
- **Passo 3:**
    
    - Participante: Seleção de Tags de Interesse.
        
    - Organizador: Dados da empresa (Nome, Nicho, Instagram).
        
- **UI:** Barra de progresso superior.
    

---

### 5.2 Telas do Participante (Attendee)

#### 5.2.1 Home (AttendeeHome)

- **Header:**
    
    - Título "unnu AI".
        
    - **Botão Esquerdo (Alerta):** Ícone AlertCircle. Abre modal "Seus Eventos" (Informações críticas: Check-in liberado, Clima, Alertas operacionais).
        
    - **Botão Direito (Megafone):** Ícone Megaphone. Abre modal "Novidades" (Promoções, novos recursos, marketing).
        
- **Busca:**
    
    - **Modo Inicial:** Campo de texto grande (Textarea) simulando chat com IA. "Ex: Quero um evento de marketing...". Botão de microfone e enviar.
        
    - **Modo Resultados:** Barra de busca compacta no topo + Lista de Filtros horizontais (Pills flutuantes sem borda, estilo minimalista).
        
- **Lista de Eventos:**
    
    - Cards verticais (EventCard).
        
    - Exibe: Imagem, Categoria (Badge), Título, Data/Hora, Local, Preço (Badge flutuante), Botão Favorito.
        
    - **Barra de Vendas:** No rodapé do card, barra de progresso indicando "% Vendido".
        

#### 5.2.2 Detalhes do Evento (EventView)

- **Hero:** Imagem full-width no topo com gradiente. Botões de voltar, compartilhar e favoritar sobrepostos.
    
- **Informações:** Título, Data, Hora, Local (com link para mapa), Organizador (Avatar + Nome).
    
- **Ingressos:** Lista de tiers (Lotes) agrupados. Seleção via clique no card do ingresso.
    
- **Footer Fixo:** Preço total e botão "Comprar Ingresso".
    

#### 5.2.3 Checkout

- **Resumo:** Miniatura do evento e ingresso selecionado.
    
- **Pagamento:** Seleção visual (Cartão vs PIX).
    
- **Valores:** Subtotal, Taxa de serviço, Total.
    
- **Ação:** Botão "Confirmar Pagamento".
    

#### 5.2.4 Carteira (WalletView)

- **Tabs:** "Próximos" (Valid) vs "Histórico" (Past).
    
- **Lista:** Cards horizontais.
    
    - Próximos: Destaque para ação "Ver Ingresso".
        
    - Passados: Destaque para ação "Ver Materiais/Certificado".
        
- **Botão Flutuante (Header):** Atalho para alertas críticos.
    

#### 5.2.5 Detalhe do Ingresso & QR Code

- **Detalhe:** Mapa, Previsão do tempo (ícone), Cronograma do dia, Materiais pré-evento.
    
- **QR Code (Modal):** Tela cheia, fundo escuro, brilho máximo. QR Code centralizado, código alfanumérico abaixo, nome do portador e tipo de ingresso.
    

#### 5.2.6 Networking (Fluxo Completo)

- **Lista de Eventos:** Mostra apenas eventos com networking ativo. Status: "Aberto" (verde) ou "Abre 24h antes" (cadeado).
    
- **Setup de Perfil:**
    
    - Foto, Cargo, Empresa, Bio.
        
    - **Tags de Interesse:** Campo de busca com autocompletar.
        
        - **Visual:** Tags selecionadas em **Cinza Escuro/Preto** (Dark). Tags sugeridas em cinza claro. **NENHUM ROXO**.
            
        - **Layout:** Área de sugestões limitada a **3 linhas** de altura com scroll interno.
            
        - **Contador:** Badge "X/5" no topo. Ao atingir 5, o input bloqueia (sem aviso de texto extra).
            
- **Menu Networking:** Dois botões grandes: "Descobrir" (Swipe) e "Conversas" (Chat).
    
- **Discovery (Swipe):** Card do perfil (Foto, Nome, Cargo, Bio, Tags). Botões de "X" (Pular) e "Check" (Conectar).
    
- **Chat:** Lista de matches. Tela de chat individual estilo WhatsApp.
    

#### 5.2.7 Perfil do Usuário

- Avatar, Nome, Email.
    
- Menu de navegação: Dados Pessoais, Pagamentos, Configurações, Favoritos.
    
- **Banner de Conversão:** Se o usuário não for organizador, exibe card "Tornar-se Organizador". Se for, exibe "Modo Organizador".
    

---

### 5.3 Telas do Organizador

#### 5.3.1 Dashboard (Home do Organizador)

- **Header:** Título "Dashboard". Seletor de período (7 dias, 30 dias).
    
- **KPIs:** Faturamento Bruto, Lucro Líquido, Qtd Ingressos.
    
- **Gráfico:** Gráfico de barras (Recharts) mostrando tendência de vendas.
    
- **Breakdown:** Lista detalhada de vendas por tipo de ingresso ou por evento.
    

#### 5.3.2 Criação de Evento (Wizard)

- **Passo 1 (Info):** Capa (Upload), Nome, Categoria, Data/Hora, Local.
    
- **Passo 2 (Cronograma):** Adição de itens na agenda (Horário, Título, Palestrante).
    
- **Passo 3 (Materiais):** Upload de PDFs/Links pré e pós evento.
    
- **Passo 4 (Ingressos):** Criação de Lotes e Tipos de Ingresso (Nome, Qtd, Preço).
    
- **Passo 5 (Revisão):** Resumo e validação de campos obrigatórios.
    

#### 5.3.3 Gestão do Evento (Detalhe)

- **Mini Dashboard:** Progresso de lotação (barra) e receita do evento específico.
    
- **Ações Rápidas:** Lista de Participantes, Enviar Notificação, Divulgar.
    
- **Atividade Recente:** Feed de vendas e check-ins em tempo real.
    

#### 5.3.4 Check-in (Scanner)

- **Visual:** Tela simulando câmera. Moldura de foco central.
    
- **Ação:** Leitura automática de QR Code.
    
- **Fallback:** Botão para input manual de código ou busca por nome.
    

#### 5.3.5 Lista de Participantes

- Barra de busca.
    
- Lista com: Avatar (iniciais), Nome, Tipo de Ingresso.
    
- Status: "Check-in realizado" (Verde) ou Botão "Fazer Check-in" (Manual).
    

---

## 6. Fluxos do Usuário

### 6.1 Fluxo de Compra (Participante)

1. **Home:** Usuário clica em um evento.
    
2. **Detalhes:** Usuário revisa infos e clica em "Comprar Ingresso".
    
3. **Seleção:** Usuário escolhe o ingresso (Tier) desejado (seleção padrão no primeiro disponível).
    
4. **Checkout:** Usuário revisa valores e método de pagamento -> Clica "Confirmar".
    
5. **Sucesso:** Tela de confirmação com animação -> Botão "Ver na Carteira".
    

### 6.2 Fluxo de Check-in (Organizador)

1. **Dashboard:** Seleciona evento -> Clica em "Scanner" (implícito ou via menu).
    
2. **Leitura:** Aponta câmera para o QR Code do participante.
    
3. **Validação:** Sistema verifica validade.
    
4. **Feedback:** Tela verde (Sucesso) ou Vermelha (Inválido/Já utilizado).
    

### 6.3 Fluxo de Networking (Match)

1. **Lista:** Usuário seleciona evento (apenas se faltar <24h).
    
2. **Setup:** Preenche perfil e tags (se for o primeiro acesso ao networking daquele evento).
    
3. **Discovery:** Visualiza card de outro participante.
    
4. **Ação:** Clica "Check" (Like).
    
5. **Match:** Se o outro usuário também deu Like -> Abre canal de chat.
    
6. **Chat:** Troca de mensagens textuais.
    

---

## 7. Funcionalidades (Lista Completa)

### 7.1 Funcionalidades Principais

- **Autenticação:** Login, Cadastro, Recuperação de Senha (mock), Troca de Perfil.
    
- **Gestão de Eventos:** Criar, Editar, Excluir, Listar (Ativos/Passados/Rascunhos).
    
- **Venda de Ingressos:** Catálogo, Carrinho (single item), Checkout, Emissão de Comprovante.
    
- **Carteira Digital:** Visualização de ingressos, Geração de QR Code dinâmico.
    
- **Check-in:** Validação de ingresso via QR Code ou Manual.
    

### 7.2 Funcionalidades de Networking

- **Perfil Temporário:** Criação de perfil específico para o contexto do evento.
    
- **Tags de Interesse:** Sistema de tags para filtro de relevância.
    
- **Matchmaking:** Lógica de "Like/Pass".
    
- **Chat:** Mensageria em tempo real (mockada) entre matches.
    

### 7.3 Funcionalidades de Engajamento

- **Alertas (Home):** Central de notificações críticas (mudança de lote, check-in aberto).
    
- **Novidades (Home):** Feed de notícias da plataforma.
    
- **Notificações Push:** Organizador pode enviar alertas para os participantes do evento.
    
- **Favoritos:** Salvar eventos para ver depois.
    

### 7.4 Funcionalidades Financeiras

- **Dashboard:** Visão de receita bruta e líquida.
    
- **Extrato:** Histórico de repasses (Organizer Finance).
    

---

## 8. Inteligência Artificial

### 8.1 Busca Conversacional (unnu AI)

- **Local:** Home do Participante.
    
- **Input:** Texto livre (Linguagem Natural).
    
- **Processamento:**
    
    - Analisa intenção (ex: "Barato", "Hoje", "Tech").
        
    - Mapeia para filtros de banco de dados (Category, Date, Price).
        
- **Output:**
    
    - Texto de resposta (ex: "Encontrei 3 eventos...").
        
    - Lista de Cards de Eventos filtrados.
        
- **Limites:** 3 créditos de IA gratuitos (simulado).
    

---

## 9. Dados e Modelos de Dados

### 9.1 Entidade: Event (Evento)

- id: string
    
- title: string
    
- date: string
    
- time: string
    
- location: string
    
- category: string (Congresso, Workshop, etc)
    
- price: number
    
- image: URL string
    
- organizerId: string
    
- isNetworkingEnabled: boolean
    
- ticketTiers: Array<TicketTier> </TicketTier>
    
- schedule: Array<{time, title, speaker}>
    
- materials: Array<{type, url, title}>
    

### 9.2 Entidade: Ticket (Ingresso Emitido)

- id: string
    
- eventId: string
    
- holderName: string
    
- qrCodeData: string (Hash único)
    
- status: 'valid' | 'used' | 'expired'
    

### 9.3 Entidade: NetworkingProfile

- id: string
    
- name: string
    
- role: string
    
- company: string
    
- bio: string
    
- tags: Array<string></string>
    

---

## 10. Regras de Negócio

1. **Acesso ao Networking:** O botão de networking de um evento só fica ativo (isOpen = true) se a data atual for >= (Data do Evento - 24 horas). Antes disso, exibe status "Bloqueado".
    
2. **Troca de Perfil:** Um Organizador pode acessar a visão de Participante sem relogar. Um Participante precisa "Ativar" a conta de organizador para acessar o dashboard.
    
3. **Limite de Tags:** No setup de networking, o usuário pode selecionar estritamente no máximo 5 tags.
    
4. **Validação de Check-in:** Um ingresso com status 'used' não pode ser validado novamente.
    
5. **Exibição de Ingressos:** Na carteira, ingressos de eventos passados vão automaticamente para a aba "Histórico".
    

---

## 11. Estados do Sistema

- **Guest (Visitante):** Não autenticado. Vê Home, Detalhes.
    
- **Logged In (Attendee):** Autenticado. Acesso total às features de compra e networking.
    
- **Logged In (Organizer):** Autenticado. Acesso ao Dashboard e ferramentas de gestão.
    
- **Networking Setup:** Estado intermediário onde o usuário precisa configurar o perfil antes de ver o discovery.
    

---

## 12. Integrações (Simuladas no Protótipo)

- **Mapas:** Botões para abrir Google Maps e Waze com a localização do evento.
    
- **Compartilhamento:** API nativa do navegador (navigator.share) para compartilhar link do evento.
    
- **Calendário:** (Previsto) Adicionar evento à agenda nativa.
    

---

## 13. Segurança

- **QR Code:** O código gerado deve ser um hash único, não apenas o ID do ingresso, para evitar falsificação simples.
    
- **Blockers:** Componente AuthRequiredBlocker protege rotas privadas (Checkout, Networking) de usuários não logados.
    
- **Privacidade:** O chat só é liberado após "Match" mútuo (Double Opt-in), exceto para perfis "Premium" (Open DM - Regra de negócio implícita no mock data).
    

---

## 14. Configurações e Parametrizações

- **Cores:** Definidas no tailwind.config.
    
    - Primary: Lime (#C9E567)
        
    - Secondary: Deep Purple (#503A88)
        
    - Background: Soft White (#f6f6f6)
        
- **Tags de Networking:** Lista fixa predefinida no código (FIXED_TAGS), mas expansível via busca.
    

---

## 15. UX e Design System

### 15.1 Estilo Visual

- **Moderno e Clean:** Uso extensivo de espaço em branco (padding), bordas arredondadas (rounded-2xl, rounded-3xl).
    
- **Feedback:** Botões mudam de estado (active:scale-95). Skeletons ou Spinners (Loader2) durante carregamento.
    
- **Sombras:** Suaves e coloridas (shadow-secondary/20) para dar profundidade.
    

### 15.2 Componentes Chave

- **EventCard:** O componente visual mais importante. Deve ser rico em informações mas não poluído. Uso de gradientes sobre a imagem para garantir legibilidade do texto.
    
- **Bottom Navigation:** Flutuante, com blur (vidro fosco), ícones mudam de estilo (outline vs filled) e label aparece apenas no ativo.
    

---

## 16. Estrutura de Arquivos (Project Structure)

codeCode

```
/
├── index.html          # Entry point, Tailwind CDN, Fonts
├── index.tsx           # React entry point
├── App.tsx             # Main Router & Global State Controller
├── types.ts            # TypeScript Interfaces
├── services/
│   └── mockData.ts     # Mock database (Events, Users, Tickets)
├── components/
│   ├── Common.tsx      # Buttons, Inputs, Header, Cards
│   └── Layout.tsx      # App wrapper with Bottom Nav
└── views/
    ├── Auth.tsx        # Login, Register, Splash
    ├── Organizer.tsx   # Dashboard, Create Event, Scanner
    ├── Attendee.tsx    # Barrel file for exports
    └── attendee/
        ├── HomeView.tsx        # Search, Feed, Alerts
        ├── EventView.tsx       # Event Details
        ├── WalletView.tsx      # Tickets List & Detail
        ├── NetworkingView.tsx  # Setup, Swipe, Chat
        ├── CheckoutView.tsx    # Payment flow
        ├── ProfileView.tsx     # User settings
        └── SharedAttendee.tsx  # EventCard, Helpers
```

---

## 17. Glossário

- **Batch:** Lote de ingressos (ex: Lote Promocional, 1º Lote).
    
- **Match:** Quando dois usuários demonstram interesse mútuo no networking.
    
- **Tier:** Tipo/Nível do ingresso dentro de um lote (ex: VIP, Standard).
    
- **Organizer:** O criador do evento.
    
- **Attendee:** O participante do evento.
    
- **PWA:** Progressive Web App.
    

---

**Fim da Especificação.** Este documento contém todas as regras, layouts e lógicas necessárias para a implementação do unnu.



A partir dessa descrição completa do projeto, responda com detalhes todas dúvidas que tenho sobre o desenvolvimento completo desse projeto do inicio ao fim.

Dúvidas:
- Quais ferramentas para um único desenvolvedor produzir esse App do inicio ao fim 
- Como fazer o deploy desse App, front, back, dados, tudo 
- Quanto tempo levaria para fazer esse app até o deploy
- Quais detalhes de back faltam ser decididos para que o App funcione
- Como escalar, qual arquitetura utilizar para não ter erros no futuro 