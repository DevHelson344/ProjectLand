# Plano de Implementação - Landing Page Builder

- [ ] 1. Configurar estrutura do projeto e dependências
  - Criar estrutura de pastas para frontend (React + Vite) e backend (Node.js + Express)
  - Instalar dependências: React, Tailwind, Express, PostgreSQL client, JWT, bcrypt
  - Configurar TypeScript em ambos os projetos
  - Configurar Vitest para frontend e Jest para backend
  - Instalar fast-check para testes baseados em propriedades
  - Criar arquivos de configuração (.env, tsconfig.json, vite.config.ts)
  - _Requisitos: Todos_

- [ ] 2. Implementar modelos de dados e esquema do banco
  - Criar schema SQL para tabelas: users, landing_pages, templates, analytics_events
  - Implementar interfaces TypeScript para User, LandingPage, Template, Block, AnalyticsEvent
  - Criar migrations para setup inicial do banco de dados
  - Adicionar índices para performance (user_id, slug, email)
  - _Requisitos: 1.1, 3.1, 5.1, 9.5_

- [ ] 3. Implementar sistema de autenticação
- [ ] 3.1 Criar serviço de autenticação no backend
  - Implementar registro de usuário com hash de senha (bcrypt)
  - Implementar login com geração de JWT
  - Criar middleware de autenticação para validar tokens
  - Implementar validação de email único e senha forte
  - _Requisitos: 9.2, 9.3, 9.5_

- [ ] 3.2 Escrever teste de propriedade para autenticação válida
  - **Propriedade 26: Autenticação com credenciais válidas retorna token**
  - **Valida: Requisitos 9.2**

- [ ] 3.3 Escrever teste de propriedade para autenticação inválida
  - **Propriedade 27: Autenticação com credenciais inválidas falha**
  - **Valida: Requisitos 9.3**

- [ ] 3.4 Escrever teste de propriedade para validação de email único
  - **Propriedade 28: Validação de email único no registro**
  - **Valida: Requisitos 9.5**

- [ ] 3.5 Escrever teste de propriedade para validação de senha forte
  - **Propriedade 29: Validação de senha forte no registro**
  - **Valida: Requisitos 9.5**

- [ ] 3.6 Criar componentes de UI para login e registro
  - Implementar formulário de login com validação
  - Implementar formulário de registro com validação
  - Adicionar gerenciamento de estado de autenticação (Context API)
  - Implementar redirecionamento após login/logout
  - _Requisitos: 9.1, 9.2, 9.3, 9.4, 9.5_

- [ ] 4. Implementar sistema de templates
- [ ] 4.1 Criar templates pré-definidos no banco de dados
  - Criar template de página de vendas com blocos padrão
  - Criar template de página de agendamento
  - Criar template de catálogo simples
  - Criar template de página de link único (estilo Linktree)
  - Adicionar thumbnails e metadados para cada template
  - _Requisitos: 1.1, 1.4_

- [ ] 4.2 Implementar API de templates
  - Criar endpoint GET /api/templates para listar templates
  - Criar endpoint GET /api/templates/:id para obter template específico
  - Filtrar templates premium baseado no plano do usuário
  - _Requisitos: 1.1, 1.3_

- [ ] 4.3 Criar componente de galeria de templates
  - Implementar grid responsivo de templates
  - Adicionar preview visual de cada template
  - Implementar seleção de template
  - Adicionar badge "Premium" para templates pagos
  - _Requisitos: 1.1, 1.3_

- [ ] 5. Implementar criação e gerenciamento de landing pages
- [ ] 5.1 Criar serviço de landing pages no backend
  - Implementar função de criação de landing page a partir de template
  - Implementar geração automática de slug único
  - Implementar funções CRUD (create, read, update, delete)
  - Adicionar validação de ownership (usuário só acessa suas páginas)
  - Implementar verificação de limites por plano
  - _Requisitos: 1.2, 1.5, 3.1, 6.2, 6.3, 6.4, 6.5, 7.1, 7.3, 7.4_

