# Demo Dash 

Exemplo de aplicação Web para Dashboard de Dados.  
> Colar gif ou imagem com funcionamento de um painel pronto

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

1. Após registrar sua conta acesse o console da AWS e clique no serviço EC2 - [clique aqui](https://console.aws.amazon.com/ec2)
Clique em Launch Instance e selecione a Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type

![Figure 1-1](https://s3.amazonaws.com/balbi-demos/aws1.png "Figure 1-1")

2. Selecione a instancia da camada gratuita t2.micro

3. Em Advanced Details copie o bootstrap abaixo para executarmos algumas tarefas administrativas e montar o ambiente da aplicação.

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
python36 -m pip install -r requirements.txt
```

O script encontra-se também no arquivo bootstrap.txt

## Built With

* [Dash](https://dash.plot.ly/) - O framework de visualização de dados.
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds


## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Gabriel Machado** - [Balbi Analytics](http://balbi-site.s3-website-sa-east-1.amazonaws.com/)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Visualização de dados usando soluções open source
* Inspirado para ser um guia simples e profissional de Dashboard

