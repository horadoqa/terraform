# Acessando a infraestrutura da AWS via CLI

Para configurar o AWS CLI com o comando `aws configure`, você precisará de **credenciais de acesso** (Access Key ID e Secret Access Key) e algumas configurações adicionais. Aqui está o passo a passo para configurar:

### Passo 1: Obter suas credenciais de acesso
Antes de rodar o `aws configure`, você precisa ter as credenciais da AWS. Se você ainda não tem essas credenciais, siga os passos abaixo:

1. **Faça login na AWS Management Console**: Acesse a [AWS Management Console](https://aws.amazon.com/console/).
2. **Vá para o IAM (Identity and Access Management)**:
   - No painel de serviços, pesquise por **IAM** e clique para abrir.
3. **Crie um usuário IAM (se necessário)**:
   - No menu à esquerda, clique em **Users** e depois em **Add user**.
   - Escolha um nome de usuário e selecione a permissão (geralmente, você vai querer selecionar as permissões de **AmazonEC2FullAccess** e **S3**para ter acesso completo aos serviços).
4. **Gerar Access Key**:
   - Após criar o usuário, você verá uma seção chamada **Access keys**. Clique em **Create access key**.
   - Salve o **Access Key ID** e o **Secret Access Key** gerados. **Esses dados são sensíveis e não podem ser recuperados novamente**, então guarde-os com segurança.

### Passo 2: Configurar o AWS CLI com `aws configure`

Agora que você tem suas credenciais, você pode configurar o AWS CLI.

1. Abra o terminal (no WSL ou no Ubuntu).
2. Execute o comando:

   ```bash
   aws configure
   ```

3. Ele pedirá as seguintes informações:
   - **AWS Access Key ID**: Insira o **Access Key ID** que você obteve na criação do usuário IAM.
   - **AWS Secret Access Key**: Insira o **Secret Access Key** que você obteve na criação do usuário IAM.
   - **Default region name**: Escolha a região padrão que você deseja usar. Por exemplo, `us-east-1` (se você estiver na região Leste dos EUA) ou `sa-east-1` (se você estiver na região São Paulo).
   - **Default output format**: O formato de saída das respostas da AWS. O mais comum é **json**, mas você também pode escolher `text` ou `table`.

   Exemplo de uma configuração básica:

   ```bash
   AWS Access Key ID [None]: AKIAXXXXXXXEXAMPLE
   AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   Default region name [None]: us-east-1
   Default output format [None]: json
   ```

### Passo 3: Verificar a configuração
Depois de configurar o AWS CLI, você pode verificar se as configurações estão corretas executando:

```bash
aws sts get-caller-identity
```

Listar os Buckets

   ```bash
   aws s3 ls
   ```

Se tudo estiver configurado corretamente, você verá um retorno com seu **User ID** e **ARN** de usuário.

### Passo 4: Alterar a configuração (se necessário)
Se precisar alterar algum dado da configuração, você pode executar o comando:

```bash
aws configure
```

Ele permitirá que você edite qualquer uma das configurações (Access Key, Secret Key, região ou formato de saída).

### Dica adicional: Usando perfis diferentes
Se você tiver várias contas ou perfis, pode usar perfis diferentes com a AWS CLI. Para isso, basta usar a opção `--profile` ao executar comandos. Por exemplo:

```bash
aws s3 ls --profile meu_perfil
```

Isso permite que você tenha várias configurações em uma máquina e alterne entre elas.

Agora você está pronto para usar o AWS CLI com suas configurações! 