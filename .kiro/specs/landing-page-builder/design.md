# Documento de Design - Landing Page Builder

## Visão Geral

O Landing Page Builder é uma aplicação web SaaS que permite usuários não-técnicos criarem landing pages profissionais através de um editor visual drag-and-drop. A arquitetura segue o padrão cliente-servidor com frontend React, backend Node.js/Express e banco de dados PostgreSQL. O sistema prioriza simplicidade, performance e escalabilidade para suportar milhares de usuários simultâneos.

## Arquitetura

### Arquitetura Geral

```
┌─────────────────┐
│   Navegador     │
│   (Visitante)   │
└────────┬────────┘
         │ HTTPS
         ▼
┌─────────────────┐
│   CDN/Cache     │
│  (CloudFlare)   │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│          Frontend (Vercel)              │
│  React + Tailwind + React Router        │
│  - Editor Visual                        │
│  - Painel de Controle                   │
│  - Páginas Públicas                     │
└────────┬────────────────────────────────┘
         │ REST API
         ▼
┌─────────────────────────────────────────┐
│       Backend (Railway/Render)          │
│     Node.js + Express + JWT             │
│  - API REST                             │
│  - Autenticação                         │
│  - Gerenciamento de Landing Pages       │
│  - Analytics                            │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│      PostgreSQL Database                │
│  - Usuários                             │
│  - Landing Pages                        │
│  - Templates                            │
│  - Analytics                            │
└─────────────────────────────────────────┘
```

### Componentes Principais

**Frontend (React)**
- Editor visual com preview em tempo real
- Painel administrativo para gerenciar landing pages
- Sistema de autenticação com JWT
- Renderizador de landing pages públicas

**Backend (Node.js/Express)**
- API RESTful para CRUD de landing pages
- Sistema de autenticação e autorização
- Processamento e otimização de imagens
- Coleta e agregação de analytics
- Gerenciamento de planos e limites

**Banco de Dados (PostgreSQL)**
- Armazenamento de usuários e credenciais
- Persistência de landing pages e configurações
- Templates pré-definidos
- Dados de analytics e métricas

**Infraestrutura**
- CloudFlare para CDN e cache de páginas públicas
- Vercel para hospedagem do frontend
- Railway/Render para hospedagem do backend
- AWS S3 ou CloudFlare R2 para armazenamento de imagens

## Componentes e Interfaces

### Frontend Components

#### 1. Editor Component
```typescript
interface EditorProps {
  landingPageId: string;
  onSave: (data: LandingPageData) => Promise<void>;
  onPublish: () => Promise<void>;
}

interface Block {
  id: string;
  type: 'title' | 'subtitle' | 'image' | 'button' | 'testimonials' | 'footer';
  content: Record<string, any>;
  styles: CSSProperties;
}
```

#### 2. Template Gallery Component
```typescript
interface TemplateGalleryProps {
  templates: Template[];
  onSelectTemplate: (templateId: string) => void;
}

interface Template {
  id: string;
  name: string;
  category: 'sales' | 'scheduling' | 'catalog' | 'linktree';
  thumbnail: string;
  blocks: Block[];
  isPremium: boolean;
}
```

#### 3. Dashboard Component
```typescript
interface DashboardProps {
  user: User;
  landingPages: LandingPage[];
  onEdit: (id: string) => void;
  onDuplicate: (id: string) => void;
  onDelete: (id: string) => void;
}
```

#### 4. Analytics Component
```typescript
interface AnalyticsProps {
  landingPageId: string;
  dateRange?: { start: Date; end: Date };
}

interface AnalyticsData {
  totalViews: number;
  totalClicks: number;
  clicksByButton: Record<string, number>;
  viewsByDate?: Record<string, number>;
}
```

### Backend API Endpoints

#### Authentication
```
POST   /api/auth/register    - Criar nova conta
POST   /api/auth/login       - Autenticar usuário
POST   /api/auth/logout      - Encerrar sessão
GET    /api/auth/me          - Obter usuário atual
```

