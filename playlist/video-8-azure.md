- **Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**
  - Exemplos práticos em **Azure** (máquina virtual, rede) e **Google Cloud** (instância de VM, bucket).
  - Usando o Terraform com diferentes provedores.

### **Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**

No oitavo vídeo da série, vamos expandir nosso aprendizado do Terraform para outros provedores de nuvem, como **Azure** e **Google Cloud**. Vamos ver como criar recursos nesses provedores, como **máquinas virtuais**, **redes**, **buckets** e muito mais. Além disso, exploraremos como o Terraform permite integrar e gerenciar diferentes provedores de nuvem no mesmo projeto, facilitando a automação e a consistência da infraestrutura.

#### **Exemplos Práticos em Azure e Google Cloud com Terraform**

1. **Criando Recursos no Azure**

   O Azure, da Microsoft, é outro provedor de nuvem popular que pode ser facilmente gerenciado com o Terraform. Vamos começar com um exemplo simples de criação de uma **máquina virtual** e uma **rede virtual**.

   - **Código para Criar uma Rede Virtual e uma Máquinas Virtual no Azure**:
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
     - Neste código:
       - **Resource Group**: Definimos um **grupo de recursos** no Azure.
       - **Virtual Network**: Criamos uma **rede virtual** (`azurerm_virtual_network`) com um espaço de endereço CIDR `10.0.0.0/16`.
       - **Subnet**: Criamos uma **sub-rede** dentro da rede virtual.
       - **Network Interface**: Definimos a **interface de rede** para a máquina virtual.
       - **Linux Virtual Machine**: Criamos uma **máquina virtual Linux** (`azurerm_linux_virtual_machine`) com uma interface de rede associada e configuramos o nome do administrador e a senha.

   - Após rodar `terraform apply`, o Terraform criará todos esses recursos na sua conta do Azure.

2. **Criando Recursos no Google Cloud**

   O Google Cloud (GCP) é outra plataforma de nuvem amplamente utilizada, e o Terraform facilita a criação e gerenciamento de recursos na GCP, assim como na AWS e no Azure. Vamos criar uma **instância de VM** e um **bucket do Google Cloud Storage**.

   - **Código para Criar uma Instância de VM e um Bucket no Google Cloud**:
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
     - Neste exemplo:
       - **Google Compute Instance**: Criamos uma **instância de máquina virtual** (`google_compute_instance`) no Google Cloud, usando a imagem Debian 9 e tipo de máquina `e2-medium`.
       - **Google Storage Bucket**: Criamos um **bucket de armazenamento** no Google Cloud Storage.

   - Assim como no Azure, basta rodar `terraform apply` para que o Terraform crie os recursos na sua conta do Google Cloud.

#### **Usando o Terraform com Diferentes Provedores**

Uma das grandes vantagens do Terraform é que ele permite **gerenciar recursos em múltiplos provedores de nuvem** em um único projeto. Isso significa que você pode definir e provisionar recursos em AWS, Azure, Google Cloud, e até outros provedores, como **Kubernetes** ou **Oracle Cloud**, tudo no mesmo código de configuração.

Aqui estão algumas práticas e dicas para usar múltiplos provedores no mesmo projeto Terraform:

1. **Configuração de Múltiplos Provedores**:
   Você pode configurar diferentes provedores em um único arquivo de configuração, como mostramos anteriormente nos exemplos. Basta incluir as seções de configuração de cada provedor desejado.

   - Exemplo de configuração de AWS e Google Cloud no mesmo projeto:
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

2. **Definindo Aliases para Múltiplos Provedores**:
   Quando você usa múltiplos provedores do mesmo tipo (por exemplo, duas contas AWS ou Google Cloud), você pode usar **aliases** para diferenciar as configurações.

   - Exemplo de alias para provedores:
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

3. **Organizando e Gerenciando o Código**:
   Quando você começa a trabalhar com múltiplos provedores e configurações mais complexas, é importante **organizar o código em módulos**. Isso facilita a manutenção e escalabilidade do projeto.

   - Por exemplo, você pode ter módulos separados para recursos da AWS, Azure e Google Cloud, e, em seguida, chamá-los no arquivo principal (`main.tf`).

4. **Consistência e Padronização**:
   Ao usar múltiplos provedores, é fundamental manter uma **estrutura de código consistente**. Certifique-se de definir as variáveis e outputs de forma padronizada, e de usar os mesmos princípios para criar recursos em diferentes nuvens.

#### **Resumo do Vídeo 8: Criando Recursos no Azure e Google Cloud com Terraform**

Neste vídeo, aprendemos a usar o **Terraform** para criar e gerenciar recursos no **Azure** e no **Google Cloud**. Vimos exemplos práticos de criação de **máquinas virtuais**, **redes**, **buckets de armazenamento** e outros recursos. Além disso, exploramos como o Terraform pode ser utilizado para integrar e provisionar recursos em **diferentes provedores de nuvem** dentro de um único projeto.

As principais vantagens dessa abordagem incluem a **flexibilidade** de gerenciar múltiplos provedores de nuvem de maneira centralizada e a **facilidade** de automatizar a infraestrutura de forma consistente, sem a necessidade de escrever scripts específicos para cada provedor.