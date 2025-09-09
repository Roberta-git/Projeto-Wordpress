# Projeto-Wordpress na AWS com Docker

> Na documentação a seguir, será detalhado o passo a passo para a implementação do Wordpress usando Docker em uma instância da AWS, juntamente com os requisitos necessários.


---


### 1. Criação da VPC e configuração de rede

- Foi criado a VPC para a organização dos recursos.
 - Criação de subnets, sendo duas públicas (com Gateway de internet, sendo associadas ao Load Balancer e ao Bastion Host), e duas privadas (para a instância e o EDS).
- Foi configurado o Internet Gateway e anexado à VPC e o NAT Gateway para o aesso das subnets privadas à internet.
- Criação da Tabela de Rotas, com rota padrão (0.0.0.0/0) para as subnets públicas e para as privadas.


 <img src="https://github.com/user-attachments/assets/04f5dd40-9cd7-4fac-84a8-4fc32179129b"  alt="" width="700"/>
</p>


### 2. Grupos de Segurança
- Criação e configuração dos grupos de seguraça específicos:
  - Grupo de Segurança da instância (Ec2): com permissão de entrada nas portas 80 (HTTP) para o Load Balancer, 22 (SSH) para o meu ip, 2024 (NFS) para o grupo de segurança do EFS, e 3306 (MySQL do RDS) para o grupo de segurança do RDS.
  - Grupo de Segurança do RDS: com permissão de entrada na porta 3306 (MySQL) para o grupo de segurança da Ec2.
  - Grupo de Segurança do EFS: com permissão de entrada na porta 2049 (NFS) para o grupo de segurança da EC2.
  - Grupo de Segurança do Load Balancer: com permissão de entrada na porta 80 (HTTP) para qualquer IP.
  - Grupo de Segurança do Bastion Host: com permissão de entrada na porta 22 (SSH) para o meu IP.
 


  <img src="https://github.com/user-attachments/assets/5a56b25a-2e94-4f09-8d8a-366b71a89a9b"  alt="" width="700"/>
</p>




