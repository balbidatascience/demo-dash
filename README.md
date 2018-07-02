# Demo Dash 

Exemplo de aplicação Web para Dashboard de Dados.  

![Figure 1.2](https://github.com/plotly/dash-table-experiments/raw/master/images/DataTable.gif)

# Overview

Dashboards Online são ótimos como soluções de monitoramento e acompanhamento de indicadores. 
Uma excelente opção é o Dash, framework em Python baseado em Flask, Plotyly.js e React.js. 
Este projeto demonstra passo a passo como construir uma aplicação em Dash e hospeda-la em um servidor Linux - EC2 da AWS. 

## Getting Started

Para executar este projeto vamos utilizar python versão 3.6 ou superior. Para verificar sua versão utilize o comando abaixo:

```
python --version 
python 36 --version
```

Caso não possua o python instalado ou não seja a versão correta, [clique aqui](https://wiki.python.org/moin/BeginnersGuide/Download) e siga os passos de instalação de acordo com o seu sistema operacional.

### Prerequisites

Após a instalação do python, execute o comando abaixo para instalar os packages utilizados no projeto:

```
pip install -r requirements.txt
```

### Test

Para testar localmente execute o comando na raiz do projeto:

```
python main.py 127.0.0.1
```


## Deployment

Vamos utilizar a solução da Amazon AWS Elastic Compute Cloud (EC2) para hospedar a aplicação.
Caso não esteja familiarizado com as soluções em nuvem da AWS, verifique [aqui](https://aws.amazon.com/pt/ec2/?sc_channel=PS&sc_campaign=acquisition_BR&sc_publisher=google&sc_medium=english_ec2_b&sc_content=ec2_e&sc_detail=ec2&sc_category=ec2&sc_segment=176355371727&sc_matchtype=e&sc_country=BR&s_kwcid=AL!4422!3!176355371727!e!!g!!ec2&ef_id=WkYTxQAAAK4HvA8g:20180702213413:s) como se registrar na AWS. A Amazon fornece acesso gratuito de alguns dos seus serviços nos primeiros 12 meses. 

Vamos começar disponibilizando o fonte da aplicação em um bucket S3 da AWS para que o servidor EC2 possa sincronizar com este diretório e copiar o fonte. [Clique aqui](https://s3.console.aws.amazon.com/s3) para criar o bucket ou utilize o meu bucket * s3://balbi-demos/demo-dash *. 

1. Após registrar sua conta e disponibilizar o fonte no S3 acesse o console da AWS e clique no serviço EC2 - [clique aqui](https://console.aws.amazon.com/ec2)
Clique em Launch Instance e selecione a Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type

![Figure 1-1](https://s3.amazonaws.com/balbi-demos/aws1.png "Figure 1-1")

2. Selecione a instancia da camada gratuita t2.micro

3. Configure Instance

Para que o servidor tenha acesso ao bucket S3 com o fonte da aplicação vamos criar uma IAM role permitindo acesso da EC2 para o serviço S3 seguindo as etpadas:

3.1. AWS Service

![Figure 1-3](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2017/11/15/1-trust1-693x630.png "Figure 1-3")

3.2. Attach permissions policies 

![Figure 1-4](https://d2wh20haedxe3f.cloudfront.net/sites/default/files/4_7.png "Figure 1-4")

3.3. Review 
Crie um nome para a nova role e clique em "Create role"

Em Advanced Details copie o bootstrap abaixo para executarmos algumas tarefas administrativas e montar o ambiente da aplicação.

```
#!/bin/bash
sudo yum update -y
sudo yum upgrade -y
sudo yum install python36 -y
curl https://bootstrap.pypa.io/get-pip.py | sudo python36
python36 -m venv env
source ./env/bin/activate
python36 -m pip install awscli
aws s3 sync s3://balbi-demos/demo-dash /home/ec2-user/demo-dash
cd demo-dash/
sudo python36 -m pip install -r requirements.txt
```

O script encontra-se também no arquivo bootstrap.txt

4. Step 6 - Configure Security Group 
Crie um Grupo de Segurança com acesso via SSH e HTTP porta 8051 para a aplicação Dash. Como origem disponibilize acesso a internet (0.0.0.0/0 e ::/0) 

5. Review and Launch 

Revise se todos os passos foram seguidos e clique em "Launch".
Um pop-up solicitando que crie uma key pair será aberta e não deixe de salvar a sua para acessarmos depois a maquina e rodar a aplicação.

![Figure 5-1](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/images/vm-new-key-pair.png "Figure 5-1")

6. EC2 iniciada
Clique em "Connect" para acessar as instruções para acessar a maquina via ssh. 

![Figure 6-1](https://www.nginx.com/wp-content/uploads/2017/11/aws-nlb-instance-connect.png "Figure 6-1")

Caso esteja utilizando um computador com sistema operacional Windows utilize o programa [PuTTY](https://www.putty.org/) para realizar esta conexão.

7. Executar a aplicação no servidor EC2
Após acessar o servidor criado via SSH, navegar até o diretório da aplicação criado no bootstrap e execute a aplicação passando como parâmetro o IP do servidor EC2 criado

```
cd demo-dash

```





## References

* [Dash](https://dash.plot.ly/) - Framework de visualização de dados
* [dash-table-experiments](https://github.com/plotly/dash-table-experiments) - Exemplo utilizado de visualização de dados


## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Gabriel Machado** - [Balbi Analytics](http://balbi-site.s3-website-sa-east-1.amazonaws.com/)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Visualização de dados usando soluções open source
* Inspirado para ser um guia simples e profissional de Dashboard

