- **Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**
  - Exemplos práticos em **Azure** (máquina virtual, rede) e **Google Cloud** (instância de VM, bucket).
  - Usando o Terraform com diferentes provedores.

### **Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**

No oitavo vídeo da nossa série, exploraremos como usar o **Terraform** para criar e gerenciar recursos na **Azure** e no **Google Cloud**. Vamos trabalhar com exemplos práticos de criação de **máquinas virtuais**, **redes**, **buckets de armazenamento**, e outros recursos essenciais, e também mostrar como o Terraform pode integrar múltiplos provedores de nuvem no mesmo projeto, facilitando a automação e gestão de infraestrutura de maneira eficiente.

#### **Exemplos Práticos em Azure e Google Cloud com Terraform**

1. **Criando Recursos no Azure**

   O **Azure**, da Microsoft, é uma das maiores plataformas de nuvem pública e oferece uma variedade de serviços que podem ser facilmente provisionados e gerenciados com o **Terraform**. Vamos começar criando uma **máquina virtual** (VM) e uma **rede virtual** (VNet), que são componentes essenciais para a maioria das arquiteturas de nuvem.

   - **Criando uma Rede Virtual e uma Máquinas Virtual no Azure**:
     ```hcl
     provider "azurerm" {
       features {}
     }

     resource "azurerm_resource_group" "example" {
       name     = "example-resources"
       location = "East US"
     }

     resource "azurerm_virtual_network" "example_vnet" {
       name                = "example-vnet"
       location            = azurerm_resource_group.example.location
       resource_group_name = azurerm_resource_group.example.name
       address_space       = ["10.0.0.0/16"]
     }

     resource "azurerm_subnet" "example_subnet" {
       name                 = "example-subnet"
       resource_group_name  = azurerm_resource_group.example.name
       virtual_network_name = azurerm_virtual_network.example_vnet.name
       address_prefixes     = ["10.0.1.0/24"]
     }

     resource "azurerm_network_interface" "example_nic" {
       name                = "example-nic"
       location            = azurerm_resource_group.example.location
       resource_group_name = azurerm_resource_group.example.name
       subnet_id           = azurerm_subnet.example_subnet.id
     }

     resource "azurerm_linux_virtual_machine" "example_vm" {
       name                = "example-vm"
       resource_group_name = azurerm_resource_group.example.name
       location            = azurerm_resource_group.example.location
       size                = "Standard_F2"
       network_interface_ids = [
         azurerm_network_interface.example_nic.id,
       ]
       admin_username = "adminuser"
       admin_password = "password123"
       image_id       = "/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Compute/images/<image_name>"
     }
     ```
     - Nesse código:
       - Criamos um **resource group** no Azure.
       - Configuramos uma **rede virtual** e uma **sub-rede**.
       - Criamos uma **interface de rede** associada à sub-rede.
       - Por fim, provisionamos uma **máquina virtual Linux** dentro dessa rede, definindo o nome do administrador e a senha.

   - Após rodar `terraform apply`, o Terraform provisionará os recursos especificados na sua conta Azure.

2. **Criando Recursos no Google Cloud**

   O **Google Cloud** oferece uma ampla gama de serviços para computação, armazenamento e redes, e o Terraform também facilita a criação e o gerenciamento desses recursos na GCP (Google Cloud Platform). Vamos criar uma **instância de VM** e um **bucket do Google Cloud Storage**.

   - **Criando uma Instância de VM e um Bucket no Google Cloud**:
     ```hcl
     provider "google" {
       project = "your-project-id"
       region  = "us-central1"
     }

     resource "google_compute_instance" "example" {
       name         = "example-instance"
       machine_type = "e2-medium"
       zone         = "us-central1-a"

       boot_disk {
         initialize_params {
           image = "debian-9-stretch-v20191210"
         }
       }

       network_interface {
         network = "default"
         access_config {
           // Ephemeral IP
         }
       }
     }

     resource "google_storage_bucket" "example" {
       name     = "example-bucket-12345"
       location = "US"
     }
     ```
     - Aqui, criamos:
       - Uma **instância de VM** na **Google Compute Engine**, especificando o tipo de máquina `e2-medium` e a imagem do sistema operacional Debian.
       - Um **bucket de armazenamento** no Google Cloud Storage, especificando o nome e a localização do bucket.
   
   - O comando `terraform apply` criará esses recursos na sua conta do Google Cloud.