#### Landing Pages
```
GET    /api/landing-pages              - Listar landing pages do usuário
POST   /api/landing-pages              - Criar nova landing page
GET    /api/landing-pages/:id          - Obter landing page específica
PUT    /api/landing-pages/:id          - Atualizar landing page
DELETE /api/landing-pages/:id          - Excluir landing page
POST   /api/landing-pages/:id/publish  - Publicar landing page
POST   /api/landing-pages/:id/duplicate - Duplicar landing page
```

#### Templates
```
GET    /api/templates         - Listar templates disponíveis
GET    /api/templates/:id     - Obter template específico
```

#### Analytics
```
GET    /api/analytics/:landingPageId   - Obter analytics de uma landing page
POST   /api/analytics/track-view       - Registrar visualização
POST   /api/analytics/track-click      - Registrar clique
```

#### Public Routes
```
GET    /:username/p/:slug     - Renderizar landing page pública
```

## Modelos de Dados

### User
```typescript
interface User {
  id: string;
  email: string;
  passwordHash: string;
  username: string;
  plan: 'basic' | 'pro' | 'elite';
  createdAt: Date;
  updatedAt: Date;
}
```

### LandingPage
```typescript
interface LandingPage {
  id: string;
  userId: string;
  title: string;
  slug: string;
  templateId: string;
  blocks: Block[];
  customDomain?: string;
  isPublished: boolean;
  publishedAt?: Date;
  createdAt: Date;
  updatedAt: Date;
}
```

### Template
```typescript
interface Template {
  id: string;
  name: string;
  category: 'sales' | 'scheduling' | 'catalog' | 'linktree';
  thumbnail: string;
  blocks: Block[];
  isPremium: boolean;
  createdAt: Date;
}
```

### Analytics
```typescript
interface AnalyticsEvent {
  id: string;
  landingPageId: string;
  eventType: 'view' | 'click';
  buttonId?: string;
  timestamp: Date;
  userAgent?: string;
  ipAddress?: string;
}
```

### Block (Detailed)
```typescript
type BlockType = 'title' | 'subtitle' | 'image' | 'button' | 'testimonials' | 'footer';

interface BaseBlock {
  id: string;
  type: BlockType;
  order: number;
}

interface TitleBlock extends BaseBlock {
  type: 'title';
  content: {
    text: string;
    alignment: 'left' | 'center' | 'right';
  };
  styles: {
    fontSize: string;
    color: string;
    fontWeight: string;
  };
}

interface ImageBlock extends BaseBlock {
  type: 'image';
  content: {
    url: string;
    alt: string;
  };
  styles: {
    width: string;
    height: string;
    objectFit: 'cover' | 'contain';
  };
}

interface ButtonBlock extends BaseBlock {
  type: 'button';
  content: {
    text: string;
    link: string;
    paymentProvider?: 'mercadopago' | 'stripe' | 'hotmart';
  };
  styles: {
    backgroundColor: string;
    textColor: string;
    borderRadius: string;
  };
}

type Block = TitleBlock | ImageBlock | ButtonBlock | /* outros tipos */;
```


## Propriedades de Correção

*Uma propriedade é uma característica ou comportamento que deve ser verdadeiro em todas as execuções válidas de um sistema - essencialmente, uma declaração formal sobre o que o sistema deve fazer. As propriedades servem como ponte entre especificações legíveis por humanos e garantias de correção verificáveis por máquina.*

### Propriedade 1: Criação de landing page a partir de template preserva estrutura
*Para qualquer* template válido, quando um usuário cria uma landing page baseada naquele template, a landing page resultante deve conter todos os blocos do template original com o mesmo tipo e ordem.
**Valida: Requisitos 1.2**

### Propriedade 2: Slugs são únicos por usuário
*Para qualquer* usuário, todas as landing pages criadas por aquele usuário devem ter slugs únicos - não deve existir duas landing pages do mesmo usuário com o mesmo slug.
**Valida: Requisitos 1.5**

### Propriedade 3: Persistência round trip de landing pages
*Para qualquer* landing page válida, salvar a landing page e depois recuperá-la do banco de dados deve resultar em uma landing page equivalente com todos os blocos, estilos e configurações preservados.
**Valida: Requisitos 3.1**

