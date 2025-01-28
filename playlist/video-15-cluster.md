#### **8. Casos de Uso Avançados**
- **Vídeo 15: Criando um Cluster Kubernetes com Terraform**
  - Passo a passo para criar e configurar um cluster Kubernetes na AWS (EKS), Azure (AKS) ou Google Cloud (GKE).
  - Integração do Terraform com Kubernetes para gerenciar recursos dentro do cluster.

### **Vídeo 15: Criando um Cluster Kubernetes com Terraform**

No **Vídeo 15**, exploramos como usar o **Terraform** para criar e configurar clusters **Kubernetes** nas principais plataformas de nuvem, como **AWS (EKS)**, **Azure (AKS)** e **Google Cloud (GKE)**. Além disso, mostramos como integrar o Terraform com o Kubernetes para gerenciar e provisionar recursos dentro do cluster de maneira eficiente e automatizada.

#### **Passo a Passo para Criar e Configurar um Cluster Kubernetes**

A criação de um cluster **Kubernetes** usando o Terraform envolve várias etapas, desde a definição de recursos necessários para o cluster até a configuração e a integração do Kubernetes com outros serviços, como balanceadores de carga, redes e armazenamentos. Vamos abordar como isso pode ser feito nas principais nuvens públicas.

##### **1. Criando um Cluster Kubernetes na AWS (EKS)**

O **Amazon Elastic Kubernetes Service (EKS)** é o serviço gerenciado da AWS para Kubernetes. O Terraform permite que você automatize a criação de clusters EKS e a configuração dos nós de trabalho (worker nodes), além de facilitar o gerenciamento de atualizações e escalabilidade.

**Exemplo de configuração para criar um cluster EKS na AWS**:

```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_eks_cluster" "example" {
  name     = "example-cluster"
  role_arn = "arn:aws:iam::123456789012:role/eks-cluster-role"
  
  vpc_config {
    subnet_ids = ["subnet-0bb1c79de3EXAMPLE", "subnet-064c5a66"]
  }
}

resource "aws_eks_node_group" "example" {
  cluster_name    = aws_eks_cluster.example.name
  node_group_name = "example-node-group"
  node_role_arn   = "arn:aws:iam::123456789012:role/eks-node-role"
  
  subnet_ids = ["subnet-0bb1c79de3EXAMPLE"]
  
  scaling_config {
    min_size     = 1
    max_size     = 3
    desired_size = 2
  }
}
```

**Explicação do código:**
- **aws_eks_cluster**: Cria o cluster Kubernetes no EKS, especificando o nome do cluster e o **role IAM** que concede as permissões necessárias.
- **aws_eks_node_group**: Configura o grupo de nós (worker nodes) que será utilizado pelo EKS, com a definição de escalabilidade (mínimo, máximo e desejado).

##### **2. Criando um Cluster Kubernetes no Azure (AKS)**

O **Azure Kubernetes Service (AKS)** é o serviço gerenciado de Kubernetes na plataforma Azure. O Terraform pode ser usado para provisionar clusters AKS com facilidade e integrar-se com o ecossistema da Azure, como redes virtuais e grupos de segurança.

**Exemplo de configuração para criar um cluster AKS na Azure**:

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_kubernetes_cluster" "example" {
  name                = "example-cluster"
  location            = "East US"
  resource_group_name = "example-rg"
  dns_prefix          = "example-dns"

  linux_profile {
    admin_username = "azureuser"
    ssh_key {
      key_data = "ssh-rsa AAAAB3... your-public-key"
    }
  }

  agent_pool_profile {
    name          = "agentpool"
    count         = 2
    vm_size       = "Standard_DS2_v2"
    os_type       = "Linux"
    vnet_subnet_id = azurerm_subnet.example.id
  }

  service_principal {
    client_id     = "your-client-id"
    client_secret = "your-client-secret"
  }
}

resource "azurerm_subnet" "example" {
  name                 = "example-subnet"
  resource_group_name  = "example-rg"
  virtual_network_name = "example-vnet"
  address_prefixes     = ["10.0.1.0/24"]
}
```

**Explicação do código:**
- **azurerm_kubernetes_cluster**: Cria o cluster Kubernetes no AKS, configurando informações de autenticação e definindo um grupo de nós.
- **agent_pool_profile**: Define a configuração dos nós de trabalho, incluindo o número de nós e o tipo de máquina virtual.
- **service_principal**: Fornece as credenciais para o Azure Active Directory (AAD) para permitir que o AKS acesse recursos no Azure.

##### **3. Criando um Cluster Kubernetes no Google Cloud (GKE)**

O **Google Kubernetes Engine (GKE)** é o serviço gerenciado de Kubernetes do Google Cloud, que oferece facilidade na criação e escalabilidade de clusters Kubernetes.

**Exemplo de configuração para criar um cluster GKE no Google Cloud**:

```hcl
provider "google" {
  project = "your-project-id"
  region  = "us-central1"
}