#### **Usando o Terraform com Diferentes Provedores**

Uma das grandes vantagens do **Terraform** é a capacidade de **gerenciar múltiplos provedores de nuvem** em um único projeto. Isso permite que você provisionar recursos de diferentes provedores (como AWS, Azure, Google Cloud, entre outros) dentro de uma mesma infraestrutura, de forma coesa e consistente.

1. **Gerenciando Múltiplos Provedores no Mesmo Código**:
   Você pode facilmente configurar e usar diferentes provedores no mesmo arquivo de configuração do Terraform. O Terraform permite que você defina múltiplos blocos de provedor e utilize esses provedores para criar recursos de forma simultânea e sem a necessidade de scripts separados.

   - **Exemplo de múltiplos provedores (AWS e Google Cloud)**:
     ```hcl
     provider "aws" {
       region = "us-west-2"
     }

     provider "google" {
       project = "your-project-id"
       region  = "us-central1"
     }

     resource "aws_instance" "example" {
       ami           = "ami-0c55b159cbfafe1f0"
       instance_type = "t2.micro"
     }

     resource "google_compute_instance" "example" {
       name         = "example-instance"
       machine_type = "e2-medium"
       zone         = "us-central1-a"
     }
     ```

   - Neste exemplo, temos a configuração para o **AWS** e o **Google Cloud**, e o Terraform irá provisionar as instâncias de acordo com a configuração definida para cada provedor.

2. **Usando Aliases para Provedores Múltiplos**:
   Quando você precisa usar diferentes **contas** ou configurações de um mesmo provedor (como várias contas da AWS ou Google Cloud), você pode utilizar **aliases** para diferenciá-los.

   - **Exemplo com aliases**:
     ```hcl
     provider "aws" {
       alias  = "us_west"
       region = "us-west-2"
     }

     provider "aws" {
       alias  = "us_east"
       region = "us-east-1"
     }

     resource "aws_instance" "west" {
       provider      = aws.us_west
       ami           = "ami-0c55b159cbfafe1f0"
       instance_type = "t2.micro"
     }

     resource "aws_instance" "east" {
       provider      = aws.us_east
       ami           = "ami-0c55b159cbfafe1f0"
       instance_type = "t2.micro"
     }
     ```

   - Nesse código, estamos utilizando dois aliases diferentes para o provedor **AWS**, o que permite criar instâncias em **duas regiões** diferentes da AWS.

3. **Benefícios de Usar Múltiplos Provedores**:
   - **Flexibilidade**: Usar múltiplos provedores no mesmo projeto permite escolher os melhores serviços de cada provedor para diferentes partes da sua infraestrutura.
   - **Consistência**: O Terraform garante que os recursos serão criados de forma consistente, independente do provedor de nuvem.
   - **Facilidade de Integração**: O Terraform permite integrar os recursos de diferentes provedores de forma simples e sem a necessidade de scripts separados.

#### **Resumo do Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**

Neste vídeo, vimos como usar o **Terraform** para provisionar recursos tanto no **Azure** quanto no **Google Cloud**, criando exemplos de **máquinas virtuais**, **redes** e **buckets de armazenamento**. Aprendemos também a como o Terraform facilita o uso de **múltiplos provedores** em um único projeto, possibilitando a criação de uma infraestrutura que abrange diferentes provedores de nuvem de forma simples e organizada.

O uso de múltiplos provedores no Terraform não só torna sua infraestrutura mais flexível, mas também permite que você aproveite os melhores recursos de cada plataforma de nuvem, criando soluções mais eficientes e escaláveis.