- [ ] 5.2 Escrever teste de propriedade para criação a partir de template
  - **Propriedade 1: Criação de landing page a partir de template preserva estrutura**
  - **Valida: Requisitos 1.2**

- [ ] 5.3 Escrever teste de propriedade para slugs únicos
  - **Propriedade 2: Slugs são únicos por usuário**
  - **Valida: Requisitos 1.5**

- [ ] 5.4 Escrever teste de propriedade para persistência round trip
  - **Propriedade 3: Persistência round trip de landing pages**
  - **Valida: Requisitos 3.1**

- [ ] 5.5 Escrever teste de propriedade para limite plano Basic
  - **Propriedade 16: Limite de landing pages por plano Basic**
  - **Valida: Requisitos 6.2**

- [ ] 5.6 Escrever teste de propriedade para limite plano Pro
  - **Propriedade 17: Limite de landing pages por plano Pro**
  - **Valida: Requisitos 6.3**

- [ ] 5.7 Escrever teste de propriedade para landing pages ilimitadas Elite
  - **Propriedade 18: Landing pages ilimitadas para plano Elite**
  - **Valida: Requisitos 6.4**

- [ ] 5.8 Escrever teste de propriedade para erro ao exceder limite
  - **Propriedade 19: Erro ao exceder limite do plano**
  - **Valida: Requisitos 6.5**

- [ ] 5.9 Implementar API de landing pages
  - Criar endpoint POST /api/landing-pages (criar)
  - Criar endpoint GET /api/landing-pages (listar do usuário)
  - Criar endpoint GET /api/landing-pages/:id (obter específica)
  - Criar endpoint PUT /api/landing-pages/:id (atualizar)
  - Criar endpoint DELETE /api/landing-pages/:id (excluir)
  - Criar endpoint POST /api/landing-pages/:id/duplicate (duplicar)
  - Adicionar middleware de autenticação e autorização
  - _Requisitos: 1.2, 3.1, 7.1, 7.3, 7.4_

- [ ] 5.10 Escrever teste de propriedade para listagem completa
  - **Propriedade 21: Listagem completa de landing pages do usuário**
  - **Valida: Requisitos 7.1**

- [ ] 5.11 Escrever teste de propriedade para duplicação
  - **Propriedade 22: Duplicação preserva conteúdo com novo slug**
  - **Valida: Requisitos 7.3**

- [ ] 5.12 Escrever teste de propriedade para exclusão
  - **Propriedade 23: Exclusão torna URL inacessível**
  - **Valida: Requisitos 7.4**

- [ ] 6. Implementar editor visual de landing pages
- [ ] 6.1 Criar componente de editor com preview
  - Implementar área de edição com lista de blocos
  - Implementar preview em tempo real ao lado
  - Adicionar barra de ferramentas com opções de salvar/publicar
  - Implementar auto-save a cada 30 segundos
  - _Requisitos: 2.1, 3.1_

- [ ] 6.2 Implementar componentes de blocos editáveis
  - Criar componente TitleBlock com edição inline
  - Criar componente SubtitleBlock com edição inline
  - Criar componente ImageBlock com upload
  - Criar componente ButtonBlock com edição de texto e link
  - Criar componente TestimonialsBlock
  - Criar componente FooterBlock
  - Adicionar validação em cada tipo de bloco
  - _Requisitos: 2.2, 2.3, 2.4, 2.6_

- [ ] 6.3 Implementar sistema de cores e estilos
  - Criar seletor de cores para personalização
  - Implementar aplicação de estilos em tempo real
  - Adicionar presets de cores por template
  - _Requisitos: 2.5_

- [ ] 6.4 Implementar funcionalidade de salvar e publicar
  - Criar função de salvar que persiste no backend
  - Implementar indicador visual de "salvando..." e "salvo"
  - Criar função de publicar que marca landing page como pública
  - Adicionar validação antes de publicar (mínimo 1 bloco)
  - _Requisitos: 3.1, 3.2_