resource "google_container_cluster" "example" {
  name     = "example-cluster"
  location = "us-central1-a"
  
  initial_node_count = 3
  node_config {
    machine_type = "e2-medium"
    oauth_scopes = [
      "https://www.googleapis.com/auth/cloud-platform",
    ]
  }
}
```

**Explicação do código:**
- **google_container_cluster**: Cria um cluster Kubernetes no GKE com 3 nós iniciais e configura os recursos de nó (tipo de máquina e permissões OAuth).

#### **Integração do Terraform com Kubernetes para Gerenciar Recursos dentro do Cluster**

Uma vez que o cluster Kubernetes está em funcionamento, o **Terraform** pode ser usado para gerenciar recursos dentro do próprio cluster. Isso permite provisionar **pods**, **serviços**, **deployments**, **ingresses**, entre outros, diretamente no Kubernetes.

##### **Exemplo de configuração de recursos no Kubernetes com Terraform**:

Após criar o cluster Kubernetes, você pode usar o Terraform para interagir diretamente com o Kubernetes, usando o provider **kubernetes**:

```hcl
provider "kubernetes" {
  host                   = "https://<K8S_API_SERVER>"
  cluster_ca_certificate = base64decode("<BASE64_CA_CERT>")
  client_certificate     = base64decode("<BASE64_CLIENT_CERT>")
  client_key             = base64decode("<BASE64_CLIENT_KEY>")
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "example-namespace"
  }
}

resource "kubernetes_deployment" "example" {
  metadata {
    name      = "nginx"
    namespace = kubernetes_namespace.example.metadata[0].name
  }

  spec {
    replicas = 2

    selector {
      match_labels = {
        app = "nginx"
      }
    }

    template {
      metadata {
        labels = {
          app = "nginx"
        }
      }

      spec {
        container {
          name  = "nginx"
          image = "nginx:latest"
          ports {
            container_port = 80
          }
        }
      }
    }
  }
}
```

**Explicação do código:**
- **kubernetes_namespace**: Cria um **namespace** dentro do Kubernetes, ajudando a organizar os recursos.
- **kubernetes_deployment**: Define um **deployment** que irá criar 2 réplicas de um container **nginx** no Kubernetes.
  
O provider do **Terraform para Kubernetes** permite interagir com os clusters Kubernetes de forma fácil, aplicando configurações, escalando recursos e gerenciando a infraestrutura de maneira automatizada dentro do cluster.

#### **Benefícios de Usar Terraform com Kubernetes**

- **Automação**: Você pode automatizar a criação de clusters Kubernetes em diferentes provedores de nuvem, além de gerenciar todos os recursos dentro do cluster usando o Terraform.
- **Consistência**: Usando Terraform, você garante que a infraestrutura seja provisionada de forma consistente, sem discrepâncias entre ambientes.
- **Escalabilidade**: Terraform facilita a criação e a gestão de clusters que podem ser facilmente escalados, ajustando o número de nós ou recursos do Kubernetes conforme necessário.
- **Reusabilidade**: Usando módulos, você pode reutilizar configurações para criar clusters Kubernetes em diferentes nuvens ou regiões, melhorando a eficiência e a governança.

#### **Resumo do Vídeo 15: Criando um Cluster Kubernetes com Terraform**

Neste vídeo, mostramos como usar o **Terraform** para criar clusters Kubernetes gerenciados nas principais plataformas de nuvem (AWS, Azure e Google Cloud), além de integrar o Terraform com o Kubernetes para gerenciar recursos dentro do cluster. Passamos por exemplos práticos de como criar clusters EKS, AKS e GKE, e como provisionar recursos como **deployments** e **namespaces** diretamente no Kubernetes usando o Terraform. A automação proporcionada pelo Terraform facilita a escalabilidade, a consistência e a governança dos recursos em nuvem, simplificando a gestão da infraestrutura Kubernetes.