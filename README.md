# LocalStack Web

Uma aplicação frontend moderna desenvolvida com **Vue.js 3** e **Vuetify 3** que fornece uma interface web intuitiva para gerenciar e visualizar recursos do LocalStack (Não oficial).

![LocalStack Web](https://img.shields.io/badge/Vue.js-3-green) ![Vuetify](https://img.shields.io/badge/Vuetify-3-blue) ![LocalStack](https://img.shields.io/badge/LocalStack-Compatible-orange) ![Docker pulls](https://img.shields.io/docker/pulls/dantasrafael/localstack-web)

## 🚀 Funcionalidades

### 📊 Dashboard Principal
- ✅ Visão geral de todos os serviços LocalStack ativos
- ✅ Status em tempo real de conectividade
- ✅ Estatísticas resumidas de cada serviço
- ✅ Ações rápidas para limpeza de recursos
- ✅ Métricas de uso e performance
- ✅ Alertas de saúde dos serviços

### 🪣 S3 (Simple Storage Service)
- ✅ Listar todos os buckets com paginação
- ✅ Visualizar estatísticas detalhadas (número de objetos, tamanho total, custo simulado)
- ✅ Criar novos buckets com configurações avançadas (versionamento, lifecycle)
- ✅ Deletar buckets (com limpeza automática de objetos)
- ✅ Navegar dentro dos buckets com estrutura de pastas
- ✅ Upload de arquivos (individual e em lote)
- ✅ Download e exclusão de objetos
- ✅ Busca e filtros avançados (por tipo, tamanho, data)
- ✅ Pré-visualização de arquivos (imagens, texto, JSON)
- ✅ Configuração de políticas de bucket
- ✅ Gerenciamento de ACLs (Access Control Lists)

### 📝 SQS (Simple Queue Service)
- ✅ Listar todas as filas com filtros
- ✅ Visualizar estatísticas em tempo real (mensagens disponíveis, em processamento, DLQ)
- ✅ Criar novas filas com configurações personalizadas (FIFO, delay, retention)
- ✅ Deletar filas com confirmação
- ✅ Enviar mensagens para filas (individual e em lote)
- ✅ Receber e visualizar mensagens com formatação JSON
- ✅ Deletar mensagens individuais ou múltiplas
- ✅ Purgar filas completamente
- ✅ Configurar Dead Letter Queues (DLQ)
- ✅ Monitoramento de métricas de fila

### 🗃️ DynamoDB
- ✅ Listar todas as tabelas com status
- ✅ Visualizar estatísticas detalhadas (número de itens, tamanho, índices)
- ✅ Criar tabelas com chaves primárias e índices secundários
- ✅ Deletar tabelas com confirmação
- ✅ Visualizar itens da tabela com paginação
- ✅ Adicionar, editar e deletar itens (via JSON e formulário)
- ✅ Suporte para todos os tipos de dados DynamoDB
- ✅ Query e Scan com filtros avançados
- ✅ Backup e restore de dados
- ✅ Gerenciamento de índices GSI/LSI

### ⚡ Lambda Functions
- ✅ Listar todas as funções com filtros
- ✅ Visualizar detalhes, configurações e versões
- ✅ Invocar funções com payload customizado
- ✅ Visualizar resultados, logs e métricas de execução
- ✅ Deletar funções e versões
- ✅ Suporte para invocação síncrona e assíncrona
- ✅ Configurar triggers e event sources
- ✅ Gerenciar aliases e configurações de concorrência
- ✅ Monitor de execuções e erros

### 🌊 Kinesis Streams
- ✅ Listar todos os streams com status
- ✅ Visualizar detalhes, shards e métricas
- ✅ Criar novos streams com configuração de shards
- ✅ Deletar streams com confirmação
- ✅ Enviar records para streams (individual e em lote)
- ✅ Ler records de streams com iteradores
- ✅ Visualizar dados decodificados em múltiplos formatos
- ✅ Monitoramento de throughput e latência
- ✅ Gerenciar resharding automático

### 📧 SNS (Simple Notification Service)
- ✅ Listar todos os tópicos
- ✅ Criar e deletar tópicos
- ✅ Gerenciar assinatures (subscriptions)
- ✅ Publicar mensagens em tópicos
- ✅ Configurar filtros de mensagens
- ✅ Visualizar histórico de notificações

### 🔑 KMS (Key Management Service)
- ✅ Listar todas as chaves de criptografia
- ✅ Criar novas chaves (simétricas e assimétricas)
- ✅ Gerenciar aliases de chaves
- ✅ Criptografar e descriptografar dados
- ✅ Gerar data keys e random data
- ✅ Configurar políticas de chaves
- ✅ Rotação automática de chaves
- ✅ Importar material de chaves externas
- ✅ Visualizar uso e métricas de chaves

### 📨 SES (Simple Email Service)
- ✅ Gerenciar identidades de email
- ✅ Enviar emails de teste
- ✅ Visualizar estatísticas de envio
- ✅ Configurar templates de email
- ✅ Monitor de bounces e complaints

### 🔒 Secrets Manager
- ✅ Listar todos os secrets
- ✅ Criar novos secrets via texto puro (plaintext) ou chave-valor (JSON)
- ✅ Deletar os secrets
- ✅ Visualizar o conteúdo dos secrets via texto puro (plaintext) ou chave-valor (JSON)

### 🎨 Interface do Usuário
- ✅ Design moderno e responsivo com Material Design 3
- ✅ Tema claro/escuro com persistência
- ✅ Feedback visual para todas as ações
- ✅ Busca e filtros em tempo real
- ✅ Notificações toast personalizadas
- ✅ Configuração dinâmica do endpoint LocalStack
- ✅ Atalhos de teclado para ações rápidas
- ✅ Suporte para múltiplos idiomas (i18n)
- ✅ Modo offline com cache local
- ✅ Exportação de dados (JSON, CSV)


## 🛠️ Tecnologias Utilizadas

- **Vue.js 3** - Framework frontend reativo
- **Vuetify 3** - Biblioteca de componentes Material Design
- **Vite** - Build tool e dev server
- **Pinia** - Gerenciamento de estado
- **Vue Router** - Roteamento SPA
- **AWS SDK for JavaScript** - Integração com LocalStack
- **Docker** - Containerização
- **Nginx** - Servidor web para produção

## 📋 Pré-requisitos

- Node.js 18+
- npm ou yarn
- Docker e Docker Compose (para execução com containers)

## 🚀 Instalação e Execução

### Opção 1: Execução Local (Desenvolvimento)

1. **Clone o repositório**
```bash
git clone <url-do-repositorio>
cd localstack-web
```

2. **Instale as dependências**
```bash
npm install
```

3. **Execute o LocalStack separadamente**
```bash
# Em um terminal separado
docker run --rm -it -p 4566:4566 localstack/localstack
```

4. **Execute a aplicação em modo desenvolvimento**
```bash
npm run dev
```

5. **Acesse a aplicação**
```
http://localhost:3000
```

### Opção 2: Execução com Docker Compose (Recomendado)

1. **Clone o repositório**
```bash
git clone <url-do-repositorio>
cd localstack-web
```

2. **Execute com Docker Compose**
```bash
docker-compose up --build
```

3. **Acesse a aplicação**
```
http://localhost:3000
```

O LocalStack estará disponível em `http://localhost:4566`

## ⚙️ Configuração

### Endpoint do LocalStack

A aplicação permite configurar dinamicamente o endpoint do LocalStack através da interface:

1. Clique no ícone de configurações na barra lateral
2. Insira o endpoint desejado (ex: `http://localhost:4566`)
3. Salve as configurações

### Variáveis de Ambiente

Você pode configurar o endpoint através de variáveis de ambiente no `docker-compose.yml`:

```yaml
environment:
  - LOCALSTACK_ENDPOINT=http://localstack:4566
```

## 🐳 Docker

### Build da Imagem
```bash
docker build -t localstack-web .
```

### Execução da Imagem
```bash
docker run -p 3000:80 localstack-web
```

## 🧪 Desenvolvimento

### Scripts Disponíveis

```bash
# Desenvolvimento
npm run dev

# Build para produção
npm run build

# Preview da build
npm run preview

# Linting
npm run lint

# Formatação de código
npm run format
```

### Estrutura do Projeto

```
src/
├── components/ # Componentes reutilizáveis
├── views/      # Páginas/Views da aplicação
├── router/     # Configuração de rotas
├── stores/     # Gerenciamento de estado (Pinia)
├── App.vue     # Componente raiz
└── main.js     # Ponto de entrada
```

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Casos de Uso

### Para Desenvolvedores
- Visualizar rapidamente o estado dos recursos durante desenvolvimento
- Testar integrações com serviços AWS de forma local
- Limpar ambientes entre testes
- Debugar problemas com dados de teste

### Para QA/Testers
- Criar cenários de teste com dados específicos
- Verificar comportamento de aplicações em diferentes estados
- Limpar e preparar ambientes de teste
- Validar integrações com serviços AWS

### Para DevOps
- Monitorar recursos em ambientes de desenvolvimento
- Automatizar limpeza de ambientes
- Configurar dados iniciais para testes
- Verificar configurações de serviços

## 🚨 Troubleshooting

### Problema de Conectividade
- Verifique se o LocalStack está rodando na porta 4566
- Confirme se o endpoint está configurado corretamente
- Verifique se não há problemas de CORS

### Performance
- Use o modo de desenvolvimento do LocalStack para melhor performance
- Considere configurar persistence se precisar manter dados entre reinicializações

### Problemas com Docker
- Certifique-se de que o Docker está rodando
- Verifique se as portas 3000 e 4566 estão disponíveis
- Use `docker-compose logs` para debug

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## 🙏 Agradecimentos

- [LocalStack](https://localstack.cloud/) - Por fornecer uma excelente ferramenta para desenvolvimento local
- [Vue.js](https://vuejs.org/) - Framework frontend incrível
- [Vuetify](https://vuetifyjs.com/) - Componentes Material Design para Vue
- [AWS SDK](https://aws.amazon.com/sdk-for-javascript/) - SDK oficial da AWS

---

⭐ Se este projeto foi útil para você, considere dar uma estrela no repositório!
