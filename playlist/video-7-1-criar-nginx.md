# TERRAFORM

## Criar a instância

```bash
terraform apply -auto-approve
```

## Acessar a instância

```bash
ssh -i ~/.ssh/id_rsa ec2-user@<IP PÚBLICO>
```

## Fazer a instalação do NGINX

```bash
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

## Acessar a Instância

http://<IP PÚBLICO>

## Destruindo a Instância

```bash
terraform destroy -auto-approve
```