- [ ] 6.5 Escrever testes unitários para componentes do editor
  - Testar renderização de cada tipo de bloco
  - Testar edição inline de texto
  - Testar upload de imagem
  - Testar aplicação de estilos
  - _Requisitos: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6_

- [ ] 7. Implementar sistema de publicação e URLs públicas
- [ ] 7.1 Criar serviço de publicação no backend
  - Implementar função de publicar landing page
  - Implementar geração de URL no formato {username}.seusite.com/p/{slug}
  - Adicionar timestamp de publicação
  - Implementar atualização de versão pública ao salvar
  - _Requisitos: 3.2, 3.5_

- [ ] 7.2 Escrever teste de propriedade para formato de URL
  - **Propriedade 4: URL de landing page publicada segue formato correto**
  - **Valida: Requisitos 3.2**

- [ ] 7.3 Escrever teste de propriedade para conteúdo publicado
  - **Propriedade 5: Landing page publicada contém todos os blocos**
  - **Valida: Requisitos 3.4**

- [ ] 7.4 Escrever teste de propriedade para atualização de versão pública
  - **Propriedade 6: Edição de landing page publicada atualiza versão pública**
  - **Valida: Requisitos 3.5**

- [ ] 7.5 Criar endpoint público para renderizar landing pages
  - Criar rota GET /:username/p/:slug (sem autenticação)
  - Implementar renderização server-side ou retorno de dados para client-side
  - Adicionar cache para performance
  - Retornar 404 para páginas não publicadas ou excluídas
  - _Requisitos: 3.4, 7.4_

- [ ] 7.6 Criar componente de renderização pública
  - Implementar renderizador de blocos para visitantes
  - Adicionar layout responsivo (mobile, tablet, desktop)
  - Implementar carregamento otimizado de imagens
  - Adicionar ou remover marca do sistema baseado no plano
  - _Requisitos: 3.4, 6.6, 10.2, 10.3, 10.4_

- [ ] 7.7 Escrever teste de propriedade para remoção de marca por plano
  - **Propriedade 20: Remoção de marca por plano**
  - **Valida: Requisitos 6.6**

- [ ] 8. Implementar sistema de botões de pagamento
- [ ] 8.1 Adicionar suporte a botões de pagamento no editor
  - Estender ButtonBlock para incluir tipo "payment"
  - Adicionar campos para provider (Mercado Pago, Stripe, Hotmart)
  - Implementar validação de link não vazio
  - Adicionar verificação de limite de botões por plano
  - _Requisitos: 4.1, 4.3, 4.4_

- [ ] 8.2 Escrever teste de propriedade para aceitação de links válidos
  - **Propriedade 7: Botão de pagamento aceita links válidos**
  - **Valida: Requisitos 4.1**

- [ ] 8.3 Escrever teste de propriedade para validação de link vazio
  - **Propriedade 9: Validação de link vazio em botão de pagamento**
  - **Valida: Requisitos 4.3**

- [ ] 8.4 Escrever teste de propriedade para múltiplos botões por plano
  - **Propriedade 10: Múltiplos botões de pagamento por plano**
  - **Valida: Requisitos 4.4**

- [ ] 8.5 Implementar redirecionamento de botões de pagamento
  - Adicionar handler de clique em botões de pagamento
  - Implementar redirecionamento para link configurado
  - Adicionar tracking de clique antes do redirecionamento
  - _Requisitos: 4.2, 5.2_

- [ ] 8.6 Escrever teste de propriedade para redirecionamento correto
  - **Propriedade 8: Botão de pagamento redireciona para link configurado**
  - **Valida: Requisitos 4.2**

