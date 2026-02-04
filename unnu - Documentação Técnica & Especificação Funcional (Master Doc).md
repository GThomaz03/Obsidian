**Versão:** 2.0.0 (Pivot PWA & Foco B2B2C)  
**Status:** Definição de Escopo  
**Plataforma:** **PWA (Progressive Web App)** - Mobile First, acessível via Browser e instalável.

---

## 1. Visão do Produto e Estratégia

### 1.1. O "Soul" do Negócio

O **unnu** não é apenas uma bilheteria; é uma **Plataforma de Gestão de Comunidades e Networking** para eventos de médio porte (Imersões, Congressos, Masterminds).

- **Diferencial Competitivo:** Enquanto Sympla/Eventbrite focam na venda, o unnu foca na **conexão** entre os participantes e no ambiente de negócios gerado pelo evento.
    
- **Público-Alvo (Organizadores):** "Pequenos Gigantes" — mentores, infoprodutores, organizadores de imersões financeiras/tech/consórcios. Não atendemos shows/festivais de massa.
    

### 1.2. O Conceito Híbrido

- **App Único:** Uma única URL/PWA atende dois perfis com alternância imediata (Toggle Switch).
    
- **Identidade:** Conta unificada, mas dados de perfil segregados (O perfil "Baladeiro" não mistura com o "Profissional").
    

---

## 2. Especificação Funcional Detalhada

### 2.1. Acesso e Onboarding

1. **Modo Visitante (Guest):**
    
    - Usuário pode navegar, usar a busca IA e ver detalhes de eventos sem login.
        
    - **Blockers:** Ao tentar Comprar, Favoritar ou acessar Networking, o modal de Login/Cadastro é acionado.
        
2. **Login/Cadastro:**
    
    - Autenticação via E-mail/Senha, Google e Apple (Obrigatório).
        
    - Dados Mínimos: A definir com base na API do Gateway (Guru), mas provavelmente Nome, CPF, E-mail e Telefone.
        
3. **Gestão de Perfil (Switch):**
    
    - Alternância entre Modo Participante e Modo Organizador.
        
    - Configurações independentes (Notificações de um perfil não afetam o outro).
        

### 2.2. O Produto "Evento"

1. **Tipologia:**
    
    - Presencial ou Online (Link de transmissão protegido).
        
    - Gratuito ou Pago.
        
    - Recorrência: Em análise (Feature Backlog).
        
2. **Bilhetagem:**
    
    - **Tiers:** Ingressos Padrão, VIP, Meet & Greet.
        
    - **Cupons:** Descontos percentuais ou fixos.
        
    - **Revenda/Transferência:** Não haverá reembolso tradicional (cancelamento). Haverá funcionalidade de **Transferência de Titularidade** ou **Revenda no Marketplace** (Regras a definir).
        
3. **Ciclo de Vida (Pós-Evento):**
    
    - Após o término, a página do ingresso se transforma em **Hub de Conteúdo**.
        
    - **Entregáveis:** Certificado de Participação (PDF gerado auto), Links de gravações, Materiais de apoio (PDFs), Cupons de parceiros.
        

### 2.3. Inteligência Artificial (Discovery)

1. **Busca Semântica (Chat Search):**
    
    - Input: "Quero aprender sobre Bolsa de Valores em SP".
        
    - Processamento: LLM traduz para filtros (Categoria: Finanças, Local: SP, Tipo: Imersão).
        
    - Output: Lista de eventos compatíveis.
        
2. **Auditor IA (Backend):**
    
    - Agente de IA que analisa novos eventos criados em busca de palavras-chave proibidas ou indícios de fraude/fake antes ou logo após a publicação.
        

### 2.4. Networking & Social (Core Feature)

1. **Mecânica de Match:**
    
    - **Opt-in Default:** O usuário entra automaticamente na lista de networking ao comprar, mas pode desativar nas configurações ("Modo Invisível").
        
    - **Filtros de Segurança:** Opção "Aparecer apenas para mulheres" (Safety feature).
        
    - **Bloqueio:** Possibilidade de bloquear usuários específicos (desfaz o match e some da lista).
        
2. **Chat Efêmero:**
    
    - Chat P2P liberado após Match mútuo.
        
    - **TTL (Time-to-Live):** Histórico é deletado automaticamente 24h após o fim do evento.
        
    - **Sem Backup:** O app não permite exportar conversas, forçando os usuários a trocarem contatos reais (WhatsApp/LinkedIn) se quiserem manter a relação.
        

### 2.5. Financeiro (Guru + Asaas)