### Propriedade 4: URL de landing page publicada segue formato correto
*Para qualquer* landing page publicada, a URL gerada deve seguir o formato `{username}.seusite.com/p/{slug}` onde username é o nome do usuário proprietário e slug é o identificador único da landing page.
**Valida: Requisitos 3.2**

### Propriedade 5: Landing page publicada contém todos os blocos
*Para qualquer* landing page publicada, quando um visitante acessa a URL pública, o conteúdo renderizado deve incluir todos os blocos configurados pelo usuário na mesma ordem.
**Valida: Requisitos 3.4**

### Propriedade 6: Edição de landing page publicada atualiza versão pública
*Para qualquer* landing page publicada, após editar e salvar mudanças, acessar a URL pública deve retornar o conteúdo atualizado refletindo as mudanças mais recentes.
**Valida: Requisitos 3.5**

### Propriedade 7: Botão de pagamento aceita links válidos
*Para qualquer* botão de pagamento, o sistema deve aceitar e armazenar corretamente links de Mercado Pago, Stripe ou Hotmart quando configurados.
**Valida: Requisitos 4.1**

### Propriedade 8: Botão de pagamento redireciona para link configurado
*Para qualquer* botão de pagamento com link configurado, quando um visitante clica no botão, o sistema deve redirecionar para exatamente o link que foi configurado pelo usuário.
**Valida: Requisitos 4.2**

### Propriedade 9: Validação de link vazio em botão de pagamento
*Para qualquer* tentativa de configurar um botão de pagamento, se o link fornecido for uma string vazia ou contiver apenas espaços em branco, o sistema deve rejeitar a configuração.
**Valida: Requisitos 4.3**

### Propriedade 10: Múltiplos botões de pagamento por plano
*Para qualquer* usuário com plano Pro ou Elite, o sistema deve permitir adicionar múltiplos botões de pagamento na mesma landing page, enquanto usuários com plano Basic devem ser limitados a um único botão.
**Valida: Requisitos 4.4**

### Propriedade 11: Registro de visualizações incrementa contador
*Para qualquer* landing page publicada, cada acesso de visitante deve incrementar o contador de visualizações nos analytics em exatamente 1.
**Valida: Requisitos 5.1**

### Propriedade 12: Registro de cliques incrementa contador
*Para qualquer* botão em uma landing page publicada, cada clique de visitante deve incrementar o contador de cliques daquele botão específico nos analytics em exatamente 1.
**Valida: Requisitos 5.2**

### Propriedade 13: Analytics retorna total correto de visualizações
*Para qualquer* landing page com N eventos de visualização registrados, a função de analytics deve retornar exatamente N como o total de visualizações.
**Valida: Requisitos 5.3**

### Propriedade 14: Analytics retorna total correto de cliques
*Para qualquer* landing page com M eventos de clique registrados, a função de analytics deve retornar exatamente M como o total de cliques.
**Valida: Requisitos 5.4**

### Propriedade 15: Analytics detalhados por plano
*Para qualquer* usuário com plano Pro ou Elite, a função de analytics deve retornar dados detalhados por período de tempo, enquanto usuários com plano Basic devem receber apenas totais agregados.
**Valida: Requisitos 5.5**

### Propriedade 16: Limite de landing pages por plano Basic
*Para qualquer* usuário com plano Basic, o sistema deve permitir criar exatamente 1 landing page e deve rejeitar tentativas de criar uma segunda landing page.
**Valida: Requisitos 6.2**

### Propriedade 17: Limite de landing pages por plano Pro
*Para qualquer* usuário com plano Pro, o sistema deve permitir criar até 5 landing pages e deve rejeitar tentativas de criar a sexta landing page.
**Valida: Requisitos 6.3**

### Propriedade 18: Landing pages ilimitadas para plano Elite
*Para qualquer* usuário com plano Elite, o sistema deve permitir criar qualquer número de landing pages sem impor limite.
**Valida: Requisitos 6.4**

### Propriedade 19: Erro ao exceder limite do plano
*Para qualquer* usuário que tenta criar uma landing page além do limite do seu plano, o sistema deve retornar um erro indicando que o limite foi atingido e upgrade é necessário.
**Valida: Requisitos 6.5**

