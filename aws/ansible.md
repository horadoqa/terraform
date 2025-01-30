# Ansible

## Instalar

Instalar o Ansible: O Ansible está nos repositórios oficiais do Ubuntu/Debian, então você pode instalá-lo diretamente com o apt:

```bash
sudo apt install ansible
```

Verifique a instalação: Após a instalação, você pode verificar se o Ansible foi instalado corretamente executando:

```bash
ansible --version
```

## Atualizando o Ansible

```bash
pip install --upgrade ansible
```

ou

```bash
sudo apt update
sudo apt upgrade ansible
```

## Adicionar o host no arquivo de inventário

O arquivo de inventário é onde você define seus hosts e grupos de hosts. Para o seu caso, você está usando web como o grupo de hosts. Você precisa especificar o endereço IP ou o DNS da sua instância da AWS no arquivo de inventário do Ansible.

O Ansible possui um arquivo de inventário padrão, geralmente localizado em /etc/ansible/hosts, mas você pode criar um arquivo de inventário personalizado, se preferir.

[web]
100.26.112.210 ansible_ssh_user=ec2-user ansible_ssh_private_key_file=~/.ssh/id_rsa

## Validando conexão

```bash
ansible web -m ping -i /home/rfahham/projetos/terraform/aws/ansible/inventory.ini
```


## Criar o Playbook do Ansible

```yaml
---
- name: Instalar e configurar o NGINX
  hosts: web
  become: yes
  tasks:
    - name: Atualizar o sistema
      apt:
        update_cache: yes
        upgrade: yes
        cache_valid_time: 3600

    - name: Instalar o NGINX
      apt:
        name: nginx
        state: present

    - name: Garantir que o serviço NGINX esteja rodando
      service:
        name: nginx
        state: started
        enabled: yes
```

## Executar o Playbook do Ansible

ansible-playbook -i inventory.ini horadoqa.yml

terraform apply -var-file=dev.tfvars

terraform destroy -auto-approve -var-file=dev.tfvars

Acessar o site: http://<IP PÚBLICO>

terraform -chdir=terraform output instance_public_ip
[
  "52.207.254.13",
]
sed -i 's/^0\.0\.0\.0/52.207.254.13/' ./ansible/AmazonLinux/inventory.ini

 IPS=$(terraform -chdir=terraform output -json instance_public_ip | jq -r '.[0]')

 make deploy-amz