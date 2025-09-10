# Projeto-Wordpress na AWS com Docker

> Na documentação a seguir, será detalhado o passo a passo para a implementação do Wordpress usando Docker em uma instância da AWS, juntamente com os requisitos necessários.


---


### 1. Configuração do Docker e Docker Compose

- Primeiramente, foi instalado e feito a configuração do ambiente Docker para rodar o Wordpress, com o arquivo `docker-compose.yml` que controla o containr do Wordpress com as variáveis de ambiente de conexão ao RDS e usa o EFS para o armazenameto dos dados.
  
colocar arquivo de script do docker compose 

### 2. Criação da VPC e configuração de rede

- Foi criado a VPC para a organização dos recursos.
 - Criação de subnets, sendo duas públicas (com Gateway de internet, sendo associadas ao Load Balancer e ao Bastion Host), e duas privadas (para a instância e o EDS).
- Foi configurado o Internet Gateway e anexado à VPC e o NAT Gateway para o aesso das subnets privadas à internet.
- Criação da Tabela de Rotas, com rota padrão (0.0.0.0/0) para as subnets públicas e para as privadas.


 <img src="https://github.com/user-attachments/assets/04f5dd40-9cd7-4fac-84a8-4fc32179129b"  alt="" width="700"/>
</p>


### 3. Grupos de Segurança

- Criação e configuração dos grupos de seguraça específicos para o controle do tráfego da rede:
  - Grupo de Segurança da instância (Ec2): com permissão de entrada nas portas 80 (HTTP) para o Load Balancer, 22 (SSH) para o meu ip, 2024 (NFS) para o grupo de segurança do EFS, e 3306 (MySQL do RDS) para o grupo de segurança do RDS.
  - Grupo de Segurança do RDS: com permissão de entrada na porta 3306 (MySQL) para o grupo de segurança da Ec2.
  - Grupo de Segurança do EFS: com permissão de entrada na porta 2049 (NFS) para o grupo de segurança da EC2.
  - Grupo de Segurança do Load Balancer: com permissão de entrada na porta 80 (HTTP) para qualquer IP.
  - Grupo de Segurança do Bastion Host: com permissão de entrada na porta 22 (SSH) para o meu IP.
 


  <img src="https://github.com/user-attachments/assets/5a56b25a-2e94-4f09-8d8a-366b71a89a9b"  alt="" width="700"/>
</p>



### 4. Banco de Dados (RDS)

- Criação do banco de dados MySQL para o armazenamento de dados do Wordpress, nomeado de `database-1` com o usuário `iasmyn` e utilizando uma instância de tamanho t3.micro.


<img src="https://github.com/user-attachments/assets/10c85030-3992-4a3d-b932-d4331a698cbc"  alt="" width="700"/>
</p>


### 5. EFS 

- Criação do EFS montado nas instâncias, para armazenar os arquivos de forma persistente nas instâncias.


<img src="https://github.com/user-attachments/assets/6e592e6f-cf5d-49a8-bbad-5b8ec1b7bc0d"  alt="" width="700"/>
</p>


### 6. Lauch Template e User Data

- Foi usado o Launch Template para a criação automática de instâncias e o script de UserData para instalação do Docker, Docker compode e deploy do Wordpress, com o objetivo de automatizar toda a configuração.


<img src="https://github.com/user-attachments/assets/75d871c0-b0eb-4736-83c7-8ca85ea4599f"  alt="" width="700"/>
</p>

- colocar arquivo de script do User data


### 7. Auto Scaling Group e Load Balancer

- Foi criado o ASG para criar e remover instâncias conforme o tráfego, associado às subnets privadas, com capacidade desejada de duas instâncias.



<img src="https://github.com/user-attachments/assets/71ab4ea6-1f16-45db-bd6e-360831cae9a9"  alt="" width="700"/>
</p>


- Além disso foi configuraedo o Application Load Balancer e associado ao Target Group, tendo o exame de saúde com o caminho`/` e códigos de sucesso `200,302`.
    - Um fator importante é que a instância seja apresentada como saudável.



<img src="https://github.com/user-attachments/assets/bafbe602-c526-4de8-8ef1-89f74e508c8a"  alt="" width="700"/>
</p>




### 8. Testes e verificação das configurações

- Foi realiazado o teste de acesso via `curl -I http://localhost:80/` no ssh da instância, retornando o status code de 
`HTTP/1.1 200 OK`.


<img src="https://github.com/user-attachments/assets/3beddfd5-c690-4791-9945-99c4b8248b2d"  alt="" width="700"/>

</p>


-Verificado o status de "healthy" no Target Group.


<img src="https://github.com/user-attachments/assets/0a6d25bd-8e69-4ec4-bfc8-12988df37393"  alt="" width="700"/>
</p>


- Acesso ao Wordpress através do DNS público do Load Balancer.


<img src=""  alt="" width="700"/>
</p>

