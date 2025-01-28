#### **7. Monitoramento, Segurança e Governança**
- **Vídeo 13: Monitorando a Infraestrutura com Terraform**
  - Como integrar o Terraform com ferramentas de monitoramento (ex: Prometheus, CloudWatch).
  - Automação para alertas e log de alterações.

### **Vídeo 13: Monitorando a Infraestrutura com Terraform**

No décimo terceiro vídeo da série, exploramos como integrar o **Terraform** com ferramentas de **monitoramento** e **alertas** para garantir que sua infraestrutura esteja sempre sob controle, identificando problemas rapidamente e respondendo a mudanças de forma eficiente. Monitorar a infraestrutura é uma parte crucial para garantir a **disponibilidade**, **segurança** e **performance** dos recursos provisionados com Terraform.

#### **Como Integrar o Terraform com Ferramentas de Monitoramento**

O monitoramento de infraestrutura permite que você visualize e controle o desempenho dos recursos, identificando potenciais problemas antes que afetem os usuários ou sistemas. Ferramentas como **Prometheus**, **CloudWatch**, **Datadog**, e outras, podem ser integradas ao Terraform para automatizar a criação de métricas, alarmes e dashboards, garantindo que a infraestrutura esteja sendo monitorada adequadamente.

1. **Prometheus**:
   O **Prometheus** é uma ferramenta popular de monitoramento open-source para métricas de sistemas e aplicações. Ele coleta e armazena métricas de diferentes fontes e oferece um poderoso sistema de consulta. O Terraform pode ser usado para configurar o **Prometheus** em ambientes de nuvem, além de configurar as métricas a serem coletadas.

   Exemplo de configuração para criar um **alarme** no Prometheus para monitorar a CPU de uma instância EC2 no AWS:
   
   ```hcl
   resource "aws_cloudwatch_metric_alarm" "cpu_alarm" {
     alarm_name                = "HighCPUUsage"
     comparison_operator       = "GreaterThanThreshold"
     evaluation_periods        = 1
     metric_name               = "CPUUtilization"
     namespace                 = "AWS/EC2"
     period                    = 300
     statistic                 = "Average"
     threshold                 = 80
     alarm_description         = "This metric monitors EC2 CPU usage"
     dimensions = {
       InstanceId = "i-1234567890abcdef0"
     }

     alarm_actions = [
       "arn:aws:sns:us-east-1:123456789012:MyTopic"
     ]
   }
   ```

   - **Metric Alarm**: Aqui, estamos criando um alarme que será disparado se o uso de CPU de uma instância EC2 ultrapassar 80% por 5 minutos.
   - **SNS Action**: Caso o alarme seja disparado, ele envia uma notificação para um tópico SNS (Simple Notification Service).

2. **CloudWatch (AWS)**:
   O **Amazon CloudWatch** é uma ferramenta da AWS para monitoramento de recursos e aplicações. Com o Terraform, você pode configurar alarmes, logs e dashboards para observar métricas como uso de CPU, memória, rede, entre outras.

   Exemplo de configuração de alarme no **CloudWatch** com Terraform:

   ```hcl
   resource "aws_cloudwatch_metric_alarm" "high_cpu" {
     alarm_name                = "HighCPUUsage"
     comparison_operator       = "GreaterThanThreshold"
     evaluation_periods        = 1
     metric_name               = "CPUUtilization"
     namespace                 = "AWS/EC2"
     period                    = 300
     statistic                 = "Average"
     threshold                 = 80
     alarm_description         = "Alarme de alta utilização de CPU"
     dimensions = {
       InstanceId = "i-0abcdef1234567890"
     }
   }
   ```

   Aqui, o Terraform está configurando um alarme para monitorar a utilização da CPU de uma instância EC2 na AWS. O alarme será disparado se o uso de CPU ultrapassar 80% por um período de 5 minutos.

3. **Datadog**:
   O **Datadog** é uma plataforma popular de monitoramento que coleta métricas de infraestrutura, logs e traces de aplicações. Ele também oferece alertas baseados em métricas e logs. O Terraform pode ser usado para integrar a infraestrutura provisionada com o Datadog, criando métricas personalizadas e alertas automaticamente.

   Exemplo de configuração de monitoramento no Datadog:

   ```hcl
   resource "datadog_monitor" "high_cpu" {
     name               = "High CPU Usage"
     type               = "metric alert"
     query              = "avg(last_5m):avg:aws.ec2.cpuutilization{instance_id:i-1234567890abcdef0} > 80"
     message            = "A instância EC2 está com alta utilização de CPU!"
     tags               = ["environment:production"]
     thresholds {
       critical          = 80
     }
   }
   ```

   - **Query**: A consulta `avg(last_5m):avg:aws.ec2.cpuutilization{instance_id:i-1234567890abcdef0}` verifica a média de utilização de CPU de uma instância EC2 nos últimos 5 minutos.
   - **Thresholds**: Se o valor médio da CPU for superior a 80%, um alarme será gerado.