### Propriedade 20: Remoção de marca por plano
*Para qualquer* landing page publicada, se o usuário proprietário possui plano Pro ou Elite, a página renderizada não deve conter a marca do sistema; se o usuário possui plano Basic, a marca deve estar presente.
**Valida: Requisitos 6.6**

### Propriedade 21: Listagem completa de landing pages do usuário
*Para qualquer* usuário com N landing pages criadas, a função de listagem deve retornar exatamente N landing pages, todas pertencentes àquele usuário.
**Valida: Requisitos 7.1**

### Propriedade 22: Duplicação preserva conteúdo com novo slug
*Para qualquer* landing page, quando duplicada, a cópia deve conter todos os blocos e configurações da original, mas deve ter um slug diferente e único.
**Valida: Requisitos 7.3**

### Propriedade 23: Exclusão torna URL inacessível
*Para qualquer* landing page publicada, após ser excluída, tentar acessar sua URL pública deve resultar em erro 404 (não encontrado).
**Valida: Requisitos 7.4**

### Propriedade 24: Configuração de domínio personalizado por plano
*Para qualquer* usuário com plano Elite, o sistema deve permitir configurar domínio personalizado para landing pages; usuários com plano Basic ou Pro devem ter essa funcionalidade bloqueada.
**Valida: Requisitos 8.1**

### Propriedade 25: Landing page acessível via domínio personalizado
*Para qualquer* landing page com domínio personalizado configurado corretamente, acessar aquele domínio deve retornar a landing page correta com todo seu conteúdo.
**Valida: Requisitos 8.3**

### Propriedade 26: Autenticação com credenciais válidas retorna token
*Para qualquer* usuário registrado, fornecer email e senha corretos deve resultar em autenticação bem-sucedida e retorno de token JWT válido.
**Valida: Requisitos 9.2**

### Propriedade 27: Autenticação com credenciais inválidas falha
*Para qualquer* tentativa de autenticação com email inexistente ou senha incorreta, o sistema deve rejeitar a autenticação e retornar erro apropriado.
**Valida: Requisitos 9.3**

### Propriedade 28: Validação de email único no registro
*Para qualquer* tentativa de criar nova conta, se o email já existe no sistema, o registro deve ser rejeitado; se o email é único, o registro deve ser bem-sucedido.
**Valida: Requisitos 9.5**

### Propriedade 29: Validação de senha forte no registro
*Para qualquer* tentativa de criar nova conta, se a senha tem menos de 8 caracteres ou não contém pelo menos uma letra e um número, o registro deve ser rejeitado.
**Valida: Requisitos 9.5**

### Propriedade 30: Otimização de imagens reduz tamanho
*Para qualquer* imagem enviada ao sistema, após processamento e otimização, o tamanho do arquivo resultante deve ser menor ou igual ao tamanho original.
**Valida: Requisitos 10.5**

## Tratamento de Erros

### Estratégia Geral
O sistema implementa tratamento de erros em múltiplas camadas:

1. **Validação no Frontend**: Validação imediata de inputs do usuário antes de enviar ao backend
2. **Validação no Backend**: Validação rigorosa de todos os dados recebidos via API
3. **Tratamento de Exceções**: Try-catch em operações críticas (banco de dados, I/O)
4. **Logging**: Registro de todos os erros para debugging e monitoramento
5. **Mensagens Amigáveis**: Erros técnicos são traduzidos para mensagens compreensíveis ao usuário

### Categorias de Erros

#### Erros de Validação (400 Bad Request)
```typescript
{
  error: 'VALIDATION_ERROR',
  message: 'Descrição amigável do erro',
  fields: {
    fieldName: 'Mensagem específica do campo'
  }
}
```

Exemplos:
- Email inválido ou já cadastrado
- Senha fraca (menos de 8 caracteres)
- Slug vazio ou com caracteres inválidos
- Link de pagamento vazio
- Imagem muito grande (> 5MB)

#### Erros de Autenticação (401 Unauthorized)
```typescript
{
  error: 'AUTHENTICATION_ERROR',
  message: 'Credenciais inválidas ou token expirado'
}
```

Exemplos:
- Credenciais incorretas no login
- Token JWT expirado ou inválido
- Tentativa de acesso sem autenticação

