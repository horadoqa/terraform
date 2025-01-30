# Variáveis
TF_DIR := ./terraform
ANSIBLE_AMZ_DIR := ./ansible/AmazonLinux
TF_VAR_FILE := dev.tfvars  # Use apenas o nome do arquivo, sem o caminho extra
INVENTORY_AMZ_FILE := $(ANSIBLE_AMZ_DIR)/inventory.ini
ANSIBLE_PLAYBOOK := $(ANSIBLE_AMZ_DIR)/horadoqa.yml  # Caminho correto para o playbook

# Comandos
TF_CMD := terraform
ANSIBLE_CMD := ansible-playbook

# Targets

# Inicializar e aplicar o Terraform
.PHONY: terraform-init terraform-apply

terraform-init:
	@echo "Inicializando o Terraform..."
	cd $(TF_DIR) && $(TF_CMD) init

terraform-apply: terraform-init
	@echo "Aplicando o Terraform..."
	cd $(TF_DIR) && $(TF_CMD) apply -var-file=$(TF_VAR_FILE) -auto-approve

# Rodar o Ansible para Amazon Linux
.PHONY: ansible-playbook-amz

ansible-playbook-amz: terraform-apply
	@echo "Executando o playbook do Ansible para Amazon Linux..."
	$(ANSIBLE_CMD) -i $(INVENTORY_AMZ_FILE) $(ANSIBLE_PLAYBOOK)  # Corrigido o caminho

# Executar Terraform e Ansible para Amazon Linux
.PHONY: deploy-amz

deploy-amz: terraform-apply ansible-playbook-amz
	@echo "Deploy completo para Amazon Linux!"