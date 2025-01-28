# TERRAFORM

O **Terraform** é uma ferramenta de **infraestrutura como código** (IaC) desenvolvida pela HashiCorp, usada para provisionar, gerenciar e configurar recursos em ambientes de nuvem e em data centers. Ele permite automatizar a criação, alteração e versionamento de infraestrutura de forma segura e eficiente.

Aqui estão alguns pontos importantes sobre o Terraform:

### 1. **Infraestrutura como Código (IaC)**

O Terraform permite que você escreva a infraestrutura em código, usando uma linguagem declarativa chamada **HCL** (HashiCorp Configuration Language). Isso significa que você pode definir o estado desejado da infraestrutura em arquivos de configuração e deixar o Terraform se encarregar de garantir que o ambiente seja configurado de acordo com esses arquivos.

### 2. **Multi-Nuvem e Multi-Plataforma**

O Terraform suporta uma vasta gama de provedores de nuvem, como **AWS, Azure, Google Cloud**, além de **provedores de infraestrutura on-premises** e outros serviços de terceiros (como Kubernetes, GitHub, Cloudflare, entre outros). Isso torna o Terraform uma solução muito poderosa, permitindo que você use uma única ferramenta para gerenciar recursos em múltiplas plataformas de forma consistente.

### 3. **Provisionamento e Gerenciamento de Infraestrutura**

Com o Terraform, você pode automatizar tarefas que, de outra forma, seriam feitas manualmente ou por meio de interfaces gráficas (GUIs), como:
- Criar máquinas virtuais (VMs)
- Configurar redes, sub-redes e firewalls
- Provisionar bancos de dados
- Gerenciar políticas de segurança

Ele permite definir **recursos** como código, o que facilita a automação, repetibilidade e auditoria da infraestrutura.

### 4. **Estado e Planejamento**

O Terraform mantém um **arquivo de estado** que armazena informações sobre os recursos já criados, facilitando a comparação do estado atual da infraestrutura com o desejado. Antes de aplicar qualquer mudança, o Terraform gera um **plano de execução** que mostra o que será alterado, criando uma forma de validar as mudanças antes que elas ocorram.

### 5. **Módulos e Reusabilidade**

O Terraform permite criar **módulos**, que são conjuntos de configurações reutilizáveis. Módulos podem ser compartilhados e utilizados em diferentes projetos, promovendo a consistência e a reutilização de código. Existe também uma grande comunidade de módulos prontos, como os disponibilizados no **Terraform Registry**.

### 6. **Execução Declarativa**

Com o Terraform, você descreve o **estado desejado** de sua infraestrutura (por exemplo, "Eu quero 3 máquinas virtuais na AWS com X configurações"), e o Terraform se encarrega de fazer o trabalho de **ajustar** a infraestrutura conforme necessário para que ela atenda a esse estado.

### 7. **Colaboração e Versionamento**

Como o Terraform usa arquivos de configuração de texto simples, é possível versionar sua infraestrutura com ferramentas como Git. Isso facilita a colaboração em equipe, o controle de mudanças e a reversibilidade de alterações.

### 8. **Planos de Execução e Aplicação**

Antes de aplicar qualquer mudança, o Terraform gera um **plano de execução**, mostrando as alterações que serão feitas na infraestrutura. Isso é muito útil para evitar erros e garantir que as mudanças sejam previstas antes de sua aplicação.

### Fluxo de Trabalho Básico do Terraform:

1. **Escrever Configuração:** Você escreve as configurações da infraestrutura no formato HCL, especificando os recursos e suas dependências.
   
2. **Inicializar:** Inicializa o Terraform com o comando `terraform init` para configurar o ambiente e baixar os plugins necessários.

3. **Planejar:** O comando `terraform plan` analisa as mudanças e mostra o que será alterado na infraestrutura.

4. **Aplicar:** Com o comando `terraform apply`, o Terraform executa as alterações na infraestrutura de acordo com o plano.

5. **Gerenciar:** Você pode atualizar, destruir ou verificar o estado da sua infraestrutura usando o `terraform apply`, `terraform destroy` e `terraform show`, respectivamente.

### Vantagens do Terraform:

- **Automação e Eficiência:** Automatiza a criação e o gerenciamento de infraestrutura, economizando tempo e reduzindo erros.
- **Consistência:** Garante que a infraestrutura seja configurada da mesma maneira em diferentes ambientes, seja em desenvolvimento, teste ou produção.
- **Escalabilidade:** Permite escalar facilmente sua infraestrutura à medida que a demanda cresce.
- **Gerenciamento de Mudanças:** Facilita a adaptação de configurações e recursos, com o gerenciamento adequado de versões e histórico de mudanças.

### Quando Usar o Terraform?

- **Provisionamento inicial de infraestrutura**: Usado para criar recursos de nuvem de forma automática e configurada.
- **Gerenciamento contínuo**: Ideal para gerenciar recursos em longo prazo, como manter configurações de servidores, bancos de dados, redes etc.
- **Migração entre provedores**: Se você precisar migrar recursos de um provedor de nuvem para outro, o Terraform facilita o gerenciamento e a transição.
  
### Conclusão

O Terraform é uma ferramenta extremamente poderosa e flexível para quem deseja adotar práticas de infraestrutura como código e automação em seu ambiente. Sua capacidade de suportar múltiplos provedores de nuvem e de facilitar o gerenciamento e provisionamento de recursos em larga escala o torna uma escolha popular para equipes que buscam mais eficiência e consistência na gestão da infraestrutura.

[Instalação](./install.md)