1. **Fluxo de Recebimento:**
    
    - Integração com **Digital Guru** (Checkout/Gateway).
        
    - Processamento via **Asaas** (Infra bancária).
        
2. **Split de Pagamento:**
    
    - O Organizador possui uma "Subconta" (ou conta conectada) no Asaas.
        
    - **Taxa (Fee):** 10% adicionado ao valor final (Outside Fee).
        
        - Ex: Ingresso R
            
            ```
            100,00−>OrganizadorrecebeR100,00−>OrganizadorrecebeR
            ```
            
             100,00 -> Usuário paga R$ 110,00.
            
    - O Split direciona os R
        
        ```
        10,00paraounnueosR10,00paraounnueosR
        ```
        
         100,00 para o organizador na fonte.
        

### 2.6. Scanner & Validação (Offline First)

1. **Tecnologia:** Uso da câmera via Browser (HTML5 Media Capture) otimizado para PWA.
    
2. **Fluxo Offline:**
    
    - O Organizador baixa a "Lista de Hashs Válidos" ao abrir o scanner (enquanto tem internet).
        
    - Se a internet cair, o Scanner valida matematicamente o QR Code contra a lista local.
        
    - Sincronização (Sync) ocorre automaticamente quando a conexão retornar.
        
3. **Validação Manual:** Busca por Nome/CPF caso o celular do participante acabe a bateria.
    

---

## 3. Arquitetura Técnica (PWA Stack)

Como mudamos para PWA, a stack muda para garantir performance web com "sentimento" de app.

- **Frontend:** React.js (Vite ou Next.js).
    
    - Por que: Melhor suporte a SEO, compartilhamento de links e performance PWA.
        
- **Service Workers:** Workbox (Google) para cache de assets e funcionamento offline da Carteira e Scanner.
    
- **Armazenamento Local:** IndexedDB (para guardar a lista de participantes offline e os ingressos na carteira).
    
- **Backend:** Node.js (NestJS) + PostgreSQL.
    
- **Infra:** Vercel (Front) + AWS/Render (Back).
    

---

## 4. Matriz de Riscos e Definições Pendentes (To-Do List)

Estas são as perguntas que ficaram em aberto ou precisam de detalhamento técnico urgente nas próximas etapas.

|   |   |   |   |
|---|---|---|---|
|ID|Área|Dúvida / Definição Necessária|Status|
|**01**|**Finanças**|**Chargeback:** Se o usuário cancelar a compra no cartão, quem paga a multa do Asaas? O unnu ou descontamos do saldo futuro do organizador?|🔴 Crítico|
|**02**|**Dados**|**Campos Obrigatórios:** Definir exatamente quais campos o **Guru** exige para aprovar uma transação antifraude (Endereço? CPF?). Isso impacta o design do cadastro.|🔴 Crítico|
|**03**|**Bilheteria**|**Regras de Transferência:** Até quanto tempo antes do evento posso transferir o ingresso? (Ex: até 4h antes?). Isso evita cambismo de última hora.|🟡 Médio|
|**04**|**Push**|**Limites de Notificação:** Quantos Pushes manuais o organizador pode enviar por evento? (Evitar spam).|🟡 Médio|
|**05**|**Jurídico**|**Termos de Uso:** O "Agente Auditor IA" pode banir eventos? Precisamos de termos claros que permitam a remoção de eventos sem aviso prévio.|🟡 Médio|
|**06**|**PWA**|**Limitação iOS:** No iPhone, PWA não tem acesso a Push Notifications nativas se não for "instalado" na Home Screen (versões antigas do iOS). Estratégia: Usar E-mail/SMS como fallback?|🟡 Técnico|

---

## 5. Próximos Passos (Roadmap Sugerido)

1. **Fase 1 (Fundação):** Design System Mobile-First + Configuração do PWA (React) + Auth (Login/Cadastro).
    
2. **Fase 2 (Core SaaS):** Criação de Evento (Simples) + Integração Asaas/Guru (Subcontas).
    
3. **Fase 3 (Core User):** Checkout + Carteira (Offline) + Scanner Web.
    
4. **Fase 4 (Social):** Networking + Chat + Algoritmo de Match.
    
5. **Fase 5 (AI & Polish):** Busca Inteligente + Auditor IA + Hub Pós-Evento.
    

---

### O que você deve fazer agora:

Como investidor e PM, sugiro focarmos imediatamente no **Item 01 e 02 da Matriz de Riscos**.

Sem saber as regras do **Guru/Asaas** (campos obrigatórios e chargeback), não conseguimos desenhar o fluxo de cadastro nem o contrato financeiro.

**Posso gerar um roteiro de perguntas técnicas para você enviar ao suporte do Guru/Asaas?**