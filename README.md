# Prometheus e Grafana para rodar em ambiente Docker Local

## Pré-requisitos mínimos:
- Docker instalado
- Aplicação Spring Boot subindo com as seguintes dependências:
  
        <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-prometheus</artifactId>
            <scope>runtime</scope>
        </dependency>
- A linha abaixo no application.properties da aplicação expondo os endpoints básicos:

    ``management.endpoints.web.exposure.include=health,info,prometheus``

# Criando ambiente local para teste de observability de métricas em 5 passos:


## 1. Configurar Prometheus para ouvir as métricas da sua aplicação no endpoint actuator/prometheus

Caminho do arquivo de configuração do Prometheus:

``cd prometheus-grafana/prometheus-data/prometheus.yml``

Trecho do arquivo que configura o container do Prometheus para ouvir a aplicação:

    scrape_configs:
    - job_name: filmes
      metrics_path: actuator/prometheus
      scrape_interval: 5s
      static_configs:
       - targets: ['host.docker.internal:8085']
Job name deve ser o nome da sua aplicação para ser identificada no Prometheus

O target deve ser "host.docker.internal:" + [porta_da_sua_aplicação]


## 2. Subir os containers:
1º `` cd prometheus-grafana``

2º ``docker-compose up -d``

## 3. Testar acesso na interface do Prometheus:
    http://localhost:9090

## 4. Testar acesso na interface do Grafana:
    http://localhost:3000

### Usuário e senha padrão para login no Grafana
    user: admin

    senha: admin

## 5. Configurar Datasource do Prometheus no Grafana para viabilizar criação Dashboards:
1. Menu lateral > Configuration > Data sources

2. Clicar em "Add data source"

3. Selecionar "Prometheus" como "type"

4. Inserir URL do container do Prometheus ``http://host.docker.internal:9090``

5. Clicar em "Save and Test"





