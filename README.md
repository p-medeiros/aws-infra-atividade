Configuração de Infraestrutura Básica na AWS
📌 Objetivo
Criar uma infraestrutura básica na AWS, incluindo Security Groups, uma instância EC2 e um banco de dados gerenciado via RDS. A atividade visa consolidar conhecimentos sobre componentes essenciais da AWS e boas práticas de segurança.

📌 Pré-requisitos

O AWS CLI configurado.
Acesso ao console da AWS.
Um par de chaves SSH para acessar a instância EC2.

📌 Etapas
1️⃣ Criar Security Groups (Grupos de Segurança)
Os Security Groups (SG) são regras de firewall que controlam o tráfego de entrada e saída das instâncias e bancos de dados.

🔹 Security Group para EC2:
Acesse o console da AWS e vá até o serviço EC2.
No menu lateral, clique em Security Groups.
Clique no botão Create security group.
Preencha as seguintes informações:
Name: sg-ec2
Description: Security Group para a instância EC2.
VPC: Selecione a VPC padrão ou uma existente.
Em Inbound rules (regras de entrada), adicione as seguintes regras:
SSH (porta 22):
Tipo: SSH
Protocolo: TCP
Porta: 22
Origem: Seu IP pessoal (My IP)
HTTP (porta 80):
Tipo: HTTP
Protocolo: TCP
Porta: 80
Origem: Anywhere (0.0.0.0/0)
Clique em Create security group.
🔹 Security Group para RDS:
Ainda na página de Security Groups, clique em Create security group.
Preencha os seguintes detalhes:
Name: sg-rds
Description: Security Group para o banco RDS.
VPC: A mesma VPC usada para o SG da EC2.
Em Inbound rules, adicione:
PostgreSQL (porta 5432):
Tipo: PostgreSQL
Protocolo: TCP
Porta: 5432
Origem: Security Group da EC2 (sg-ec2).
Clique em Create security group.
2️⃣ Criar uma Instância EC2
Acesse o console da AWS e vá até EC2 > Instances.
Clique em Launch Instance.
Configure os seguintes parâmetros:
Name: MinhaEC2
AMI (Amazon Machine Image): Selecione Amazon Linux 2
Instance Type: t2.micro (grátis no Free Tier)
Key Pair: Escolha um existente ou crie um novo .pem
Security Group: Selecione o sg-ec2 criado anteriormente.
Clique em Launch Instance.
Após a criação, copie o Endereço IPv4 público da instância para futura conexão SSH.
🔹 Acessar a instância EC2 via SSH:
Linux/macOS:
ssh -i minha-chave.pem ec2-user@IP_DA_INSTANCIA

Windows (usando PowerShell):
ssh -i minha-chave.pem ec2-user@IP_DA_INSTANCIA

3️⃣ Criar um Banco de Dados RDS (PostgreSQL)
No console da AWS, acesse RDS > Databases.
Clique em Create database.
Em Database creation method, escolha Standard Create.
Selecione o PostgreSQL como o motor de banco de dados.
Escolha a versão mais recente do PostgreSQL.
Em Templates, selecione Free Tier.
Configure os detalhes do banco:
DB instance identifier: meubanco
Master username: admin
Master password: ************
Confirm password: ************
Em Connectivity, faça as seguintes configurações:
VPC: Escolha a mesma usada para a EC2.
Public access: No (o banco só será acessível pela EC2).
VPC Security Groups: Selecione sg-rds criado anteriormente.
Clique em Create database.
🔹 Obter o endpoint do banco:
Vá até RDS > Databases.
Clique no banco criado (meubanco).
Anote o endpoint gerado (exemplo: meubanco.xxxxxxx.us-east-1.rds.amazonaws.com).
📌 Testando a Conexão ao Banco de Dados

Após acessar a EC2 via SSH, instale o PostgreSQL client:
sudo yum install -y postgresql
Conecte ao banco RDS usando o seguinte comando:

psql -h ENDPOINT_DO_BANCO -U admin -d postgres
Digite a senha configurada e, se tudo estiver correto, você terá acesso ao banco!