- [ ] 9. Implementar sistema de analytics
- [ ] 9.1 Criar serviço de analytics no backend
  - Implementar função de registrar visualização
  - Implementar função de registrar clique em botão
  - Implementar função de agregar dados (total de views, total de clicks)
  - Implementar filtro por período para planos Pro/Elite
  - Adicionar índices no banco para queries eficientes
  - _Requisitos: 5.1, 5.2, 5.3, 5.4, 5.5_

- [ ] 9.2 Escrever teste de propriedade para registro de visualizações
  - **Propriedade 11: Registro de visualizações incrementa contador**
  - **Valida: Requisitos 5.1**

- [ ] 9.3 Escrever teste de propriedade para registro de cliques
  - **Propriedade 12: Registro de cliques incrementa contador**
  - **Valida: Requisitos 5.2**

- [ ] 9.4 Escrever teste de propriedade para total de visualizações
  - **Propriedade 13: Analytics retorna total correto de visualizações**
  - **Valida: Requisitos 5.3**

- [ ] 9.5 Escrever teste de propriedade para total de cliques
  - **Propriedade 14: Analytics retorna total correto de cliques**
  - **Valida: Requisitos 5.4**

- [ ] 9.6 Escrever teste de propriedade para analytics detalhados por plano
  - **Propriedade 15: Analytics detalhados por plano**
  - **Valida: Requisitos 5.5**

- [ ] 9.7 Implementar API de analytics
  - Criar endpoint GET /api/analytics/:landingPageId
  - Criar endpoint POST /api/analytics/track-view (público)
  - Criar endpoint POST /api/analytics/track-click (público)
  - Adicionar rate limiting para prevenir abuso
  - _Requisitos: 5.1, 5.2, 5.3, 5.4, 5.5_

- [ ] 9.8 Integrar tracking nas landing pages públicas
  - Adicionar script de tracking ao renderizador público
  - Implementar tracking de view ao carregar página
  - Implementar tracking de click em botões
  - Usar fire-and-forget para não bloquear UX
  - _Requisitos: 5.1, 5.2_

- [ ] 9.9 Criar componente de dashboard de analytics
  - Implementar visualização de total de visitas
  - Implementar visualização de total de cliques
  - Adicionar gráfico de visitas por dia (Pro/Elite)
  - Adicionar breakdown de cliques por botão
  - _Requisitos: 5.3, 5.4, 5.5_

- [ ] 10. Implementar dashboard de gerenciamento
- [ ] 10.1 Criar componente de listagem de landing pages
  - Implementar grid/lista de landing pages do usuário
  - Adicionar thumbnail de preview para cada página
  - Mostrar status (publicada/draft), data de criação, visitas
  - Implementar paginação se necessário
  - _Requisitos: 7.1_

- [ ] 10.2 Adicionar ações de gerenciamento
  - Implementar botão de editar (abre editor)
  - Implementar botão de visualizar (abre URL pública)
  - Implementar botão de duplicar com confirmação
  - Implementar botão de excluir com confirmação
  - Adicionar feedback visual para cada ação
  - _Requisitos: 7.2, 7.3, 7.4, 7.5_

- [ ] 10.3 Criar componente de informações do plano
  - Mostrar plano atual do usuário
  - Mostrar limite de landing pages e uso atual
  - Adicionar botão de upgrade para planos superiores
  - Mostrar features disponíveis por plano
  - _Requisitos: 6.1, 6.2, 6.3, 6.4, 6.5_

- [ ] 10.4 Escrever testes unitários para componentes do dashboard
  - Testar renderização da lista de landing pages
  - Testar ações de editar, duplicar, excluir
  - Testar exibição de informações do plano
  - _Requisitos: 7.1, 7.2, 7.3, 7.4_

- [ ] 11. Implementar sistema de upload e otimização de imagens
- [ ] 11.1 Criar serviço de upload no backend
  - Implementar endpoint POST /api/upload para upload de imagens
  - Adicionar validação de tipo MIME (JPEG, PNG, WebP)
  - Adicionar validação de tamanho máximo (5MB)
  - Implementar geração de nome único para arquivo
  - Configurar armazenamento (filesystem local ou S3/R2)
  - _Requisitos: 2.3, 10.5_

