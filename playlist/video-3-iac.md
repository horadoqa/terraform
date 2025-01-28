#### **2. Introdução à Infraestrutura como Código (IaC)**
- **Vídeo 3: O que é Infraestrutura como Código?**
  - Definição de IaC e sua importância.
  - Diferença entre IaC e automação de infraestrutura.
  - Benefícios do IaC: versionamento, consistência, repetibilidade.

### **Vídeo 3: O que é Infraestrutura como Código?**

#### **Definição de IaC e Sua Importância**

**Infraestrutura como Código (IaC)** é uma abordagem que permite gerenciar e provisionar recursos de infraestrutura por meio de código, ao invés de configurá-los manualmente ou através de interfaces gráficas. Em outras palavras, com o IaC, toda a configuração da infraestrutura (como servidores, redes, bancos de dados, etc.) é descrita e gerenciada através de arquivos de código, que podem ser versionados, auditados e reproduzidos automaticamente.

**Exemplo prático**: Em vez de criar servidores, redes e configurar firewalls manualmente, você escreve um código (geralmente em um formato declarativo, como **HCL** no Terraform ou **YAML** no Ansible) que descreve como a infraestrutura deve ser configurada. Esse código pode então ser executado para criar e gerenciar a infraestrutura automaticamente.

**Importância**:
- **Automatização**: A IaC ajuda a automatizar o processo de provisionamento e gestão de infraestrutura, o que reduz erros humanos e acelera a implantação.
- **Escalabilidade**: Permite replicar a infraestrutura facilmente, criando ambientes consistentes em várias regiões ou ambientes (produção, testes, desenvolvimento).
- **Consistência**: Com IaC, você pode garantir que a infraestrutura seja configurada de maneira idêntica em diferentes ambientes, sem discrepâncias ou configurações manuais erradas.

#### **Diferença entre IaC e Automação de Infraestrutura**

Embora ambos os conceitos envolvam a automação do gerenciamento da infraestrutura, **IaC** e **automação de infraestrutura** não são a mesma coisa.

- **IaC (Infraestrutura como Código)**:
  - A principal diferença do IaC é que ele trata a infraestrutura como **código**. Ou seja, ao invés de usar scripts ou ferramentas para configurar a infraestrutura, você escreve a configuração de forma declarativa em arquivos de código. Isso inclui definir redes, instâncias de máquinas, balanceadores de carga, etc.
  - O código é versionado e gerenciado da mesma forma que o código-fonte de um aplicativo, possibilitando auditorias e revisões no histórico de mudanças.
  - **Exemplo**: Usar o **Terraform** para definir toda a infraestrutura (instâncias de servidores, redes, serviços de nuvem) em um arquivo de código.

- **Automação de Infraestrutura**:
  - A automação de infraestrutura, por outro lado, pode envolver o uso de **scripts** ou ferramentas que realizam tarefas repetitivas de configuração, mas não necessariamente tratam a infraestrutura como código. A automação pode ser imperativa, ou seja, você descreve o **passo a passo** para executar uma tarefa específica, sem garantir a total consistência do ambiente.
  - **Exemplo**: Usar scripts em **Bash** ou **PowerShell** para instalar software e configurar servidores manualmente, ou ferramentas como **Ansible** ou **Chef** para orquestrar a configuração de servidores de maneira mais programada.

**Resumo da diferença**:
- **IaC** foca em definir e gerenciar a infraestrutura de forma declarativa, com código versionado e facilmente reproduzível.
- **Automação de Infraestrutura** envolve scripts ou ferramentas para automatizar tarefas e configurações, mas não necessariamente com a mesma abordagem de código-fonte versionado ou reusabilidade.

#### **Benefícios do IaC: Versionamento, Consistência, Repetibilidade**

1. **Versionamento**:
   - Um dos maiores benefícios do IaC é que a configuração da infraestrutura é tratada da mesma forma que o código-fonte do aplicativo, ou seja, você pode **versionar** a infraestrutura. Isso permite que você tenha um histórico de alterações, podendo verificar quando, como e por que a infraestrutura foi alterada.
   - **Exemplo**: Se uma nova versão de sua infraestrutura for implantada (como adicionar uma nova instância de servidor), o código que descreve essa infraestrutura pode ser versionado, facilitando o rollback para uma versão anterior, caso necessário.

2. **Consistência**:
   - O IaC garante que a infraestrutura seja criada da mesma forma em diferentes ambientes (desenvolvimento, testes, produção). Ao tratar a infraestrutura como código, você pode garantir que, quando o código for executado em diferentes máquinas ou regiões, o ambiente será idêntico em todos os pontos.
   - **Exemplo**: Se você criar um ambiente de teste com uma determinada configuração, pode replicá-lo exatamente no ambiente de produção, sem o risco de uma configuração manual incorreta. Isso evita erros comuns causados pela diferença entre os ambientes.

3. **Repetibilidade**:
   - Com o IaC, a criação e configuração de infraestrutura se tornam **repetíveis** e previsíveis. Uma vez que a infraestrutura é descrita como código, ela pode ser **recriada** de maneira idêntica, quantas vezes forem necessárias. Isso é fundamental para a escalabilidade e para a implementação de **práticas DevOps**.
   - **Exemplo**: Suponha que você queira criar um novo ambiente de produção para um cliente. Com IaC, você pode rodar o mesmo código que foi usado para criar o ambiente anterior e ter a mesma infraestrutura criada automaticamente.

### **Resumo do Vídeo 3: O que é Infraestrutura como Código?**

- **Infraestrutura como Código (IaC)** é uma abordagem onde a infraestrutura é gerida por meio de código, permitindo automação, versionamento e reprodução fácil.
- **Diferença entre IaC e automação de infraestrutura**: Enquanto IaC trata a infraestrutura como código declarativo e versionado, a automação de infraestrutura envolve scripts ou ferramentas para tarefas repetitivas, sem garantir a mesma consistência ou reusabilidade.
- **Benefícios do IaC**:
  - **Versionamento**: Permite o controle de mudanças e facilita a auditoria e o rollback de configurações.
  - **Consistência**: Garante que ambientes sejam idênticos, sem discrepâncias entre desenvolvimento, testes e produção.
  - **Repetibilidade**: Facilita a criação de ambientes idênticos em qualquer lugar, quando necessário.

Este vídeo ajudará o espectador a entender o que é IaC, como ela difere da automação tradicional de infraestrutura e os principais benefícios dessa abordagem para a gestão de infraestrutura moderna.