#### Erros de Autorização (403 Forbidden)
```typescript
{
  error: 'AUTHORIZATION_ERROR',
  message: 'Você não tem permissão para esta ação'
}
```

Exemplos:
- Tentar editar landing page de outro usuário
- Tentar acessar funcionalidade premium sem plano adequado
- Exceder limite de landing pages do plano

#### Erros de Recurso Não Encontrado (404 Not Found)
```typescript
{
  error: 'NOT_FOUND',
  message: 'Recurso não encontrado'
}
```

Exemplos:
- Landing page inexistente
- Template não encontrado
- URL pública de página excluída

#### Erros de Limite de Plano (403 Forbidden)
```typescript
{
  error: 'PLAN_LIMIT_EXCEEDED',
  message: 'Você atingiu o limite do seu plano',
  currentPlan: 'basic',
  limit: 1,
  upgradeUrl: '/upgrade'
}
```

Exemplos:
- Tentar criar segunda landing page no plano Basic
- Tentar criar sexta landing page no plano Pro
- Tentar configurar domínio personalizado sem plano Elite

#### Erros de Servidor (500 Internal Server Error)
```typescript
{
  error: 'INTERNAL_ERROR',
  message: 'Erro interno do servidor. Tente novamente mais tarde.',
  requestId: 'uuid-para-rastreamento'
}
```

Exemplos:
- Falha na conexão com banco de dados
- Erro ao processar imagem
- Falha ao enviar email

### Tratamento Específico por Componente

#### Editor
- Salvar automaticamente a cada 30 segundos para evitar perda de dados
- Mostrar indicador visual de "salvando..." e "salvo"
- Em caso de erro ao salvar, manter dados no localStorage e permitir retry
- Validar blocos antes de salvar (ex: URLs válidas em botões)

#### Upload de Imagens
- Validar tipo de arquivo (apenas JPEG, PNG, WebP)
- Validar tamanho máximo (5MB)
- Mostrar progresso do upload
- Em caso de falha, permitir retry ou cancelamento
- Comprimir imagens automaticamente no frontend antes do upload

#### Publicação
- Validar que landing page tem pelo menos um bloco
- Validar que slug é único antes de publicar
- Em caso de erro, manter landing page em modo draft
- Mostrar preview antes de publicar

#### Analytics
- Falhas ao registrar eventos não devem bloquear a experiência do visitante
- Usar fire-and-forget para tracking (não esperar resposta)
- Buffer de eventos em caso de falha temporária de rede

## Estratégia de Testes

### Visão Geral
O sistema utiliza uma abordagem dupla de testes: testes unitários para casos específicos e edge cases, e testes baseados em propriedades para verificar correção universal.

### Testes Unitários

**Objetivo**: Verificar comportamentos específicos, casos extremos e integrações entre componentes.

**Ferramentas**:
- Frontend: Vitest + React Testing Library
- Backend: Jest + Supertest

**Cobertura**:
- Componentes React individuais (Editor, Dashboard, TemplateGallery)
- Funções utilitárias (geração de slug, validação de URL, formatação)
- Endpoints da API (casos de sucesso e erro)
- Middlewares de autenticação e autorização
- Casos extremos: strings vazias, valores nulos, arrays vazios
- Integração entre camadas (controller → service → repository)

**Exemplos de Testes Unitários**:
```typescript
// Teste de validação de senha
test('deve rejeitar senha com menos de 8 caracteres', () => {
  expect(validatePassword('abc123')).toBe(false);
});

// Teste de geração de slug
test('deve gerar slug único a partir do título', () => {
  const slug1 = generateSlug('Minha Landing Page');
  const slug2 = generateSlug('Minha Landing Page');
  expect(slug1).not.toBe(slug2);
});

// Teste de endpoint de criação
test('POST /api/landing-pages deve criar landing page', async () => {
  const response = await request(app)
    .post('/api/landing-pages')
    .set('Authorization', `Bearer ${token}`)
    .send({ templateId: 'template-1', title: 'Test' });
  
  expect(response.status).toBe(201);
  expect(response.body).toHaveProperty('id');
});
```

### Testes Baseados em Propriedades