#### **Automação para Alertas e Log de Alterações**

O Terraform permite integrar alertas e logs automaticamente para monitorar alterações e operações nos recursos de infraestrutura. Isso é essencial para auditoria e para detectar e corrigir rapidamente problemas em produção.

1. **Logs de alterações (Terraform Cloud e Enterprise)**:
   Quando você usa o **Terraform Cloud** ou **Terraform Enterprise**, as mudanças feitas na infraestrutura são registradas automaticamente, com detalhes de quem fez a mudança, quando e o que foi alterado. Isso é essencial para auditoria e governança.

   O Terraform também pode ser configurado para **registrar logs** em tempo real usando ferramentas como **AWS CloudTrail**, **Google Cloud Logging**, ou **Azure Monitor**. Isso ajuda a rastrear mudanças nos recursos e pode ser integrado a sistemas de alertas.

2. **Alertas para Falhas em Pipelines de Terraform**:
   Uma prática comum ao usar Terraform em pipelines de **CI/CD** é configurar **alertas** para falhas em operações como `terraform apply`. Isso pode ser feito através de ferramentas de monitoramento como o **Slack** ou o **Email**, configurando ações baseadas em falhas nos pipelines de CI/CD.

   Exemplo de configuração de alerta no **GitLab CI**:
   
   ```yaml
   stages:
     - plan
     - apply

   terraform_plan:
     stage: plan
     script:
       - terraform plan
     when: on_failure
     notifications:
       email:
         - "devteam@example.com"

   terraform_apply:
     stage: apply
     script:
       - terraform apply -auto-approve
     when: on_failure
     notifications:
       email:
         - "devteam@example.com"
   ```

   Com esse exemplo, o GitLab enviará um alerta por e-mail caso qualquer etapa da pipeline falhe.

3. **Integração com Sistemas de Notificação**:
   Terraform pode ser integrado a sistemas de notificação como **Slack**, **Email**, ou **Webhook**, para enviar alertas sobre mudanças e status da infraestrutura. Por exemplo, você pode configurar o Terraform para enviar uma mensagem para um canal do Slack sempre que uma mudança significativa for aplicada.

   Exemplo de alerta via **Slack** após execução do Terraform:

   ```hcl
   resource "slack_incoming_webhook" "terraform_alert" {
     url = "https://hooks.slack.com/services/XXXXX/XXXXX/XXXXX"
     message = "Mudança aplicada com sucesso ao Terraform!"
   }
   ```

#### **Boas Práticas para Monitoramento e Governança de Infraestrutura**

1. **Defina métricas chave**: Escolha métricas críticas que afetam a performance e a disponibilidade da infraestrutura, como utilização de CPU, uso de disco, latência de rede, etc. 

2. **Automatize alertas**: Configure alertas automáticos para detectar anomalias e falhas rapidamente. Certifique-se de que esses alertas sejam enviados para os canais apropriados, como e-mail ou Slack, e que os responsáveis pela infraestrutura sejam notificados imediatamente.

3. **Armazenamento centralizado de logs**: Utilize ferramentas como **ELK Stack**, **Splunk** ou **CloudWatch Logs** para armazenar logs de eventos e operações de infraestrutura em um local centralizado. Isso facilita auditoria, depuração e análise.

4. **Gerenciamento de IAM e Acesso**: Configure as permissões e controle de acesso (via IAM) para garantir que apenas as pessoas certas possam modificar e acessar os recursos monitorados. A governança de acesso é fundamental para a segurança da infraestrutura.

5. **Auditoria e Compliance**: Mantenha registros de todas as mudanças e operações feitas com Terraform para auditoria e conformidade com políticas internas e externas. Ferramentas como **AWS CloudTrail** e **Azure Activity Logs** são ideais para este fim.

#### **Resumo do Vídeo 13: Monitorando a Infraestrutura com Terraform**

Neste vídeo, aprendemos como integrar o **Terraform** com ferramentas de **monitoramento** como **Prometheus**, **CloudWatch**, e **Datadog**, além de configurar **alertas** automáticos e **logs** para garantir que a infraestrutura esteja sob controle contínuo. A integração de monitoramento e alertas ajuda a identificar falhas, melhorar a segurança, e garantir que a infraestrutura seja resiliente e bem gerenciada. Ao automatizar esses processos, você garante uma operação de infraestrutura mais eficiente, segura e escalável.