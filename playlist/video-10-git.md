- **Vídeo 10: Versionamento de Infraestrutura com Terraform**
  - Como usar o Terraform com Git para versionar configurações.
  - Estratégias para gerenciar mudanças de infraestrutura com segurança.

### **Vídeo 10: Versionamento de Infraestrutura com Terraform**

No décimo vídeo da série, vamos explorar como integrar o **Terraform** com **Git** para o **versionamento de configurações de infraestrutura**. O versionamento é uma prática essencial para garantir a rastreabilidade e a consistência ao gerenciar a infraestrutura como código. Além disso, abordaremos **estratégias para gerenciar mudanças de infraestrutura** de maneira segura e controlada, ajudando a evitar erros e impactos negativos durante as modificações.

#### **Como Usar o Terraform com Git para Versionar Configurações**

O **Git** é uma ferramenta de controle de versão amplamente usada, que permite rastrear e gerenciar mudanças no código de forma eficiente. Quando se trata de **infraestrutura como código** (IaC) com o **Terraform**, o uso do Git é fundamental para permitir que equipes colaborem, monitorem alterações e garantam a consistência ao longo do tempo.

1. **Versionando Arquivos de Configuração do Terraform com Git**:
   - O primeiro passo é colocar os arquivos de configuração do Terraform (os arquivos `.tf`) em um repositório Git. Esses arquivos contêm todas as definições de recursos e a lógica da infraestrutura que você está criando e gerenciando.
   - Exemplo de estrutura de projeto com Git:
     ```
     meu-projeto-terraform/
     ├── main.tf
     ├── variables.tf
     ├── outputs.tf
     ├── terraform.tfvars
     └── README.md
     ```
   - Esses arquivos definem os recursos que o Terraform irá criar, como máquinas virtuais, redes, e outros componentes de infraestrutura.

2. **Commitando e Versionando as Configurações**:
   - Após criar ou modificar a infraestrutura em Terraform, você pode versionar essas mudanças com **commits** no Git. O fluxo básico seria:
     ```bash
     git init  # Inicializa um repositório Git se ainda não houver um
     git add .  # Adiciona todos os arquivos modificados ou novos
     git commit -m "Adiciona configuração inicial da infraestrutura"
     git push origin main  # Envia as alterações para o repositório remoto
     ```
   - Isso cria um histórico das mudanças feitas na configuração da infraestrutura, permitindo que você **volte no tempo** e **entenda como a infraestrutura evoluiu** ao longo dos commits.

3. **Usando Branches para Isolar Mudanças**:
   - Para evitar impactos inesperados na infraestrutura de produção, é importante usar **branches** no Git para isolar diferentes ambientes ou alterações. Exemplo:
     - `main` (para produção)
     - `dev` (para ambiente de desenvolvimento)
     - `feature/infra-v2` (para novos recursos ou mudanças)

   Isso permite que você trabalhe em alterações sem afetar diretamente os ambientes críticos, aplicando mudanças em produção somente quando elas forem validadas e testadas.

4. **Práticas recomendadas ao versionar configurações do Terraform**:
   - **Não versionar o arquivo de estado (`terraform.tfstate`)**: O arquivo de estado contém informações sensíveis sobre a infraestrutura e não deve ser versionado no Git. Em vez disso, utilize **backends remotos** (como AWS S3) para gerenciar o estado de forma segura.
   - **Adicione `terraform.tfstate` ao `.gitignore`**: Para garantir que o Terraform não adicione o arquivo de estado ao repositório, inclua-o no arquivo `.gitignore`:
     ```
     terraform.tfstate
     terraform.tfstate.backup
     .terraform/
     ```

#### **Estratégias para Gerenciar Mudanças de Infraestrutura com Segurança**

Gerenciar mudanças na infraestrutura requer cuidado para evitar impactos negativos em produção. Com o uso do Terraform, existem algumas estratégias recomendadas para garantir que as alterações sejam feitas de forma controlada e segura.

