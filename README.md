# azure-ai-predictive-model-example

## Descrição
Demonstração de um modelo preditivo construído com Azure Machine Learning para previsões inteligentes.

## Sobre este projeto
Este repositório contém um exemplo prático de como utilizar o Automated Machine Learning na Azure Machine Learning para treinar, avaliar, implantar e testar um modelo de machine learning. O objetivo é prever o número de aluguéis de bicicletas com base em características sazonais e meteorológicas usando um conjunto de dados históricos.

## Resumo Passo a passo
1. **Crie um workspace da Azure Machine Learning:** Antes de começar, é necessário criar um workspace na Azure. Isso pode ser feito seguindo as instruções [neste link](https://aka.ms/ai900-auto-ml).
2. **Use Automated Machine Learning para treinar um modelo:** Siga as instruções detalhadas [neste link](https://aka.ms/ai900-auto-ml) para configurar e treinar seu modelo utilizando o Automated Machine Learning.
3. **Reveja o melhor modelo:** Após o treinamento, revise os detalhes do melhor modelo treinado e suas métricas de desempenho.
4. **Implante e teste o modelo:** Implante o modelo treinado como um serviço web e teste-o com dados de entrada para verificar sua eficácia.
5. **Limpeza:** Após concluir os testes, exclua o serviço implantado e quaisquer outros recursos utilizados para evitar custos desnecessários.

## Links Importantes
- **Documentação Utilizada:** [Documentação do Automated Machine Learning na Azure Machine Learning](https://aka.ms/ai900-auto-ml)
- **Documentação Geral da Azure AI Services:** [Azure AI Services](https://aka.ms/ai900-azure-ai-services)


# Mão na Massa!


Nesta seção, vamos seguir as instruções fornecidas na documentação para criar, treinar, implantar e testar um modelo de machine learning usando o Automated Machine Learning na Azure Machine Learning.

### 1. Criar um workspace da Azure Machine Learning

Para começar, precisamos provisionar um workspace da Azure Machine Learning em sua assinatura da Azure. Siga os passos abaixo:

1. Faça login no [portal da Azure](https://portal.azure.com) usando suas credenciais da Microsoft.
2. Selecione "+ Criar um recurso" e pesquise por "Machine Learning".
3. Crie um novo recurso da Azure Machine Learning com as seguintes configurações:
   - Assinatura: Sua assinatura da Azure.
   - Grupo de recursos: Crie ou selecione um grupo de recursos.
   - Nome: Insira um nome exclusivo para o seu workspace.
   - Região: Selecione a região geográfica mais próxima.
   - Conta de armazenamento: Anote a conta de armazenamento padrão que será criada para o seu workspace.
   - Key vault: Anote o Key Vault padrão que será criado para o seu workspace.
   - Insights da aplicação: Anote o recurso Insights da aplicação padrão que será criado para o seu workspace.
   - Registro de contêiner: Nenhum (um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner).

4. Selecione "Revisar + criar" e, em seguida, "Criar". Aguarde até que o workspace seja criado (isso pode levar alguns minutos) e, em seguida, vá para o recurso implantado.
5. Selecione "Iniciar estúdio" (ou abra uma nova aba do navegador e acesse https://ml.azure.com) e faça login no Azure Machine Learning studio usando sua conta da Microsoft. Feche quaisquer mensagens que forem exibidas.

### 2. Usar automated machine learning para treinar um modelo

O Automated Machine Learning permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Siga estas etapas para treinar um modelo:

1. No Azure Machine Learning studio, vá para a página Automated ML (em Autoria).
2. Crie um novo trabalho Automated ML com as seguintes configurações, usando "Próximo" conforme necessário para avançar pela interface do usuário:

   **Configurações básicas:**
   - Nome do trabalho: mslearn-bike-automl
   - Novo nome do experimento: mslearn-bike-rental
   - Descrição: Automated machine learning for bike rental prediction
   - Tags: nenhum

   **Tipo de tarefa e dados:**
   - Selecione o tipo de tarefa: Regressão
   - Selecione o conjunto de dados: Crie um novo conjunto de dados com as seguintes configurações:
     - Tipo de dados:
       - Nome: bike-rentals
       - Descrição: Dados históricos de aluguel de bicicletas
       - Tipo: Tabular
     - Fonte de dados:
       - Selecione "De arquivos da web"
       - URL da web: https://aka.ms/bike-rentals
       - Validação de dados: não selecione
     - Configurações:
       - Formato do arquivo: Delimitado
       - Delimitador: Vírgula
       - Codificação: UTF-8
       - Cabeçalhos de coluna: Somente o primeiro arquivo tem cabeçalhos
       - Pular linhas: Nenhum
       - Conjunto de dados contém dados de várias linhas: não selecione
     - Esquema:
       - Inclua todas as colunas exceto Path
       - Reveja os tipos detectados automaticamente

   **Configurações da tarefa:**
   - Tipo de tarefa: Regressão
   - Conjunto de dados: bike-rentals
   - Coluna de destino: Rentals (inteiro)
   - Configurações adicionais:
     - Métrica primária: Erro médio quadrático raiz normalizado
     - Explicar melhor modelo: Não selecionado
     - Usar todos os modelos suportados: Não selecionado. Você restringirá o trabalho a tentar apenas alguns algoritmos específicos.
     - Modelos permitidos: Selecione apenas RandomForest e LightGBM - normalmente você gostaria de tentar o maior número possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.
   - Limites: Expanda esta seção
     - Número máximo de testes: 3
     - Testes concorrentes máximos: 3
     - Máximo de nós: 3
     - Limiar de pontuação métrica: 0.085 (para que, se um modelo alcançar uma pontuação métrica de erro médio quadrático raiz normalizado de 0.085 ou menos, o trabalho seja encerrado.)
     - Tempo limite: 15
     - Tempo limite de iteração: 15
     - Ativar término antecipado: selecionado
   - Validação e teste:
     - Tipo de validação: Divisão de treinamento-validação
     - Porcentagem de dados de validação: 10
     - Conjunto de dados de teste: Nenhum

   **Computação:**
   - Selecione o tipo de computação: Sem servidor
   - Tipo de máquina virtual: CPU
   - Nível da máquina virtual: Dedicado
   - Tamanho da máquina virtual: Standard_DS3_V2*
   - Número de instâncias: 1

   * Se sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

3. Envie o trabalho de treinamento. Ele começa automaticamente.
4. Aguarde o término do trabalho. Isso pode levar um tempo - agora pode ser um bom momento para uma pausa para o café!

## Implantação e Teste do Modelo

Após treinar o modelo usando o Automated Machine Learning, é hora de implantá-lo e testá-lo como um serviço web. Siga as instruções abaixo para implantar e testar o modelo:

### 1. Implantação do Modelo

1. Na página Model do melhor modelo treinado pelo seu trabalho Automated Machine Learning, selecione "Deploy" e use a opção "Web service" para implantar o modelo com as seguintes configurações:
   - Nome: predict-rentals
   - Descrição: Prever aluguéis de bicicletas
   - Tipo de computação: Azure Container Instance
   - Habilitar autenticação: Selecionado

2. Aguarde o início da implantação - isso pode levar alguns segundos. O status da implantação para o ponto de extremidade predict-rentals será indicado na parte principal da página como "Running".

3. Aguarde até que o status de implantação mude para "Succeeded". Isso pode levar de 5 a 10 minutos.

### 2. Teste do Serviço Implantado

Agora você pode testar o serviço implantado para verificar se ele está funcionando corretamente. Siga as instruções abaixo para testar o serviço:

1. No Azure Machine Learning studio, no menu à esquerda, selecione "Pontos de extremidade" e abra o endpoint em tempo real "predict-rentals".

2. Na página do endpoint em tempo real predict-rentals, visualize a guia "Testar".

3. Na seção "Input data to test endpoint", substitua o JSON do modelo pelo seguinte conjunto de dados de entrada:
   ```json
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

4. Clique em "Testar"
5. Revise os resultados do teste, que incluem o número previsto de aluguéis com base nas características de entrada.

### Resultado
Após realizar o teste do serviço implantado, o resultado retornado pelo modelo é representado no formato JSON da seguinte maneira:

```json
{
  "Results": [
    {
      "0": 360.87541184639
    }
  ]
}
```
## Explicando o JSON
Aqui, o JSON fornecido representa um conjunto de dados de entrada que será usado para testar o modelo. Este JSON contém uma estrutura específica, onde os dados de entrada estão aninhados dentro da chave "Inputs". Dentro dessa chave, há uma lista chamada "data", que contém um único objeto representando uma instância de dados. Cada chave neste objeto corresponde a uma característica específica (como dia, mês, ano, etc.), e os valores associados a essas chaves são os valores das características para a instância de dados específica que está sendo testada. Esses valores são usados pelo modelo para fazer uma previsão do número de aluguéis de bicicletas para essa instância específica.


       Inputs: Este objeto contém os dados de entrada para o modelo. No exemplo fornecido, existe um único conjunto de dados representado por uma matriz (data), que contém um objeto com várias características (features) que são usadas como entrada para o modelo.
        data: Uma matriz que contém os dados de entrada para o modelo. Cada objeto dentro desta matriz representa uma instância de dados que será processada pelo modelo. No exemplo fornecido, há apenas uma instância de dados.
            
            day: O dia do mês para o qual você deseja prever o número de aluguéis de bicicletas.
            mnth: O mês para o qual você deseja fazer a previsão.
            year: O ano para o qual você deseja fazer a previsão.
            season: A estação do ano (1 para primavera, 2 para verão, 3 para outono, 4 para inverno).
            holiday: Indica se o dia é feriado (0 para não feriado, 1 para feriado).
            weekday: O dia da semana (0 para domingo, 1 para segunda-feira, ..., 6 para sábado).
            workingday: Indica se o dia é um dia útil (0 para não dia útil, 1 para dia útil).
            weathersit: Condição climática (1 para limpo, 2 para nublado, 3 para chuva leve, 4 para chuva forte).
            temp: A temperatura em graus Celsius.
            atemp: A temperatura aparente em graus Celsius.
            hum: A umidade relativa.
            windspeed: A velocidade do vento.

    GlobalParameters: Este objeto pode ser usado para especificar parâmetros globais que afetam o comportamento do modelo.
