- **Vídeo 12: Integrando Terraform com CI/CD**
  - Como usar Terraform em pipelines de integração contínua (CI) e entrega contínua (CD).
  - Exemplos com **GitLab CI**, **GitHub Actions**, **Jenkins**.

### **Vídeo 12: Integrando Terraform com CI/CD**

No décimo segundo vídeo da série, vamos abordar como integrar o **Terraform** com **pipelines de CI/CD** (Integração Contínua e Entrega Contínua) para automatizar o provisionamento e a gestão da infraestrutura. O objetivo é garantir que as mudanças na infraestrutura sejam aplicadas de forma controlada e segura, sempre que houver alterações no código, sem a necessidade de intervenção manual.

#### **Como Usar Terraform em Pipelines de Integração Contínua (CI) e Entrega Contínua (CD)**

**CI/CD** são práticas essenciais no desenvolvimento de software moderno, e quando combinadas com ferramentas de **Infraestrutura como Código (IaC)**, como o Terraform, elas ajudam a automatizar o ciclo de vida da infraestrutura. A integração do Terraform com **CI/CD** permite que você crie, atualize e destrua recursos de infraestrutura de forma automatizada e segura, sempre que houver uma alteração no código.

1. **Integração Contínua (CI)**: Trata-se do processo de integração frequente de código em um repositório compartilhado, com a execução de testes automatizados para garantir que o código esteja funcionando corretamente. No contexto do Terraform, a CI pode ser configurada para validar as configurações da infraestrutura (por exemplo, rodar `terraform plan` para verificar possíveis mudanças).
   
2. **Entrega Contínua (CD)**: O CD automatiza a entrega de alterações para um ambiente de produção ou pré-produção. Com o Terraform, isso implica em aplicar as mudanças na infraestrutura de maneira automatizada e sem intervenção manual, como rodar o `terraform apply` após uma aprovação ou a conclusão de testes.

3. **Benefícios de Usar Terraform em CI/CD**:
   - **Automação**: Reduz a intervenção manual ao aplicar mudanças de infraestrutura, acelerando os ciclos de deploy.
   - **Segurança**: Minimiza o risco de erros humanos e inconsistências ao aplicar as configurações automaticamente.
   - **Reusabilidade e Consistência**: Como a infraestrutura é codificada, você pode usar as mesmas configurações em múltiplos ambientes, garantindo consistência.
   - **Auditoria e Histórico**: O uso de CI/CD também garante um histórico de mudanças em logs, facilitando auditorias e rastreabilidade de alterações.

#### **Exemplos de Integração do Terraform com GitLab CI, GitHub Actions e Jenkins**

Vamos agora ver exemplos de como integrar o Terraform com três das ferramentas de CI/CD mais populares: **GitLab CI**, **GitHub Actions** e **Jenkins**.

---

##### **1. GitLab CI**

**GitLab CI** é uma plataforma de integração e entrega contínua nativa do GitLab. Para integrar o Terraform com o GitLab CI, é necessário configurar um arquivo `.gitlab-ci.yml`, onde você define os estágios e jobs da pipeline.

Exemplo de `.gitlab-ci.yml`:

```yaml
stages:
  - validate
  - plan
  - apply

variables:
  TF_VAR_region: "us-east-1"
  TF_STATE_BUCKET: "my-terraform-state"

before_script:
  - export TF_VERSION=$(terraform version -json | jq -r .terraform_version)
  - terraform --version

validate:
  stage: validate
  script:
    - terraform init
    - terraform validate

plan:
  stage: plan
  script:
    - terraform init
    - terraform plan -out=tfplan

apply:
  stage: apply
  script:
    - terraform init
    - terraform apply -auto-approve tfplan
  when: manual  # Habilita o 'manual' para aprovação antes de aplicar
```

- **`before_script`**: Este bloco executa tarefas comuns antes de qualquer job, como verificar a versão do Terraform.
- **Jobs**:
  - **validate**: Inicializa o Terraform e valida as configurações.
  - **plan**: Executa o comando `terraform plan` para gerar um plano de execução.
  - **apply**: Aplica o plano gerado, mas somente após aprovação manual (usando `when: manual`).

