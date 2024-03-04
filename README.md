# Desafio de projeto 

## Trabalhando com Machine Learning na Prática com Azure ML - Lab1

Projeto desenvolvido no Bootcamp Microsoft Azure AI Fundamentals - oferecido pela **[Digital Innovation One](https://www.dio.me/)**.

Este experimento foi baseado na documentação da [Microsoft](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html). 

## 

# Passos

## Criar um workspace na Azure Machine Learning

1. Criar uma conta na Microsoft e acessar https://portal.azure.com

2. Selecionar a opção **+ Creat a resource**, buscar por *Machine Learning*, localizar **Azure Machine Learning** e clicar em **Criar**.

A configuração deverá ser da seguinte forma:
- Subscription: É a Subscription do Azure;
- Resource group: Dar um nome para o resource group;
- Name: Dar um nome para a workspace;
- Region: Selecionar a região mais próxima a você;
- Storage account: Será gerado um padrão;
- Key vault: Será gerado um padrão;
- Application insights:  Será gerado um padrão;
- Container registry: Nenhum.

3. Selecionar **Review + Create** e selecionar **Create**. Aguardar o workspace ser criado, selecionar este workspace e depois clicar em **Launch Studio**.

4. Dentro de **Azure Machine Learning Studio** o novo workspace já estará disponível.

##

## Utilizar o automated machine learning para treinar um modelo de aluguel de bicicletas

1. Dentro de **Azure Machine Learning Studio** clicar em **Automated ML** page.

2. Clicar em **New automated ML job** e configurar da seguinte forma:

Básico:

- Nome do trabalho: mslearn-bike-automl
- Nome do novo experimento: mslearn-bike-rental
- Descrição: Automated machine learning for bike rental prediction
- Tags: nenhuma

Tipo de tarefa & data:

- Selecionar: Regressão
- Selecionar dataset: Criar o novo dataset da seguinte forma:

*Data type:*
- Nome: bike-rentals
- Descrição: Historic bike rental data
- Tipo: Tabular
*Data source:*
- Selecionar Arquivos da web
*Web URL:*
- Web URL: https://aka.ms/bike-rentals
- Skip data validation: não selecionar
*Settings:*
- Formato do arquivo: Delimited
- Delimitador: Comma
- Encoding: UTF-8
- Column headers: Somente o primeiro arquivo com cabeçalho
- Pular linha: Nenhum
- Dataset contains multi-line data: não selecionar
*Schema:*
- Selecionar todas as colunas exceto Path
- Selecionar Create. Depois de criado, selecionar o bike-rentals dataset para continuar a criação do Automated ML job.

##

Configurações da tarefa:

- Tipo da tarefa: Regressão
- Dataset: bike-rentals 
- Coluna alvo: rentals (integer)

Configurações adicionais:

- Métrica primária: Normalized root mean squared error
- Explain best model: Não selecionar
- Usar todos os modelos suportados: Não selecionar
- Modelos permitidos: Selecionar somente RandomForest e LightGBM 

Limites: Expandir essa seção

- Max tentativas: 3
- Max de tentativas seguidas: 3
- Max de nós: 3
- Métrica de pontuação: 0.085 
- Timeout: 15 (minutos)
- Timeout da iteração: 15
- Habilitar encerramento: Selecionar

Validação e teste:

- Tipo de validação: Validação de teste
- Porcentagem de validação de dados: 10
- Dataset teste: Nenhum

Computação:

- Selecionar tipo de computação: Sem servidor
- Tipo de máquina virtual: CPU
- Tier da máquina virtual: Dedicado
- Tamanho da máquina virtual: Standard_DS3_V2
- Número de instância: 1

Submeter o trabalho de treinamento que começará automaticamente.

Aguardar a finalização do trabalho que pode demorar uns minutos.

##

## Revisar o melhor modelo

Assim que o trabalho estiver Completed já podemos revisar o melhor modelo de treinamento.

1. Na aba **Overview** selecionar abaixo de **Algorithm name** o nome do melhor modelo.

2. Verificar em **Metrics** se estão selecionados **residuals** e **predicted_true**

##

## Colocar em produção e testar 

Dentro do melhor modelo, selecionar **Deploy** --> **Web Service**:

Configurar da seguinte forma:

- Nome: predict-rentals2
- Descrição: Predict cycle rentals
- Tipo de computação: Azure Container Instance
- Habilitar autenticação: Selecionar

Aguardar entrar em produção que ficará com status Running.

Quando o status mudar para Succeeded, está concluído.

##

## Testar o serviço em produção

1. Dentro do **Azure Machine Learning studio** selecionar **Endpoints** e abrir o **predict-rentals2** 

2. Clicar em *Test* 

3. Inserir o seguinte JSON:

```
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }

 ```

 4. Clicar no botão *Test* e o resultado deverá ser algo parecido com:

 ```
 {
  "Results": [
    338.9341164334042
  ]
}
```

Este experimento é baseado em dados históricos de aluguel de bicicletas para treinar um modelo. Este modelo prevê a quantidade de bicicletas alugadas conforme os dias baseado em estações e meteorologia.


##

## Limpar o experimento

Para evitar custas desnecessárias, o ideal é deletar o experimento que pode ser feito da seguinte forma:

1. Na aba **Endpoints** selecionar **predict-rentals2** e selecionar *Deletar*

2. Na aba **Resource Groups** clicar no workspace criado e em *Deletar resource group*

##
