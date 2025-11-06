# Projeto DevOps Fase 1
Este projeto foi desenvolvido como parte da disciplina de **DevOps**, com o objetivo de demonstrar na prática conceitos de **Integração Contínua (CI)**, **Infraestrutura como Código (IaC)** e **automação de pipelines** utilizando GitHub Actions.

A aplicação é propositalmente simples — uma API em **Node.js** utilizando **Express**, que retorna uma mensagem `"hello world"`. O foco principal do projeto é a automação do ciclo de build, teste e provisionamento de infraestrutura.

## 🚀 Tecnologias
- Node.js
- Express.js
- Jest (testes)
- Supertest (testes de API)
- GitHub Actions (CI/CD)
- Terraform (Infraestrutura como Código)
- AWS S3 (Armazenamento)

## 📦 Instalação
```bash
npm install
```

## ▶️ Executar a API
```bash
npm start
```

A API estará disponível em `http://localhost:3000`

## 🧪 Executar os Testes
```bash
npm test
```

Para executar os testes em modo watch:

```bash
npm run test:watch
```

## 📍 Endpoints

### GET /
Retorna uma mensagem "Hello World"

**Resposta:**
```json
{
  "message": "Hello World"
}
```

### GET /health
Endpoint de health check

**Resposta:**
```json
{
  "status": "OK"
}
```

## 📊 Cobertura de Testes

Os testes incluem:
- Verificação do status HTTP 200
- Validação do conteúdo da resposta
- Verificação do Content-Type (JSON)
- Health check endpoint

Execute `npm test` para ver o relatório de cobertura.

## 🔄 Pipeline de CI/CD

O projeto utiliza **GitHub Actions** para automação do pipeline de CI. O workflow é executado automaticamente em:
- Push para a branch `main`
- **Todos os Pull Requests** (independentemente da branch)

### Jobs do Pipeline

#### 1. **Build e Testes** (`build-and-test`)
- Executa em múltiplas versões do Node.js (18.x e 20.x)
- Instala as dependências
- Executa os testes unitários
- Gera relatório de cobertura
- Faz upload do relatório de cobertura como artefato

#### 2. **Verificação de Qualidade** (`lint`)
- Verifica a estrutura do projeto
- Valida a presença de arquivos essenciais

#### 3. **Auditoria de Segurança** (`security`)
- Executa `npm audit` para identificar vulnerabilidades
- Verifica dependências com problemas de segurança

#### 4. **Status Final** (`build-status`)
- Consolida o resultado de todos os jobs
- Indica sucesso ou falha do pipeline

### Badge de Status

Após fazer push para o GitHub, você pode adicionar um badge de status ao README:

```markdown
![CI Pipeline](https://github.com/SEU_USUARIO/projeto-devops-fase-1/actions/workflows/ci.yml/badge.svg)
```

### Arquivo do Workflow

O workflow está localizado em: `.github/workflows/ci.yml`

## ☁️ Infraestrutura como Código (Terraform)

O projeto utiliza **Terraform** para provisionar automaticamente recursos na AWS.

### Recursos Provisionados

- **S3 Bucket** para armazenar artefatos da aplicação
  - Versionamento habilitado
  - Criptografia AES256
  - Bloqueio de acesso público (segurança)

### Workflow de Infraestrutura

O arquivo `.github/workflows/terraform.yml` automatiza:
- ✅ `terraform init` - Inicialização
- ✅ `terraform validate` - Validação
- ✅ `terraform plan` - Planejamento (em PRs)
- ✅ `terraform apply` - Aplicação automática (push na main)

### Como Usar

#### Pré-requisitos
1. Conta AWS
2. Configurar secrets no GitHub:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`

#### Uso Local

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

#### Uso Automático

O Terraform é executado automaticamente via GitHub Actions:
- **Push na main** → Aplica mudanças automaticamente
- **Pull Request** → Mostra o plano de mudanças
- **Manual** → Via workflow_dispatch