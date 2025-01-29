# Instalação

Para instalar o Terraform, siga os passos abaixo dependendo do seu sistema operacional:

### 1. **No Linux (Ubuntu/Debian)**

1. **Instalar as dependências** (se necessário):
   Abra o terminal e execute:
   ```bash
   sudo apt-get update && sudo apt-get install -y wget unzip
   ```

2. **Baixar o Terraform**:
   Baixe a versão mais recente do Terraform usando o comando `wget`:
   ```bash
   wget https://releases.hashicorp.com/terraform/1.5.0/terraform_1.5.0_linux_amd64.zip
   ```

3. **Descompactar o arquivo**:
   Após o download, descompacte o arquivo ZIP:
   ```bash
   unzip terraform_1.5.0_linux_amd64.zip
   ```

4. **Mover o Terraform para um diretório no PATH**:
   Mova o binário para um diretório que esteja no seu `PATH`, como `/usr/bin`:
   ```bash
   sudo mv terraform /usr/bin/
   ```

5. **Verificar a instalação**:
   Para verificar se o Terraform foi instalado corretamente, execute:
   ```bash
   terraform --version
   ```

### 2. **No macOS**

1. **Instalar via Homebrew**:
   A maneira mais simples é usar o [Homebrew](https://brew.sh/). Se você não tiver o Homebrew instalado, instale-o primeiro. Depois, basta rodar:
   ```bash
   brew install terraform
   ```

2. **Verificar a instalação**:
   Após a instalação, execute:
   ```bash
   terraform --version
   ```

### 3. **No Windows**

1. **Baixar o arquivo ZIP**:
   Vá para a [página de lançamentos do Terraform](https://www.terraform.io/downloads) e baixe a versão para Windows.

2. **Extrair o arquivo**:
   Após o download, extraia o arquivo ZIP em uma pasta de sua escolha.

3. **Adicionar o Terraform ao PATH**:
   Para facilitar o uso do Terraform no terminal, adicione o diretório onde você extraiu o arquivo ao `PATH` do Windows:
   - Abra as configurações do sistema (digite "variáveis de ambiente" na pesquisa do Windows).
   - Em "Variáveis do sistema", clique em "Path" e depois em "Editar".
   - Adicione o caminho da pasta onde o Terraform foi extraído.

4. **Verificar a instalação**:
   Abra o Prompt de Comando (cmd) e digite:
   ```bash
   terraform --version
   ```

### 4. **Via Pacote de Gerenciamento (Linux, macOS e Windows)**

Você também pode instalar o Terraform usando o [Chocolatey](https://chocolatey.org/) (Windows) ou [Snap](https://snapcraft.io/) (Linux e macOS), mas essas são opções alternativas.

Após a instalação, basta confirmar com o comando:

```bash
terraform --version
```

Se tudo estiver certo, você verá a versão do Terraform que foi instalada.

[Próximo passo... Conhecendo os providers](providers.md)