**Objetivo**: Verificar que propriedades universais se mantêm verdadeiras para todos os inputs válidos.

**Ferramenta**: fast-check (JavaScript/TypeScript)

**Configuração**: Cada teste de propriedade deve executar no mínimo 100 iterações com inputs aleatórios.

**Requisitos de Implementação**:
- Cada propriedade de correção do design deve ser implementada como UM ÚNICO teste baseado em propriedade
- Cada teste deve incluir comentário explícito referenciando a propriedade: `// Feature: landing-page-builder, Property X: [texto da propriedade]`
- Testes devem ser colocados próximos à implementação para detectar erros cedo

**Exemplos de Testes de Propriedade**:

```typescript
// Feature: landing-page-builder, Property 3: Persistência round trip de landing pages
test('salvar e recuperar landing page preserva todos os dados', () => {
  fc.assert(
    fc.property(
      landingPageArbitrary, // gerador de landing pages aleatórias
      async (landingPage) => {
        const saved = await repository.save(landingPage);
        const retrieved = await repository.findById(saved.id);
        
        expect(retrieved).toEqual(saved);
        expect(retrieved.blocks).toHaveLength(saved.blocks.length);
        expect(retrieved.blocks).toEqual(saved.blocks);
      }
    ),
    { numRuns: 100 }
  );
});

// Feature: landing-page-builder, Property 2: Slugs são únicos por usuário
test('todas as landing pages de um usuário têm slugs únicos', () => {
  fc.assert(
    fc.property(
      fc.array(landingPageArbitrary, { minLength: 2, maxLength: 10 }),
      async (landingPages) => {
        const userId = 'test-user';
        const created = await Promise.all(
          landingPages.map(lp => service.create({ ...lp, userId }))
        );
        
        const slugs = created.map(lp => lp.slug);
        const uniqueSlugs = new Set(slugs);
        
        expect(uniqueSlugs.size).toBe(slugs.length);
      }
    ),
    { numRuns: 100 }
  );
});

// Feature: landing-page-builder, Property 11: Registro de visualizações incrementa contador
test('cada visualização incrementa contador em exatamente 1', () => {
  fc.assert(
    fc.property(
      fc.uuid(), // landing page ID aleatório
      fc.integer({ min: 1, max: 50 }), // número de visualizações
      async (landingPageId, numViews) => {
        const initialCount = await analytics.getViewCount(landingPageId);
        
        for (let i = 0; i < numViews; i++) {
          await analytics.trackView(landingPageId);
        }
        
        const finalCount = await analytics.getViewCount(landingPageId);
        expect(finalCount).toBe(initialCount + numViews);
      }
    ),
    { numRuns: 100 }
  );
});
```

**Geradores Personalizados (Arbitraries)**:
```typescript
// Gerador de blocos aleatórios
const blockArbitrary = fc.oneof(
  fc.record({
    id: fc.uuid(),
    type: fc.constant('title'),
    content: fc.record({ text: fc.string() }),
    styles: fc.record({ fontSize: fc.string(), color: fc.hexaString() })
  }),
  fc.record({
    id: fc.uuid(),
    type: fc.constant('button'),
    content: fc.record({ text: fc.string(), link: fc.webUrl() }),
    styles: fc.record({ backgroundColor: fc.hexaString() })
  })
  // ... outros tipos de blocos
);

// Gerador de landing pages aleatórias
const landingPageArbitrary = fc.record({
  title: fc.string({ minLength: 1, maxLength: 100 }),
  templateId: fc.uuid(),
  blocks: fc.array(blockArbitrary, { minLength: 1, maxLength: 20 }),
  isPublished: fc.boolean()
});

// Gerador de usuários aleatórios
const userArbitrary = fc.record({
  email: fc.emailAddress(),
  password: fc.string({ minLength: 8, maxLength: 50 }),
  username: fc.string({ minLength: 3, maxLength: 30 }),
  plan: fc.oneof(
    fc.constant('basic'),
    fc.constant('pro'),
    fc.constant('elite')
  )
});
```

### Testes de Integração

**Objetivo**: Verificar que o sistema funciona corretamente end-to-end.

