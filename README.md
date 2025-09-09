# Projeto-Wordpress na AWS com Docker

> Na documentação a seguir, será detalhado o passo a passo para a implementação do Wordpress usando Docker em uma instância da AWS, juntamente com os requisitos necessários.


---


### 1. Criação da VPC e configuração de rede

- Foi criado a VPC para a organização dos recursos.
 - Criação de subnets, sendo duas públicas (com Gateway de internet, sendo associadas ao Load Balancer e ao Bastion Host), e duas privadas (para a instância e o EDS).
- Foi configurado o Internet Gateway e anexado à VPC e o NAT Gateway para o aesso das subnets privadas à internet.
- Criação da Tabela de Rotas, com rota padrão (0.0.0.0/0) para as subnets públicas e para as privadas.


 <img src="https://github.com/user-attachments/assets/903117d2-ce77-44e7-a7cb-d584e6aec882"  alt="" width="700"/>
</p>
