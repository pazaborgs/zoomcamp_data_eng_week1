# 🚕 Data Engineering Zoomcamp: Semana 1

Este repositório documenta os estudos e a infraestrutura desenvolvida durante a Semana 1 do Data Engineering Zoomcamp, curso oferecido pela comunidade DataTalksClub e liderado por Alexey Grigorev.

O objetivo desta primeira semana foi estabelecer as fundações de infraestrutura e criar um pipeline básico de engenharia de dados. O projeto consistiu em extrair dados de viagens de táxi de Nova York (ny_taxi), processá-los e ingeri-los em um banco de dados PostgreSQL, operando em um ambiente 100% conteinerizado.

    Créditos e Material de Apoio: Os conceitos e arquivos base desta semana foram extraídos do repositório oficial do curso: alexeygrigorev/workshops/dezoomcamp-docker.

## 🛠️ Tecnologias Utilizadas

- Linguagem: Python 3 (gerenciado via uv)
- Processamento de Dados: Pandas
- Banco de Dados: PostgreSQL 18
- Interação com BD: SQLAlchemy & psycopg2-binary
- CLI: Click
- Infraestrutura: Docker & Docker Compose
- Interfaces SQL: pgAdmin (Web GUI) & pgcli (Terminal)

## 🏗️ Arquitetura e O Que Foi Feito

O ciclo de vida do desenvolvimento desta etapa seguiu uma estrutura modular para simular um ambiente real de dados:

1. Desenvolvimento Local: O código foi prototipado em Jupyter Notebooks com ambiente virtual isolado na máquina host via uv, garantindo a validação de tipagem e a visualização do esquema em blocos (chunks).
2. Refatoração (CLI): O pipeline analítico foi modularizado no script pipeline.py. Utilizando a biblioteca Click, as variáveis hardcoded foram removidas, transformando o script em uma ferramenta CLI que recebe parâmetros dinâmicos via terminal (ex: credenciais, datas de ingestão).
3. Containerização: O script Python foi empacotado através de um Dockerfile, travando as versões exatas das dependências através do uv.lock e configurando o ENTRYPOINT para execução automática.
4. Infraestrutura (IaC): A subida do banco de dados e da interface visual foi consolidada e orquestrada via docker-compose.yaml.
5. Redes e Volumes: O projeto configurou Docker Networks para permitir a comunicação interna entre contêineres utilizando DNS interno (ex: pgdatabase), e aplicou Bind Mounts (Docker Volumes) para garantir a persistência de estado do banco de dados e das configurações do pgAdmin localmente na pasta do projeto.

## 📝 Notas de Desenvolvimento

- psycopg2 vs SQLAlchemy: O psycopg2 atua no baixo nível (cabo de rede/driver TCP), enquanto o SQLAlchemy atua como o motor/gerente ORM para tradução eficiente do código Python para o dialeto do Postgres.
- Stateless Containers: Contêineres são efêmeros por natureza. O mapeamento de volumes (diretórios locais refletindo dentro do contêiner) provou-se estritamente necessário nas instâncias do Postgres e pgAdmin para evitar a perda da base de dados e de configurações entre reinicializações do sistema.
