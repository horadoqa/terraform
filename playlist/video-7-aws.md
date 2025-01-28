- **Vídeo 7: Como Criar Infraestrutura na AWS com Terraform**
  - Exemplos práticos de provisionamento de recursos AWS (ex: EC2, VPC, S3).
  - Boas práticas de organização de código: módulos, variáveis, outputs.

### **Vídeo 7: Como Criar Infraestrutura na AWS com Terraform**

No sétimo vídeo da nossa série, vamos aprender a criar recursos mais complexos na **AWS** com o **Terraform**. A ideia é partir de exemplos práticos de **provisionamento de infraestrutura** para recursos como **EC2**, **VPC** e **S3**, além de discutir **boas práticas** para organizar o código Terraform de forma eficiente, utilizando **módulos**, **variáveis** e **outputs**. Isso ajudará a tornar a infraestrutura mais modular, reutilizável e escalável.

#### **Exemplos Práticos de Provisionamento de Recursos na AWS**

1. **Criando uma Instância EC2**
   
   Vamos começar com um exemplo simples, criando uma **instância EC2** na AWS. Esse é um dos recursos mais comuns quando se trabalha com Terraform na AWS.

   - **Código para Criar uma Instância EC2**:
     ```hcl
     provider "aws" {
       region = "us-east-1"
     }

     resource "aws_instance" "example" {
       ami           = "ami-0c55b159cbfafe1f0"  # ID da AMI
       instance_type = "t2.micro"              # Tipo de instância
     }
     ```
     - Neste exemplo, estamos criando uma **instância EC2** com a AMI e tipo de instância especificados.
     - Após rodar `terraform apply`, o Terraform provisionará essa instância na região definida.

2. **Criando uma VPC**

   A **VPC** (Virtual Private Cloud) é um componente fundamental para isolar e organizar recursos dentro da AWS. Vamos criar uma VPC simples com uma **sub-rede**.

   - **Código para Criar uma VPC e Sub-rede**:
     ```hcl
     resource "aws_vpc" "main" {
       cidr_block = "10.0.0.0/16"
     }

     resource "aws_subnet" "subnet" {
       vpc_id     = aws_vpc.main.id
       cidr_block = "10.0.1.0/24"
     }
     ```
     - Neste exemplo, estamos criando uma **VPC** com um bloco CIDR de `10.0.0.0/16` e uma **sub-rede** com o CIDR `10.0.1.0/24`.
     - O Terraform irá provisionar esses recursos na AWS, criando uma rede isolada onde podemos conectar outros recursos, como a instância EC2.

3. **Criando um Bucket S3**

   O **S3** (Simple Storage Service) é utilizado para armazenar dados, como arquivos estáticos e backups. Vamos criar um bucket S3.

   - **Código para Criar um Bucket S3**:
     ```hcl
     resource "aws_s3_bucket" "example" {
       bucket = "my-unique-bucket-name-12345"
     }
     ```
     - Neste código, estamos criando um **bucket S3** com o nome especificado. Lembre-se de que o nome do bucket precisa ser único globalmente.
     - Depois de rodar `terraform apply`, o bucket será criado na AWS e estará pronto para uso.

#### **Boas Práticas de Organização de Código: Módulos, Variáveis e Outputs**

À medida que sua infraestrutura cresce, manter o código organizado e reutilizável torna-se cada vez mais importante. Vamos explorar as boas práticas de organização de código no Terraform: **módulos**, **variáveis** e **outputs**.

1. **Módulos**

   - Os **módulos** são a chave para tornar seu código Terraform **reutilizável e modular**. Um módulo pode ser uma coleção de recursos relacionados que são agrupados em um único arquivo ou diretório. 
   - Utilizar módulos permite que você abstraia partes do código, tornando-o mais organizado e fácil de manter.
   
   - **Exemplo de Módulo**: Criação de um módulo para provisionar uma instância EC2:
     - **Estrutura de Diretórios**:
       ```
       ├── main.tf
       └── modules
           └── ec2_instance
               ├── main.tf
               └── outputs.tf
       ```

     - **Código do módulo (modules/ec2_instance/main.tf)**:
       ```hcl
       resource "aws_instance" "example" {
         ami           = var.ami
         instance_type = var.instance_type
       }
       ```

     - **Código do módulo (modules/ec2_instance/outputs.tf)**:
       ```hcl
       output "instance_id" {
         value = aws_instance.example.id
       }
       ```

     - **Código principal (main.tf)**:
       ```hcl
       provider "aws" {
         region = "us-east-1"
       }

       module "ec2_instance" {
         source        = "./modules/ec2_instance"
         ami           = "ami-0c55b159cbfafe1f0"
         instance_type = "t2.micro"
       }
       ```

     Nesse exemplo, criamos um módulo **ec2_instance** que pode ser reutilizado em diferentes projetos. O código principal faz referência ao módulo, passando as variáveis necessárias (como `ami` e `instance_type`).

2. **Variáveis**

   As **variáveis** são uma ótima maneira de tornar seu código Terraform mais **flexível e reutilizável**. Você pode definir variáveis no início do seu código e passar valores para elas durante a execução do Terraform.

   - **Definição de variáveis**:
     ```hcl
     variable "ami" {
       description = "AMI ID"
       type        = string
     }

     variable "instance_type" {
       description = "Tipo da instância EC2"
       type        = string
     }
     ```

   - **Uso de variáveis** no código:
     ```hcl
     resource "aws_instance" "example" {
       ami           = var.ami
       instance_type = var.instance_type
     }
     ```

   - **Passando valores para as variáveis**:
     Você pode passar valores para as variáveis de várias formas:
     - Através de um arquivo `terraform.tfvars`.
     - No comando `terraform apply` usando a flag `-var`:
       ```bash
       terraform apply -var="ami=ami-0c55b159cbfafe1f0" -var="instance_type=t2.micro"
       ```

3. **Outputs**

   **Outputs** são úteis para exibir informações importantes depois que o Terraform aplica as mudanças, como IDs de recursos, URLs de serviços ou outros valores que você deseja acompanhar.

   - **Exemplo de Outputs**:
     ```hcl
     output "instance_id" {
       value = aws_instance.example.id
     }
     ```

   - Isso permite que, depois de executar `terraform apply`, você veja o valor de `instance_id` diretamente na saída do terminal. Também é possível acessar esse valor em outros módulos ou em outras execuções do Terraform.

#### **Resumo das Boas Práticas**

- **Módulos**: Use módulos para organizar e reutilizar seu código. Divida sua infraestrutura em partes menores e mais gerenciáveis.
- **Variáveis**: Defina variáveis para tornar seu código mais flexível. Isso permite passar valores dinâmicos e manter o código mais limpo.
- **Outputs**: Utilize outputs para expor informações importantes, como IDs de recursos, e para facilitar o uso desses dados em outros módulos ou sistemas.

---

### **Resumo do Vídeo 7: Como Criar Infraestrutura na AWS com Terraform**

Neste vídeo, vimos exemplos práticos de como criar **recursos na AWS** com o Terraform, como **instâncias EC2**, **VPCs** e **S3**. Além disso, exploramos as **boas práticas de organização de código**, como o uso de **módulos**, **variáveis** e **outputs**.

Essas práticas não apenas tornam seu código mais organizado e reutilizável, mas também ajudam a escalar e manter a infraestrutura de forma eficiente. O uso de módulos, variáveis e outputs é fundamental para garantir que seu código seja flexível, modular e fácil de gerenciar à medida que sua infraestrutura cresce.