- [ ] 11.2 Implementar otimização de imagens
  - Adicionar biblioteca de processamento (sharp)
  - Implementar compressão automática (qualidade 85%)
  - Implementar redimensionamento para múltiplos tamanhos
  - Implementar conversão para WebP quando possível
  - _Requisitos: 10.5_

- [ ] 11.3 Escrever teste de propriedade para otimização de imagens
  - **Propriedade 30: Otimização de imagens reduz tamanho**
  - **Valida: Requisitos 10.5**

- [ ] 11.4 Criar componente de upload no frontend
  - Implementar drag-and-drop de imagens
  - Adicionar preview antes do upload
  - Mostrar progresso do upload
  - Implementar retry em caso de falha
  - Adicionar compressão client-side antes do upload
  - _Requisitos: 2.3_

- [ ] 12. Implementar sistema de domínio personalizado (Plano Elite)
- [ ] 12.1 Adicionar suporte a domínio personalizado no backend
  - Adicionar campo customDomain na tabela landing_pages
  - Implementar validação de domínio (formato válido)
  - Implementar verificação de plano Elite antes de permitir
  - Criar endpoint PUT /api/landing-pages/:id/custom-domain
  - _Requisitos: 8.1_

- [ ] 12.2 Escrever teste de propriedade para configuração de domínio por plano
  - **Propriedade 24: Configuração de domínio personalizado por plano**
  - **Valida: Requisitos 8.1**

- [ ] 12.3 Implementar roteamento para domínios personalizados
  - Adicionar middleware para detectar domínio personalizado
  - Implementar lookup de landing page por domínio
  - Servir landing page correta quando acessada via domínio personalizado
  - _Requisitos: 8.3_

- [ ] 12.4 Escrever teste de propriedade para acesso via domínio personalizado
  - **Propriedade 25: Landing page acessível via domínio personalizado**
  - **Valida: Requisitos 8.3**

- [ ] 12.5 Criar UI para configuração de domínio
  - Adicionar campo de input para domínio personalizado (apenas Elite)
  - Mostrar instruções de configuração DNS
  - Adicionar verificação de status do domínio
  - Mostrar mensagem de upgrade para usuários não-Elite
  - _Requisitos: 8.1, 8.2_

- [ ] 13. Implementar responsividade e otimizações de performance
- [ ] 13.1 Otimizar renderização de landing pages públicas
  - Implementar lazy loading de imagens
  - Adicionar tags meta para SEO
  - Implementar cache de páginas publicadas
  - Otimizar CSS (remover não utilizado)
  - _Requisitos: 10.1, 10.2, 10.3, 10.4, 10.5_

- [ ] 13.2 Implementar code splitting no frontend
  - Configurar React.lazy para rotas
  - Implementar loading states
  - Otimizar bundle size
  - _Requisitos: 10.1_

- [ ] 13.3 Adicionar rate limiting e segurança
  - Implementar rate limiting em endpoints de autenticação
  - Implementar rate limiting em endpoints públicos
  - Adicionar sanitização de HTML em blocos de texto
  - Configurar CORS adequadamente
  - Adicionar headers de segurança (helmet)
  - _Requisitos: Todos (segurança geral)_

- [ ] 13.4 Escrever testes de integração end-to-end
  - Testar fluxo completo: registro → criar landing page → publicar
  - Testar fluxo de autenticação completo
  - Testar fluxo de analytics: visita → tracking → visualização
  - _Requisitos: Todos_

- [ ] 14. Checkpoint final - Garantir que todos os testes passam
  - Executar suite completa de testes unitários
  - Executar suite completa de testes de propriedade
  - Executar testes de integração
  - Verificar cobertura de código
  - Corrigir quaisquer testes falhando
  - Garantir que todas as propriedades de correção estão validadas
  - Perguntar ao usuário se há dúvidas ou problemas
