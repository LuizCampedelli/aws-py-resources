# AWS Resource Scanner em Python

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)	![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

Este script Python coleta informações detalhadas sobre diversos recursos na sua conta AWS, como instâncias EC2, VPCs, Load Balancers, recursos IAM e muito mais. Os dados coletados são exibidos no console e exportados automaticamente para um arquivo Excel (resources.xlsx), com cada tipo de recurso em uma aba separada.

Recursos Coletados
O script atualmente coleta informações para os seguintes serviços e recursos, com paginação para garantir que todos os dados sejam obtidos:

- EC2: Instâncias, VPCs, Transit Gateways
- S3: Buckets
- Lambda: Funções
- RDS: Instâncias de Banco de Dados
- ELB: Load Balancers (Application/Network e Classic)
- Auto Scaling: Grupos de Auto Scaling
- IAM: Usuários, Roles e Políticas (customizadas)
- CloudFormation: Stacks
- DynamoDB: Tabelas
- SNS: Tópicos
- SQS: Filas
- EKS: Clusters
- ECS: Clusters e Serviços
- Funcionalidades Principais
- Coleta Abrangente: Obtém dados detalhados de uma ampla gama de recursos AWS.
- Paginação Automática: Lida com grandes volumes de recursos, garantindo que nenhum dado seja perdido.
- Saída JSON: Exibe um resumo formatado em JSON diretamente no seu terminal para uma visualização rápida.
- Exportação para Excel (resources.xlsx): Gera um relatório organizado em um arquivo Excel, com cada tipo de recurso em uma aba dedicada para fácil análise.

## Pré-requisitos

Para executar este script, você precisará:

- Python 3 (versão 3.6 ou superior é recomendada).
- AWS CLI (para configurar suas credenciais, especialmente se estiver rodando localmente).
- Credenciais AWS configuradas com permissões de leitura (Describe*, List*) para os serviços que o script acessa.

## Como Rodar o Script:

Você tem duas opções principais para executar este script: no AWS CloudShell (altamente recomendado pela sua simplicidade) ou no seu terminal local após configurar o AWS CLI.

## Opção 1: No AWS CloudShell (Recomendado)
- O AWS CloudShell oferece um ambiente pronto para usar, com Python, boto3 e AWS CLI já instalados e configurados com as credenciais da sua sessão no console AWS.

## Acesse o AWS CloudShell:

- Faça login no seu Console AWS.
- No canto superior direito da tela, clique no ícone do CloudShell (parecido com um >_) ou use a barra de pesquisa para encontrar "CloudShell".`
- Clone o repositório git:
```sh
    git clone https://github.com/LuizCampedelli/aws-py-resources.git
```
```sh
    cd aws-py-resources
```
- No terminal do CloudShell, digite o comando abaixo:
```sh
    pip install -r requirements.txt
```
- Em seguida:
```sh
    chmod +x aws_resource_scanner.py
```
```sh
    python3 aws_resource_scanner.py
```
- Após rodar o script, um arquivo será salvo com o nome:
```sh
    resources.xlsx
```

- Baixe o arquivo para poder abrir-lo e verificar os recursos coletados:

```sh
    pwd
```
- Colete o caminho:

```sh
 /home/cloudshelluser/aws-py-resources
```

- Adicione o nome e extensão do arquivo, na aba de download do cloudshell.

## Opção 2: No Seu Terminal Local (usando AWS CLI):

- Para rodar o script no seu computador local, você precisará ter o Python, as bibliotecas necessárias e o AWS CLI instalados e configurados.

### Instale o AWS CLI:
Se ainda não o tiver, siga as instruções oficiais para instalar a versão mais recente do AWS CLI: [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Configure o AWS CLI com suas credenciais:
Este passo é crucial para que o script possa se autenticar na sua conta AWS. Execute aws configure e forneça suas chaves de acesso, chave secreta e a região padrão (ex: sa-east-1).

```sh
    aws configure
    # Para usar perfis nomeados (recomendado para múltiplas contas):
    # aws configure --profile meu-perfil-dev
```
- Clone o repositório
```sh
    git clone https://github.com/LuizCampedelli/aws-py-resources.git
```
```sh
    cd aws-py-resources
```
- No terminal, digite o comando abaixo:
```sh
    pip install -r requirements.txt
```
- Em seguida:
```sh
    chmod +x aws_resource_scanner.py
```
```sh
    python3 aws_resource_scanner.py
```
- Após rodar o script, um arquivo será salvo com o nome:
```sh
    resources.xlsx
```
- Abra o arquivo no seu editor de planilhas preferido.

### Permissões IAM Necessárias (Caso necessário):
- O usuário ou role IAM que executa este script precisa ter permissões de leitura (usualmente operações Describe* e List*) para os serviços AWS que o script tenta inspecionar. Abaixo, um exemplo de política com as permissões mínimas necessárias:

- JSON

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeVpcs",
                "ec2:DescribeTransitGateways",
                "s3:ListAllMyBuckets",
                "lambda:ListFunctions",
                "rds:DescribeDBInstances",
                "elbv2:DescribeLoadBalancers",
                "elb:DescribeLoadBalancers",
                "autoscaling:DescribeAutoScalingGroups",
                "iam:ListUsers",
                "iam:ListRoles",
                "iam:ListPolicies",
                "cloudformation:DescribeStacks",
                "dynamodb:ListTables",
                "dynamodb:DescribeTable",
                "sns:ListTopics",
                "sqs:ListQueues",
                "sqs:GetQueueAttributes",
                "eks:ListClusters",
                "eks:DescribeCluster",
                "ecs:ListClusters",
                "ecs:DescribeClusters",
                "ecs:ListServices",
                "ecs:DescribeServices"
            ],
            "Resource": "*"
        }
    ]
}
```

### Permissões Necessárias Adicionais para AWS Organizations:

- JSON

```sh
    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "organizations:ListRoots",
                "organizations:ListOrganizationalUnitsForParent",
                "organizations:ListAccountsForParent"
                # Adicione estas ao seu JSON de permissões existente
            ],
            "Resource": "*"
        }
    ]
}
```