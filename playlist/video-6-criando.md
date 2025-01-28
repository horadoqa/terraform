- **Vídeo 6: Criando sua Primeira Infraestrutura com Terraform**
  - Passo a passo: configuração do Terraform, criando recursos simples na AWS (ex: instância EC2).
  - Explicação do fluxo de trabalho: escrever código, rodar `terraform plan` e `terraform apply`.

### **Vídeo 6: Criando sua Primeira Infraestrutura com Terraform**

Neste sexto vídeo da série, vamos colocar a teoria em prática e mostrar como você pode começar a usar o Terraform para criar sua primeira infraestrutura na nuvem. O exemplo será a criação de uma **instância EC2 na AWS**, um dos recursos mais comuns que você provavelmente usará com o Terraform. Vamos detalhar o passo a passo da configuração e execução, além de explicar o fluxo de trabalho que envolve escrever código, rodar os comandos `terraform plan` e `terraform apply`.

#### **Passo a Passo: Configuração do Terraform e Criando Recursos Simples na AWS**

1. **Instalação do Terraform**
   
   - Antes de começar, você precisa **instalar o Terraform** em sua máquina. Você pode baixá-lo a partir do [site oficial do Terraform](https://www.terraform.io/downloads.html) e seguir as instruções de instalação para o seu sistema operacional (Windows, macOS, Linux).
   
2. **Configuração do Terraform**
   
   Após a instalação, o primeiro passo é **configurar o Terraform** para interagir com a AWS. Para isso, você precisa garantir que o Terraform tenha as credenciais da AWS para criar e gerenciar recursos.
   
   - **Credenciais da AWS**: Você pode configurar as credenciais AWS utilizando o **AWS CLI** ou manualmente criando um arquivo de credenciais. O Terraform irá usar essas credenciais para se conectar à AWS.
   
   Se você ainda não tem as credenciais configuradas, o procedimento mais comum é usar o **AWS CLI** para configurar suas credenciais. Isso pode ser feito com o comando:
   ```bash
   aws configure
   ```
   Você precisará fornecer o **Access Key ID** e **Secret Access Key** da sua conta AWS, além da região (ex: `us-east-1`).

3. **Escrevendo o Código Terraform**
   
   Após a configuração do Terraform e das credenciais, vamos escrever o código que define a infraestrutura que queremos criar. O código será escrito em um arquivo de configuração com a extensão `.tf`.

   - **Exemplo de Código para Criar uma Instância EC2**:
     ```hcl
     provider "aws" {
       region = "us-east-1"  # Região da AWS
     }

     resource "aws_instance" "example" {
       ami           = "ami-0c55b159cbfafe1f0"  # ID da AMI (imagem de máquina)
       instance_type = "t2.micro"  # Tipo de instância
     }
     ```

     Nesse código:
     - **provider** define o provedor de nuvem que estamos usando (AWS) e a região onde os recursos serão provisionados.
     - **resource** descreve o recurso específico que desejamos criar. Neste caso, estamos criando uma **instância EC2** usando a imagem **AMI** fornecida.

     Salve esse código em um arquivo chamado `main.tf`.

4. **Inicializando o Terraform**

   Após escrever o código, o próximo passo é **inicializar o Terraform**. O comando `terraform init` é utilizado para inicializar o diretório de trabalho do Terraform. Isso irá configurar o ambiente e baixar os plugins necessários para interagir com a AWS (e com outros provedores, se necessário).

   ```bash
   terraform init
   ```

5. **Rodando o Comando `terraform plan`**

   O comando `terraform plan` é utilizado para visualizar as ações que o Terraform realizará, comparando a infraestrutura desejada (descrita no código) com o estado atual da infraestrutura. Ele gera um **plano de execução** mostrando o que será criado, modificado ou destruído.

   O comando a ser executado é:
   ```bash
   terraform plan
   ```

   - Quando você executa esse comando, o Terraform analisa o código no arquivo `.tf` e consulta o estado atual da AWS. Ele então exibe um plano detalhado de **quais recursos serão criados**, como no caso da instância EC2. O Terraform irá mostrar algo como:
     ```
     + aws_instance.example
         id: <computed>
         ami: "ami-0c55b159cbfafe1f0"
         instance_type: "t2.micro"
     ```

   - Isso garante que você saiba o que acontecerá antes de aplicar as mudanças.

6. **Rodando o Comando `terraform apply`**

   Após revisar o plano e confirmar que está tudo correto, o próximo passo é rodar o comando `terraform apply`. Esse comando vai **aplicar as mudanças** e provisionar os recursos na AWS.

   ```bash
   terraform apply
   ```

   - O Terraform pedirá uma confirmação antes de aplicar as alterações. Basta digitar **`yes`** para confirmar.

   - O Terraform então criará a instância EC2 na AWS, e você verá uma saída semelhante à seguinte:
     ```
     aws_instance.example: Creating...
     aws_instance.example: Still creating... (10s elapsed)
     aws_instance.example: Creation complete after 20s (id: i-0ab1c234d567e8fgh)
     ```

7. **Verificando a Instância na AWS**

   Após o Terraform concluir a criação da instância, você pode ir ao **Console da AWS** e verificar se a instância EC2 foi criada corretamente na região que você especificou (neste exemplo, `us-east-1`).

#### **Explicação do Fluxo de Trabalho: Escrever Código, Rodar `terraform plan` e `terraform apply`**

1. **Escrever Código**:
   - O primeiro passo é sempre escrever o código que define a infraestrutura que você quer provisionar. No exemplo, isso envolve a criação de uma instância EC2, mas você pode adicionar outros recursos, como redes, sub-redes, ou serviços de banco de dados.
   - O Terraform usa uma sintaxe simples e clara, facilitando a definição de ambientes de infraestrutura.

2. **Rodar `terraform plan`**:
   - Antes de aplicar qualquer mudança, o comando `terraform plan` permite visualizar o que o Terraform vai fazer. Esse é um passo essencial para garantir que o código foi escrito corretamente e que as mudanças feitas não vão afetar outros recursos de maneira inesperada.
   - O plano ajuda a identificar potenciais problemas antes de modificar a infraestrutura real.

3. **Rodar `terraform apply`**:
   - Depois de revisar o plano e ter certeza de que tudo está certo, o comando `terraform apply` é utilizado para aplicar as mudanças e criar ou modificar os recursos na nuvem.
   - Esse comando executa as ações descritas no plano e cria a infraestrutura conforme configurado no código.

---

### **Resumo do Vídeo 6: Criando sua Primeira Infraestrutura com Terraform**

Neste vídeo, aprendemos como criar uma instância EC2 na AWS usando o Terraform. O passo a passo incluiu:
- **Configuração do Terraform**, incluindo credenciais e provedores.
- **Escrita do código de configuração** usando arquivos `.tf`.
- **Execução de comandos**: `terraform init` para inicializar, `terraform plan` para visualizar o plano de execução e `terraform apply` para aplicar as mudanças e criar a infraestrutura.
  
Esse processo ajuda a entender o fluxo básico de trabalho com o Terraform, permitindo que você automatize a criação e gestão de recursos de forma segura e eficiente.