**Escopo**:
- Fluxo completo de criação de landing page (template → editor → publicação)
- Fluxo de autenticação (registro → login → acesso protegido)
- Fluxo de analytics (visita → tracking → visualização de dados)
- Integração com banco de dados real (usando container Docker para testes)

### Estratégia de Execução

1. **Desenvolvimento**: Executar testes unitários e de propriedade a cada mudança
2. **Pre-commit**: Executar todos os testes unitários e de propriedade
3. **CI/CD**: Executar suite completa incluindo testes de integração
4. **Cobertura**: Manter cobertura mínima de 80% para código crítico

### Priorização de Testes

**Alta Prioridade** (devem sempre passar):
- Autenticação e autorização
- Persistência de dados (round trip)
- Limites de plano
- Geração de URLs únicas

**Média Prioridade**:
- Validações de input
- Analytics
- Duplicação e exclusão

**Baixa Prioridade**:
- Otimização de imagens
- Formatação de UI
- Casos extremos raros

## Considerações de Segurança

### Autenticação e Autorização
- Senhas armazenadas com bcrypt (salt rounds: 12)
- Tokens JWT com expiração de 7 dias
- Refresh tokens para renovação automática
- Rate limiting em endpoints de autenticação (5 tentativas por minuto)

### Proteção de Dados
- Validação rigorosa de todos os inputs
- Sanitização de HTML em blocos de texto para prevenir XSS
- Proteção contra SQL injection usando queries parametrizadas
- CORS configurado para permitir apenas domínios autorizados

### Isolamento de Dados
- Usuários só podem acessar suas próprias landing pages
- Middleware de autorização em todos os endpoints protegidos
- Validação de ownership antes de qualquer operação CRUD

### Upload de Arquivos
- Validação de tipo MIME real (não apenas extensão)
- Limite de tamanho de arquivo (5MB)
- Armazenamento em bucket isolado com URLs assinadas
- Scan de malware em arquivos enviados (opcional, para produção)

### Rate Limiting
- API pública (landing pages): 100 requisições/minuto por IP
- API autenticada: 1000 requisições/minuto por usuário
- Tracking de analytics: 10 eventos/segundo por landing page

## Considerações de Performance

### Frontend
- Code splitting por rota (React.lazy)
- Lazy loading de imagens
- Debounce em auto-save do editor (500ms)
- Virtualização de listas longas (react-window)
- Cache de templates no localStorage

### Backend
- Índices no banco de dados:
  - `users(email)` - único
  - `landing_pages(user_id, slug)` - único composto
  - `landing_pages(is_published)` - para queries de páginas públicas
  - `analytics_events(landing_page_id, timestamp)` - para agregações
- Connection pooling no PostgreSQL (max: 20 conexões)
- Cache de landing pages publicadas no Redis (TTL: 5 minutos)
- Paginação em listagens (20 itens por página)

### Otimização de Imagens
- Redimensionamento automático para múltiplos tamanhos (thumbnail, medium, large)
- Conversão para WebP quando suportado
- Compressão com qualidade 85%
- CDN para servir imagens estáticas

### Monitoramento
- Logging estruturado com Winston
- Métricas de performance com Prometheus
- Alertas para:
  - Tempo de resposta > 2 segundos
  - Taxa de erro > 1%
  - Uso de CPU > 80%
  - Uso de memória > 85%

## Escalabilidade

### Arquitetura Stateless
- Backend completamente stateless (sessões via JWT)
- Permite escalonamento horizontal fácil
- Load balancer distribui requisições entre múltiplas instâncias

### Separação de Responsabilidades
- Processamento de imagens em worker separado (queue com Bull)
- Analytics agregados em batch job noturno
- Emails enviados de forma assíncrona

### Limites e Quotas
- Implementação de rate limiting por usuário e por IP
- Soft limits com avisos antes de atingir hard limits
- Monitoramento de uso para identificar abuso

### Plano de Crescimento
1. **0-1000 usuários**: Arquitetura monolítica, single server
2. **1000-10000 usuários**: Separar frontend e backend, adicionar Redis
3. **10000+ usuários**: Múltiplas instâncias backend, CDN, workers separados
4. **100000+ usuários**: Microserviços, sharding de banco de dados
