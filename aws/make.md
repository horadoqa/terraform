# Como usar o Makefile:

## Para rodar o deploy para Amazon Linux:

```bash
make deploy-amz
```
## Para rodar o deploy para Ubuntu:

```bash
make deploy-ubuntu
```

## Para rodar o deploy completo (Terraform + Amazon Linux + Ubuntu):

```bash
make deploy-all
```

## Para rodar apenas o Terraform:

```bash
make terraform-apply
```

## Para rodar o playbook do Ansible para Amazon Linux separadamente:

```bash
make ansible-playbook-amz
```

## Para rodar o playbook do Ansible para Ubuntu separadamente:

```bash
make ansible-playbook-ubuntu
```