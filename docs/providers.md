# Providers

## Configuração do Providers (provedores)

Os provedores permitem que o Terraform interaja com provedores de nuvem, provedores de SaaS e outras APIs.

Alguns provedores exigem que você os configure com URLs de endpoint, regiões de nuvem ou outras configurações antes que o Terraform possa usá-los. Esta página documenta como configurar definições para provedores.

Além disso, todas as configurações do Terraform devem declarar quais provedores eles exigem para que o Terraform possa instalá-los e usá-los. A página Provider Requirements documenta como declarar provedores para que o Terraform possa instalá-los.

Aqui está uma lista das principais provedores de nuvem e exemplos de seus **providers** no Terraform:

### 1. **Amazon Web Services (AWS)**

- **Provider**: `aws`
- **Descrição**: O AWS é uma das plataformas de nuvem mais populares, oferecendo uma vasta gama de serviços, como computação, armazenamento, redes e banco de dados.
  
  **Exemplo de uso**:
  ```hcl
  provider "aws" {
    region = "us-east-1"
  }

  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"
  }
  ```

### 2. **Microsoft Azure**

- **Provider**: `azurerm`
- **Descrição**: A plataforma de nuvem da Microsoft, que oferece serviços de computação, armazenamento, redes, inteligência artificial e mais.

  **Exemplo de uso**:
  ```hcl
  provider "azurerm" {
    features {}
  }

  resource "azurerm_virtual_machine" "example" {
    name                = "example-vm"
    location            = "East US"
    resource_group_name = "example-resources"
    network_interface_ids = [azurerm_network_interface.example.id]
    vm_size             = "Standard_B1s"

    storage_os_disk {
      name              = "myosdisk1"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed           = true
    }
  }
  ```

### 3. **Google Cloud Platform (GCP)**

- **Provider**: `google`
- **Descrição**: O GCP oferece uma ampla gama de serviços, incluindo computação, rede, análise de dados, IA e mais, com forte integração com tecnologias como Kubernetes.

  **Exemplo de uso**:
  ```hcl
  provider "google" {
    project = "my-project-id"
    region  = "us-central1"
  }

  resource "google_compute_instance" "default" {
    name         = "example-instance"
    machine_type = "f1-micro"
    zone         = "us-central1-a"

    boot_disk {
      initialize_params {
        image = "debian-9-stretch-v20191210"
      }
    }
  }
  ```

### 4. **Oracle Cloud Infrastructure (OCI)**

- **Provider**: `oci`
- **Descrição**: O OCI oferece recursos de nuvem de alto desempenho, com forte foco em máquinas virtuais, redes, bancos de dados e armazenamento.

  **Exemplo de uso**:
  ```hcl
  provider "oci" {
    region = "us-phoenix-1"
  }

  resource "oci_core_instance" "example" {
    availability_domain = "Uocm:PHX-AD-1"
    compartment_id      = "ocid1.compartment.oc1..aaaaaaaaargh"
    shape               = "VM.Standard2.1"
    display_name        = "example-instance"

    create_vnic_details {
      subnet_id = "ocid1.subnet.oc1..aaaaaaaabbbbbbb"
    }
  }
  ```

### 5. **IBM Cloud**

- **Provider**: `ibm`
- **Descrição**: A IBM Cloud oferece uma plataforma de nuvem híbrida com suporte para computação, armazenamento, redes, AI e ferramentas de DevOps.

  **Exemplo de uso**:
  ```hcl
  provider "ibm" {
    ibmcloud_api_key = "your-ibm-cloud-api-key"
  }

  resource "ibm_is_instance" "example" {
    name             = "example-vm"
    image             = "rhel-7.8-x64-1"
    profile           = "bx2-2x8"
    zone              = "us-south-1"
    vpc               = "vpc-id"
    subnet            = "subnet-id"
  }
  ```

### 6. **Alibaba Cloud**

- **Provider**: `alicloud`
- **Descrição**: O Alibaba Cloud oferece serviços globais de nuvem com foco na Ásia e oferece uma ampla gama de produtos, incluindo computação, redes e banco de dados.

  **Exemplo de uso**:
  ```hcl
  provider "alicloud" {
    access_key = "your-access-key"
    secret_key = "your-secret-key"
    region     = "cn-beijing"
  }

  resource "alicloud_instance" "example" {
    instance_name = "example-instance"
    image_id      = "ubuntu_18_04_64_20G_alibase_20201029.vhd"
    instance_type = "ecs.t5-lc2m1.nano"
  }
  ```