##### **2. GitHub Actions**

**GitHub Actions** é uma plataforma de automação nativa do GitHub que permite criar workflows para CI/CD. Abaixo está um exemplo básico de um arquivo de workflow para o Terraform em **GitHub Actions**.

Exemplo de `main.yml` (em `.github/workflows`):

```yaml
name: Terraform CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan -out=tfplan

      - name: Terraform Apply
        run: terraform apply -auto-approve tfplan
        if: github.ref == 'refs/heads/main'  # Aplica apenas quando houver um push para o branch main
```

- **`on`**: Define os eventos que acionam o workflow (ex: push para o branch `main` ou pull request).
- **Jobs**:
  - **checkout**: Faz o checkout do código fonte.
  - **setup-terraform**: Instala o Terraform no runner do GitHub.
  - **Terraform Init**: Inicializa o Terraform.
  - **Terraform Plan**: Executa o comando `terraform plan` para verificar as mudanças.
  - **Terraform Apply**: Aplica as mudanças na infraestrutura quando o código é enviado ao branch `main`.

##### **3. Jenkins**

**Jenkins** é uma das ferramentas mais conhecidas de CI/CD, com ampla flexibilidade na configuração de pipelines. Para usar o Terraform com Jenkins, você pode criar um pipeline no Jenkinsfile.

Exemplo de `Jenkinsfile`:

```groovy
pipeline {
    agent any

    environment {
        TF_VAR_region = 'us-east-1'
        TF_STATE_BUCKET = 'my-terraform-state'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: 'Approve Apply?', ok: 'Yes'
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }
}
```

- **Environment**: Define as variáveis de ambiente, como região e bucket de estado do Terraform.
- **Stages**:
  - **Checkout**: Realiza o checkout do código do repositório.
  - **Terraform Init**: Inicializa o Terraform.
  - **Terraform Plan**: Executa o comando `terraform plan`.
  - **Terraform Apply**: Exige aprovação manual para aplicar as mudanças (usando `input message`).

#### **Boas Práticas ao Integrar Terraform com CI/CD**

1. **Armazenamento do Estado**: Utilize **backends remotos** (como S3 ou GCS) para armazenar o estado da infraestrutura. Isso garante que o estado seja compartilhado entre diferentes pipelines e equipes, além de ser mais seguro.
   
2. **Segurança**: Nunca armazene credenciais ou dados sensíveis diretamente no código da pipeline. Utilize **variáveis de ambiente** ou **secret management** (ex: AWS Secrets Manager, HashiCorp Vault, GitHub Secrets).

3. **Aprovação Manual**: Ao aplicar mudanças críticas em ambientes de produção, sempre utilize uma etapa de **aprovação manual** na pipeline para garantir que as alterações sejam revisadas antes de serem aplicadas.

4. **Validação do Código**: Sempre valide as configurações do Terraform com `terraform validate` antes de rodar `terraform plan`, para garantir que não há erros de sintaxe ou configuração.

5. **Ambientes Separados**: Separe os ambientes de **desenvolvimento**, **staging** e **produção** nas pipelines para evitar que mudanças em um ambiente afetem outro, usando variáveis de ambiente ou workspaces do Terraform.

#### **Resumo do Vídeo 12: Integrando Terraform com CI/CD**

Neste vídeo, abordamos como integrar o **Terraform** com pipelines de **Integração Contínua (CI)** e **Entrega Contínua (CD)** para automatizar o processo de provisionamento e gerenciamento de infraestrutura. Vimos exemplos práticos de integração do Terraform com três ferramentas populares: **GitLab CI**, **GitHub Actions** e **Jenkins**. Essas práticas permitem que você acelere o ciclo de vida da infraestrutura, garanta consistência, segurança e controle, e automatize o processo de aplicar mudanças na infraestrutura, sempre que houver uma alteração no código.