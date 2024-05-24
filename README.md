
# Documentação Técnica da Arquitetura AWS

## Diagrama da Arquitetura AWS

![Diagrama da Arquitetura AWS](https://miro.medium.com/v2/resize:fit:586/0*1bsqQcqYTtkeO6cq.png)

**Foto retirada da internet, porém muito similar a nossa topologia (desconsiderando as ferramentas usadas na imagem)**

## Justificativas Técnicas

### VPC e Subnets
- Criamos uma VPC com dois subnets públicos em zonas de disponibilidade diferentes para melhorar a disponibilidade e a redundância dos recursos. Isso garante que, caso uma zona de disponibilidade enfrente problemas, a aplicação continuará operando através da outra zona.

### Internet Gateway e Route Tables
- Um Internet Gateway foi configurado para permitir a comunicação das instâncias EC2 com a internet. As rotas foram associadas para direcionar todo o tráfego para a internet através desse gateway, garantindo que as instâncias possam acessar recursos externos e sejam acessíveis publicamente.

### Auto Scaling e Load Balancer
- Implementamos o Auto Scaling para ajustar automaticamente a quantidade de instâncias EC2 conforme a demanda. Utilizamos um Application Load Balancer (ALB) para distribuir o tráfego de entrada entre as instâncias disponíveis. Esta configuração garante escalabilidade, permitindo que a aplicação responda a variações de carga, e melhora o balanceamento de carga, otimizando a utilização dos recursos.

### Segurança
- Grupos de segurança específicos foram criados para o ALB e para as instâncias EC2, permitindo apenas tráfego HTTP e HTTPS. Essa configuração assegura que somente tráfego autorizado possa acessar os recursos da aplicação, fornecendo uma camada adicional de segurança ao definir regras de entrada e saída específicas.

### DynamoDB
- Optamos por utilizar o DynamoDB como banco de dados, configurado com o modo de pagamento por demanda (PAY_PER_REQUEST). Esta escolha oferece uma solução de banco de dados altamente escalável e gerenciada, com custo ajustado automaticamente de acordo com a utilização, eliminando a necessidade de provisionamento manual de capacidade.

### IAM Roles
- Definimos um perfil de instância (Instance Profile) com permissões para acessar o DynamoDB. Esta configuração garante que as instâncias EC2 possam interagir com o DynamoDB de maneira segura e controlada, seguindo as melhores práticas de segurança e gerenciamento de permissões.

### Escolha de Região
- Selecionamos a região as-east-1 (São Paulo) para hospedar a infraestrutura, levando em consideração a proximidade geográfica dos usuários finais e a disponibilidade de serviços na região. Essa escolha visa reduzir a latência e melhorar o desempenho da aplicação, além de garantir a conformidade com requisitos de residência de dados. Além disso, essa região oferece preços competitivos em comparação com outras regiões.

## Guia Passo a Passo para Execução dos Scripts

**Evitar trocar o nome da stack de "InfraCloud", pois o banco de dados está configurado para que o nome da stack seja o mesmo. Caso queira criar a stack com outro nome, lembrar de alterar também a configuração no banco de dados (app.py)**

1. **Configuração Inicial**:
    - Certifique-se de ter o AWS CLI configurado com as credenciais apropriadas.
    - Instale as ferramentas necessárias: AWS CLI, jq, etc.

2. **Criação da Stack com CloudFormation**:
    ```bash
    aws cloudformation create-stack --InfraCloud MyInfrastructure --template-body file://path/to/infra.yaml
    ```

3. **Atualização da Stack com CloudFormation**:
    ```bash
    aws cloudformation update-stack --InfraCloud MyInfrastructure --template-body file://path/to/infra.yaml
    ```

4. **Deletar a Stack com CloudFormation**:
    ```bash
    aws cloudformation delete-stack --InfraCloud MyInfrastructure
    ```

5. **Validação da Infraestrutura**:
    - Verifique se todos os recursos foram criados corretamente no console da AWS.
    - Teste o acesso ao Load Balancer através do DNS público fornecido. (Aplicação está no DNS público do Load Balancer, com CRUD implementado de simples interface web.)
    - Verifique se as instâncias EC2 estão sendo escaladas corretamente conforme a demanda.

## Relatório Detalhado de Previsão de Custos

### Projeção de custos do projeto

Para estimar os custos relacionados à arquitetura proposta pelo projeto, utilizamos o AWS Cost Calculator. Esta ferramenta permite modelar e comparar os custos de diferentes configurações de serviços AWS, com base em fatores como região, tipo de instância, armazenamento, transferência de dados e outros. Os principais custos da arquitetura do projeto são associados ao DynamoDB e ao Elastic Load Balancer. Abaixo estão os custos estimados para a aplicação proposta:

- **DynamoDB:DynamoDB:** $3,33 por/mês
- **Elastic Load Balancer:** Elastic Load Balancer: $1,69 per/month (1 Application Load Balancer)


### Possíveis Otimizações
- **Reserva de Instâncias EC2**: Considerar a reserva de instâncias para obter descontos significativos em custos de longo prazo.
- **Utilização de Spot Instances**: Para workloads tolerantes a falhas, usar Spot Instances pode reduzir significativamente os custos.
- **Monitoramento e Ajuste de Capacidade**: Monitorar a utilização do DynamoDB e ajustar as capacidades provisionadas para evitar custos desnecessários.

## Link do Repositório com o Código CloudFormation

[Repositório CloudFormation](https://github.com/DouglasCelestino/aplicacaoCloud.git)


### Referências
As principais referências utilizadas foram:

- [AWS](https://aws.amazon.com/pt/)
- [AWS CLI](https://aws.amazon.com/pt/cli/)
- [AWS EC2](https://aws.amazon.com/pt/ec2/)
- [AWS ALB](https://aws.amazon.com/pt/elasticloadbalancing/)
- [AWS IAM](https://aws.amazon.com/pt/iam/)
- [AWS VPC](https://aws.amazon.com/pt/vpc/)