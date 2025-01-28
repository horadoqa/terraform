- **Vídeo 14: Segurança e Governança com Terraform**
  - Como gerenciar credenciais e permissões de forma segura (ex: AWS IAM, Terraform Cloud).
  - Implementação de práticas de governança e controle de mudanças (ex: revisão de código, aprovações).

### **Vídeo 14: Segurança e Governança com Terraform**

No **Vídeo 14**, abordamos as práticas essenciais para garantir que a infraestrutura provisionada com **Terraform** seja segura, audível e esteja em conformidade com políticas de governança. A segurança e a governança são fatores cruciais para o sucesso a longo prazo na automação da infraestrutura, pois garantem que os recursos sejam configurados e acessados de maneira controlada e com o mínimo de risco.

#### **Como Gerenciar Credenciais e Permissões de Forma Segura**

Quando se trabalha com **Terraform** em ambientes de nuvem (como AWS, Azure, Google Cloud, etc.), é fundamental garantir que as credenciais e permissões sejam gerenciadas de forma segura, para evitar o acesso não autorizado e proteger a integridade da infraestrutura.

##### **1. Gerenciamento de Credenciais no AWS IAM**

O **AWS IAM (Identity and Access Management)** permite que você controle o acesso aos recursos da AWS. Quando você usa Terraform para provisionar e gerenciar recursos na AWS, é crucial garantir que as credenciais da AWS sejam configuradas de forma segura.

**Boas práticas** para gerenciamento de credenciais na AWS com Terraform:
   
- **Utilização de credenciais temporárias**: Sempre que possível, utilize **credenciais temporárias** com **AWS STS (Security Token Service)**. Isso limita o tempo de vida das credenciais e reduz os riscos associados a credenciais de longo prazo.
  
- **Uso de perfis IAM**: Em vez de hardcodificar as credenciais no código (o que é inseguro), utilize **perfis IAM** no EC2, Lambda ou outros serviços para fornecer credenciais temporárias automaticamente aos recursos da AWS.

- **Gerenciamento de credenciais através de ferramentas de gerenciamento**: Ferramentas como **AWS Secrets Manager** ou **HashiCorp Vault** podem ser usadas para armazenar e recuperar credenciais de forma segura, sem expô-las diretamente no código.

**Exemplo de configuração de um provider com perfil IAM**:

```hcl
provider "aws" {
  region  = "us-east-1"
  profile = "my-iam-profile"
}
```

##### **2. Gerenciamento de Permissões com Terraform Cloud**

**Terraform Cloud** oferece um gerenciamento centralizado de credenciais, controle de acesso e colaboração entre equipes. Ao utilizar o Terraform Cloud, você pode **centralizar** a gestão de permissões e credenciais em um único lugar e seguir boas práticas de segurança, como autenticação multifatorial (MFA) e controle de acesso baseado em funções (RBAC).

**Exemplo de configuração de um Workspace no Terraform Cloud**:

No **Terraform Cloud**, as credenciais e configurações são gerenciadas diretamente no **Workspace**, que pode ser configurado com permissões específicas para diferentes membros da equipe. A partir do **Terraform Cloud**, é possível integrar o uso de **Secret Variables** para gerenciar credenciais sensíveis de forma segura, sem a necessidade de hardcoding no código do Terraform.

##### **3. Uso de Variáveis Seguras no Terraform**

O Terraform permite o uso de variáveis para armazenar credenciais, chaves de API e outros dados sensíveis. Para evitar o armazenamento de informações sensíveis no código, é fundamental utilizar **variáveis** de forma segura.

- **Variáveis de ambiente**: Você pode armazenar variáveis sensíveis (como chaves de acesso e tokens) como **variáveis de ambiente**, que são carregadas no momento da execução.

  ```bash
  export AWS_ACCESS_KEY_ID="your-access-key"
  export AWS_SECRET_ACCESS_KEY="your-secret-key"
  ```

- **Files de variáveis**: Em vez de incluir valores sensíveis no código ou nos arquivos `.tf`, você pode armazená-los em **files de variáveis** (ex: `secrets.tfvars`) e garantir que este arquivo não seja versionado no controle de versão (como Git) utilizando um `.gitignore`.

#### **Implementação de Práticas de Governança e Controle de Mudanças**

A **governança** no contexto de infraestrutura como código com Terraform envolve práticas para garantir que as mudanças na infraestrutura sejam feitas de forma controlada, segura e auditável. Isso envolve a revisão do código, aprovação de mudanças e a auditoria de quem fez o quê e quando.

##### **1. Revisão de Código e Controle de Versão**

O código do Terraform é, essencialmente, código fonte e deve ser tratado como tal. **Revisões de código** são fundamentais para garantir que as mudanças sejam aprovadas por membros da equipe antes de serem aplicadas.

