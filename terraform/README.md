# Infraestrutura com Terraform

Este diretório contém os arquivos Terraform para provisionar a infraestrutura AWS.

## 📦 Recursos Provisionados

- **S3 Bucket**: Para armazenar artefatos da aplicação
  - Versionamento habilitado
  - Criptografia AES256
  - Bloqueio de acesso público

## 🚀 Como Usar

### Localmente

```bash
# Inicializar Terraform
terraform init

# Validar configuração
terraform validate

# Planejar mudanças
terraform plan

# Aplicar mudanças
terraform apply

# Destruir infraestrutura
terraform destroy
```

### Com GitHub Actions

O workflow `.github/workflows/terraform.yml` automatiza o processo:

1. **Push na main**: Aplica automaticamente as mudanças na infraestrutura
2. **Pull Request**: Executa apenas `plan` para revisar mudanças
3. **Manual**: Pode ser executado via workflow_dispatch

## 🔐 Configuração de Secrets

Configure no GitHub: Settings → Secrets and variables → Actions

```
AWS_ACCESS_KEY_ID: sua-access-key
AWS_SECRET_ACCESS_KEY: sua-secret-key
```

## 📋 Variáveis

Edite `variables.tf` ou crie `terraform.tfvars`:

```hcl
aws_region   = "us-east-2"
project_name = "devops-fase-01"
environment  = "dev"
```