1. **Usando o Terraform Plan para Antecipar Alterações**:
   - O comando `terraform plan` é fundamental para verificar e visualizar as mudanças antes de aplicá-las. Esse comando gera um plano de execução que mostra quais recursos serão **criados**, **modificados** ou **destruídos**.
   - Exemplo de uso:
     ```bash
     terraform plan -out=tfplan
     ```
   - A saída do comando é um relatório detalhado das mudanças propostas. Isso permite que você revise o plano e se certifique de que as alterações não terão impactos inesperados.

2. **Revisão de Código e Pull Requests**:
   - Sempre que uma mudança for feita na configuração da infraestrutura, é importante usar o **GitHub**, **GitLab** ou outra plataforma de versionamento para realizar uma **revisão de código**. Isso envolve a criação de **pull requests** (PRs), onde as alterações podem ser revisadas por colegas antes de serem mescladas na branch principal.
   - **Estratégia**: Um processo comum de revisão seria garantir que qualquer mudança na configuração do Terraform passe por uma **revisão de código** e um **teste de integração** para garantir que a infraestrutura proposta esteja em conformidade com os requisitos.

3. **Ambientes Separados (Dev, Staging, Prod)**:
   - Uma estratégia comum para gerenciar mudanças de infraestrutura é usar **ambientes separados** para desenvolvimento (Dev), testes (Staging) e produção (Prod). Isso permite que você valide as alterações em ambientes não críticos antes de aplicá-las na produção.
   - O Terraform pode ser configurado para **variáveis** específicas por ambiente. Por exemplo, você pode ter diferentes arquivos `.tfvars` para ambientes diferentes:
     - `dev.tfvars` para configurações de desenvolvimento.
     - `prod.tfvars` para configurações de produção.
   - Isso ajuda a evitar que alterações acidentais em um ambiente de produção possam ocorrer.

4. **Utilizando Workspaces no Terraform**:
   - O Terraform suporta **workspaces**, que permitem criar **instâncias separadas de uma configuração**. Cada workspace pode ter seu próprio estado, o que facilita a gestão de múltiplos ambientes com a mesma configuração de código.
   - Com isso, você pode usar comandos como:
     ```bash
     terraform workspace new dev
     terraform workspace select dev
     ```
   - Isso permite a criação de recursos de infraestrutura específicos para cada ambiente (ex: Dev, Test, Prod), mantendo tudo sob a mesma base de código.

5. **Automatizando o Fluxo de Trabalho com CI/CD**:
   - Para melhorar a segurança e reduzir o risco de erros manuais, é recomendado automatizar o fluxo de trabalho de **Infraestrutura como Código** com ferramentas de **CI/CD** (Integração Contínua e Entrega Contínua).
   - Exemplos:
     - **GitHub Actions**, **GitLab CI**, ou **Jenkins** podem ser configurados para rodar automaticamente `terraform plan` e `terraform apply` em diferentes ambientes após a aprovação de um pull request.
     - Isso ajuda a aplicar de maneira segura as mudanças de infraestrutura, garantindo que o processo seja auditável e controlado.

6. **Backup e Recuperação**:
   - Como as mudanças de infraestrutura podem ser críticas, é importante ter um **plano de backup** para o estado da infraestrutura (por exemplo, fazer backup do arquivo de estado em S3 ou outras soluções de armazenamento seguro).
   - Em caso de falhas, o Terraform também oferece comandos como `terraform destroy` para remover recursos com segurança, ou `terraform state` para manipular e restaurar o estado manualmente.

#### **Resumo do Vídeo 10: Versionamento de Infraestrutura com Terraform**

Neste vídeo, abordamos como usar o **Git** para versionar as configurações de infraestrutura criadas com o **Terraform**, permitindo o controle das alterações e a colaboração entre equipes. Também exploramos **estratégias para gerenciar mudanças de infraestrutura** de maneira segura, incluindo o uso de `terraform plan` para antecipar mudanças, a criação de ambientes separados para desenvolvimento, staging e produção, o uso de **workspaces** para isolar ambientes, e a automação do fluxo de trabalho de infraestrutura por meio de **CI/CD**.

A integração do Terraform com o Git e a implementação de boas práticas para o versionamento e gerenciamento de mudanças ajudam a garantir a **segurança**, **consistência** e **eficiência** na gestão da infraestrutura como código.