- **Uso de Pull Requests**: Ao usar sistemas de controle de versão como **Git** (GitHub, GitLab, Bitbucket), sempre use **pull requests** para submeter mudanças no código de infraestrutura. Isso garante que o código seja revisado por outras pessoas antes de ser mesclado ao branch principal.

- **Análise de Código e Linting**: Ferramentas de linting como **terraform validate** e **terraform fmt** devem ser integradas ao seu processo de CI/CD para garantir que o código esteja correto e formatado de acordo com as melhores práticas.

- **Auditoria e Logs de Mudanças**: Ferramentas como **Terraform Cloud** e **Terraform Enterprise** oferecem funcionalidades de auditoria, registrando quem fez cada mudança no código e quando ela foi realizada. Essa transparência é essencial para manter a conformidade e rastreabilidade.

##### **2. Aprovação de Mudanças**

A **apreciação e revisão de mudanças** é um processo essencial de governança, especialmente em ambientes de produção. Garantir que as mudanças sejam analisadas e aprovadas por membros da equipe ou responsáveis de diferentes áreas antes de sua execução reduz o risco de erros e configurações indesejadas.

- **Processo de Aprovação Manual**: No Terraform Cloud ou em uma pipeline de CI/CD, você pode configurar etapas de **aprovação manual** para garantir que as mudanças sejam revisadas por uma pessoa antes de serem aplicadas em ambientes críticos (ex: produção).

**Exemplo de aprovação manual no GitLab CI**:

```yaml
apply:
  stage: apply
  script:
    - terraform apply -auto-approve tfplan
  when: manual  # Exige aprovação manual antes de aplicar as mudanças
```

##### **3. Controle de Acesso Baseado em Funções (RBAC)**

O **RBAC** (Controle de Acesso Baseado em Funções) é uma técnica de governança usada para definir o que cada usuário ou grupo de usuários pode fazer com a infraestrutura. No Terraform, você pode configurar o RBAC tanto nas ferramentas de nuvem quanto no **Terraform Cloud** para garantir que apenas pessoas autorizadas possam fazer mudanças em ambientes críticos.

- **Exemplo de Configuração de RBAC no Terraform Cloud**: 

  No **Terraform Cloud**, você pode configurar o acesso de forma granular, atribuindo permissões específicas para diferentes membros da equipe ou grupos. Por exemplo, você pode permitir que alguns membros tenham apenas permissões de leitura (`read`), enquanto outros tenham permissões para aplicar mudanças (`write`).

##### **4. Gerenciamento de Workspaces e Ambientes**

A governança também envolve o gerenciamento de **workspaces** do Terraform, que permitem separar configurações e estados de diferentes ambientes (desenvolvimento, staging, produção). Isso ajuda a garantir que mudanças em um ambiente não afetem outros e facilita a organização do código.

- **Workspaces de Terraform**: Ao usar workspaces, você pode isolar configurações para diferentes ambientes, o que facilita o controle de versões e as aprovações de mudanças por ambiente.

**Exemplo de Workspaces**:

```bash
terraform workspace new development
terraform workspace select production
```

#### **Boas Práticas de Segurança e Governança**

1. **Utilize MFA (Autenticação Multifatorial)**: Para proteger ainda mais as credenciais e o acesso aos ambientes de nuvem, sempre habilite **MFA** em contas que utilizam Terraform, especialmente para acesso a sistemas de gerenciamento de estado e Terraform Cloud.

2. **Revisões de código e auditoria**: Garanta que todo código do Terraform seja revisado antes de ser aplicado. Utilize ferramentas de auditoria para registrar todas as mudanças na infraestrutura.

3. **Use ferramentas de Secrets Management**: Armazene credenciais sensíveis em ferramentas de gerenciamento de segredos, como **HashiCorp Vault** ou **AWS Secrets Manager**, em vez de armazená-las diretamente no código.

4. **Segregação de ambientes**: Separe claramente os diferentes ambientes (dev, staging, produção) e aplique controles de acesso rigorosos para evitar que mudanças não intencionais afetem ambientes críticos.

5. **Controle de versões de configurações**: Utilize **Git** e **GitOps** para controlar e versionar as configurações de infraestrutura, garantindo que o histórico de mudanças seja mantido e auditável.

#### **Resumo do Vídeo 14: Segurança e Governança com Terraform**

Neste vídeo, discutimos como implementar práticas de **segurança** e **governança** ao usar o **Terraform** para provisionar e gerenciar infraestrutura. Isso inclui a gestão de credenciais de maneira segura, como o uso do **AWS IAM**, **Terraform Cloud** e ferramentas de gerenciamento de segredos, além de práticas de governança como **revisões de código**, **aprovações manuais** e **controle de acessos** com **RBAC**. Essas práticas ajudam a garantir que a infraestrutura seja gerenciada de forma segura, auditável e controlada, minimizando riscos e garantindo a conformidade com políticas organizacionais e regulatórias.