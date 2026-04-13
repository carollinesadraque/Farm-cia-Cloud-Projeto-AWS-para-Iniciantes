# Farm-cia-Cloud-Projeto-AWS-para-Iniciantes
Este projeto foi desenvolvido para iniciantes em AWS e tem como objetivo demonstrar como uma farmácia pode reduzir custos utilizando computação em nuvem.
Objetivos
Reduzir custos com infraestrutura local
Melhorar o controle de estoque
Armazenar dados com segurança e baixo custo
Disponibilizar relatórios simples para tomada de decisão
Arquitetura da Solução
A arquitetura utiliza os seguintes serviços AWS:
Amazon S3 → armazenamento de dados (estoque, relatórios)
AWS Lambda → processamento sem servidor
Amazon DynamoDB → banco de dados NoSQL
Amazon API Gateway → exposição de APIs
AWS CloudWatch → monitoramento
O sistema recebe dados de vendas e estoque via API
Os dados são armazenados no DynamoDB
Relatórios são gerados via Lambda
Arquivos são armazenados no S3
Monitoramento é feito via CloudWatch
Criação da Conta AWS
Criar conta AWS Free Tier
Configurar usuário IAM (evitar usar root)
Armazenamento com S3
Criar bucket S3
Configurar política de acesso
Criar pastas:
/estoque
/relatorios
Banco de Dados (DynamoDB)
Criar tabela:
Nome: estoque-farmacia
Chave primária: id_produto
Inserir dados de teste
Funções Lambda
Criar função Lambda:
Linguagem: Python ou Node.js
Funções:
Registrar venda
Atualizar estoque
Gerar relatório
API Gateway
Criar API REST
Criar endpoints:
POST /venda
GET /estoque
GET /relatorio
Monitoramento
Ativar CloudWatch
Criar logs das funções Lambda
Monitorar erros
Estratégias de Redução de Custos
Uso de Lambda → paga apenas pelo uso
S3 com armazenamento inteligente (Intelligent-Tiering)
DynamoDB sob demanda
Evitar servidores EC2 desnecessários
Resultados Esperados
Redução de custos com servidores físicos
Melhor controle de estoque
Dados acessíveis em tempo real
Escalabilidade automática
Possíveis Melhorias Futuras
Dashboard com Amazon QuickSight
Notificações com SNS
Integração com aplicativo mobile
Machine Learning para previsão de demanda
Aprendizados
Fundamentos de computação em nuvem
Arquitetura serverless
Integração entre serviços AWS
Boas práticas de custo na nuvem
def lambda_handler(event, context):
    
    s3 = boto3.client('s3')

    # Nome do bucket e arquivo
    bucket = 'farmacia-estoque'
    arquivo_entrada = 'entrada/estoque.csv'
    arquivo_saida = 'saida/relatorio.txt'

    # Ler arquivo do S3
    obj = s3.get_object(Bucket=bucket, Key=arquivo_entrada)
    conteudo = obj['Body'].read().decode('utf-8')

    linhas = conteudo.split('\n')

    relatorio = "Produtos com pouco estoque:\n"

    # Ignorar o cabeçalho (primeira linha)
    for linha in linhas[1:]:
        if linha:
            partes = linha.split(',')
            nome = partes[1]
            quantidade = int(partes[2])

            if quantidade < 10:
                relatorio += nome + "\n"

    # Salvar relatório no S3
    s3.put_object(
        Bucket=bucket,
        Key=arquivo_saida,
        Body=relatorio
    )

    return relatorio


    def lambda_handler(event, context):

    # Lista de produtos (estoque da farmácia)
    produtos = [
        {"nome": "Fralda", "quantidade": 8},
        {"nome": "Dipirona", "quantidade": 10},
        {"nome": "Paracetamol", "quantidade": 5},
        {"nome": "Vitamina C", "quantidade": 20}
    ]

    relatorio = "Produtos com pouco estoque:\n"

    # Verifica quais produtos estão com estoque baixo
    for produto in produtos:
        if produto["quantidade"] < 10:
            relatorio += produto["nome"] + "\n"

    return relatorio