### 7. **DigitalOcean**

- **Provider**: `digitalocean`
- **Descrição**: O DigitalOcean oferece serviços de nuvem simples e escaláveis, focados em desenvolvedores e pequenas empresas.

  **Exemplo de uso**:
  ```hcl
  provider "digitalocean" {
    token = "your-digitalocean-token"
  }

  resource "digitalocean_droplet" "example" {
    name   = "example-droplet"
    image  = "ubuntu-20-04-x64"
    region = "nyc1"
    size   = "s-1vcpu-1gb"
  }
  ```

### 8. **VMware vSphere**

- **Provider**: `vsphere`
- **Descrição**: O vSphere da VMware permite criar e gerenciar máquinas virtuais em ambientes on-premises ou em nuvem híbrida.

  **Exemplo de uso**:
  ```hcl
  provider "vsphere" {
    user           = "user"
    password       = "password"
    vsphere_server = "vsphere.example.com"
  }

  resource "vsphere_virtual_machine" "example" {
    name             = "example-vm"
    resource_pool_id = data.vsphere_compute_cluster.cluster.resource_pool_id
    datastore_id     = data.vsphere_datastore.datastore.id

    num_cpus = 2
    memory   = 4096
    disk {
      size = 20
    }
  }
  ```

### 9. **Kubernetes (para gerenciar clusters Kubernetes em qualquer nuvem)**

- **Provider**: `kubernetes`
- **Descrição**: O Terraform também pode ser usado para gerenciar clusters Kubernetes em qualquer nuvem pública ou privada, usando o provider `kubernetes`.

  **Exemplo de uso**:
  ```hcl
  provider "kubernetes" {
    host                   = "https://your-k8s-cluster-url"
    cluster_ca_certificate = base64decode("your-cluster-ca-cert")
    token                  = "your-token"
  }

  resource "kubernetes_pod" "example" {
    metadata {
      name = "example-pod"
    }

    spec {
      container {
        name  = "nginx"
        image = "nginx:latest"
      }
    }
  }
  ```

### 10. **Huawei Cloud**

- **Provider**: `huaweicloud`
- **Descrição**: A Huawei Cloud fornece uma variedade de serviços de nuvem pública, privada e híbrida com foco em áreas como IA, análise de dados e redes.

  **Exemplo de uso**:
  ```hcl
  provider "huaweicloud" {
    region = "cn-north-4"
    ak     = "your-access-key"
    sk     = "your-secret-key"
  }

  resource "huaweicloud_compute_instance_v2" "example" {
    name          = "example-instance"
    image_id      = "ubuntu-20-04"
    flavor_id     = "s3.large.2"
    availability_zone = "cn-north-4a"
  }
  ```

### 11. **Oracle Cloud Infrastructure (OCI)**

- **Provider**: `oci`
- **Descrição**: A OCI é a plataforma de nuvem da Oracle. O provider oci permite a criação, gestão e configuração de recursos na Oracle Cloud, como instâncias de computação, redes, armazenamento, bancos de dados, entre outros.

  **Exemplo de uso**:
  ```hcl
  provider "oci" {
  region = "us-phoenix-1"
  tenancy_ocid = "ocid1.tenancy.oc1..example"
  user_ocid = "ocid1.user.oc1..example"
  fingerprint = "your-api-key-fingerprint"
  private_key_path = "/path/to/your/private/key.pem"
}

resource "oci_core_instance" "example" {
  availability_domain = "Uocm:PHX-AD-1"
  compartment_id      = "ocid1.compartment.oc1..example"
  shape               = "VM.Standard2.1"
  display_name        = "example-instance"

  create_vnic_details {
    subnet_id = "ocid1.subnet.oc1..example"
  }
  
  metadata = {
    ssh_authorized_keys = "ssh-rsa AAAA...your-public-key..."
  }
}
  ```
  
---

Esses são alguns dos principais provedores de nuvem que o Terraform suporta, cada um com suas particularidades e uma ampla gama de recursos. O Terraform permite gerenciar esses recursos de maneira unificada, o que facilita a automação e a manutenção de infraestrutura em múltiplas nuvens.
