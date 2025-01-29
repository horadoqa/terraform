# AWS CLI

Para instalar a AWS CLI no Ubuntu via WSL (Windows Subsystem for Linux), siga os passos abaixo:

### 1. Abra o terminal WSL
Se ainda não tiver o WSL aberto, você pode fazê-lo procurando por "Ubuntu" no menu Iniciar do Windows e abrindo o terminal.

### 2. Atualize o sistema
É sempre bom garantir que seu sistema esteja atualizado. Execute o seguinte comando:

```bash
sudo apt update && sudo apt upgrade -y
```

### 3. Instale dependências necessárias
Antes de instalar o AWS CLI, instale algumas dependências:

```bash
sudo apt install unzip curl -y
```

### 4. Baixe e instale o AWS CLI
Baixe o instalador do AWS CLI com o comando `curl`:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

### 5. Extraia o arquivo baixado
Depois de baixar o arquivo, extraia-o com o comando `unzip`:

```bash
unzip awscliv2.zip
```

### 6. Instale o AWS CLI
Agora, execute o script de instalação:

```bash
sudo ./aws/install
```

### 7. Verifique a instalação
Para verificar se a instalação foi bem-sucedida, execute o comando:

```bash
aws --version
```

Isso deve retornar a versão do AWS CLI instalada.

### 8. (Opcional) Configure o AWS CLI
Para configurar o AWS CLI com suas credenciais da AWS, use:

```bash
aws configure
```

Você será solicitado a fornecer sua **Access Key ID**, **Secret Access Key**, **Default region name** e **Default output format**.

E pronto! Agora você tem o AWS CLI instalado e configurado no Ubuntu via WSL. Se precisar de algo mais, só avisar!