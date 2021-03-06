---
title: 'Tutorial: Treinar um modelo de classificação de imagem com o serviço do Azure Machine Learning'
description: Este tutorial mostra como usar o serviço Azure Machine Learning para treinar um modelo de classificação de imagem com scikit-learn em um Jupyter Notebook Python. Este tutorial é parte de uma série de duas partes.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 8d3dd87adaad168d193b53507dbbb40efab57810
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879478"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning-service"></a>Tutorial nº 1: Treinar um modelo de classificação de imagem com o serviço do Azure Machine Learning

Neste tutorial, você treina um modelo de aprendizado de máquina localmente e em recursos de computador remotos. Você usará o fluxo de trabalho de treinamento e implantação para o serviço de aprendizado de máquina do Azure em um bloco de anotações Jupyter do Python.  Você pode usar o notebook como um modelo para treinar seu próprio modelo de aprendizado de máquina com seus próprios dados. Este tutorial é **parte uma de uma série de tutoriais de duas partes**.  

Este tutorial treina uma regressão logística simples usando o conjunto de dados [MNIST](https://yann.lecun.com/exdb/mnist/) e [scikit-learn](https://scikit-learn.org) com o serviço do Azure Machine Learning.  MNIST é um conjunto de dados popular que consiste em 70.000 imagens em escala de cinza. Cada imagem é um dígito manuscrito de 28x28 pixels, representando um número de 0 a 9. O objetivo é criar um classificador de várias classes para identificar o dígito que uma determinada imagem representa. 

Saiba como:

> [!div class="checklist"]
> * Configurar seu ambiente de desenvolvimento
> * Acesse e examine os dados
> * Treinar uma regressão logística simples localmente usando a popular biblioteca de aprendizado de máquina scikit-learn 
> * Treinar vários modelos em um cluster remoto
> * Revise os resultados do treinamento e registre o melhor modelo

Você aprenderá como selecionar um modelo e implementá-lo em [parte dois deste tutorial](tutorial-deploy-models-with-aml.md) depois. 

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

>[!NOTE]
> O código deste artigo foi testado com a versão 1.0.2 do SDK do Azure Machine Learning

## <a name="get-the-notebook"></a>Obter o bloco de anotações

Para sua conveniência, este tutorial está disponível como um [Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb). Execute o Notebook `tutorials/img-classification-part1-training.ipynb` em Azure Notebooks ou em seu próprio servidor de Jupyter Notebook.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Toda a configuração para o seu trabalho de desenvolvimento pode ser realizada em um bloco de anotações do Python.  A instalação inclui:

* Importação de pacotes do Python
* Conectando-se a um workspace para permitir a comunicação entre seu computador local e recursos remotos
* Criação de um experimento para acompanhar todas as suas execuções
* Criando um destino de computação remota para usar no treinamento

### <a name="import-packages"></a>Importe os pacotes

Importe pacotes Python que você precisa nesta sessão. Além disso, exiba a versão do SDK do Aprendizado de Máquina do Azure.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Conectar-se ao workspace

Crie um objeto de workspace a partir do workspace existente. `Workspace.from_config()` lê o arquivo **config. JSON** e carrega os detalhes em um objeto chamado `ws`.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Criar um experimento

Crie um teste para acompanhar as execuções em seu workspace. Um workspace pode ter vários testes. 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-or-attach-existing-amlcompute"></a>Criar ou anexar AMlCompute existente

A Computação Gerenciada do Azure Machine Learning (AmlCompute) é um serviço gerenciado que permite que os cientistas de dados treinem modelos de aprendizado de máquina em clusters de máquinas virtuais do Azure, incluindo VMs com suporte a GPU.  Neste tutorial, você cria um AmlCompute como seu ambiente de treinamento. Esse código cria clusters de computação, se ainda não existir em seu workspace.

 **A criação do cluster de computação leva aproximadamente 5 minutos.** Se o cluster de computação já estiver no workspace, este código o utiliza e ignora o processo de criação.


```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use the 'status' property    
    print(compute_target.status.serialize())
```

Agora você tem os pacotes necessários e recursos de computação para treinar um modelo na nuvem. 

## <a name="explore-data"></a>Explorar dados

Antes de treinar um modelo, você precisa entender os dados que você está usando para treiná-lo.  Você também precisa copiar os dados na nuvem para que possa ser acessado pelo seu ambiente de treinamento em nuvem.  Nesta seção, você aprenderá como:

* Baixe o conjunto de dados MNIST
* Exibir algumas imagens de exemplo
* Carregar dados para a nuvem

### <a name="download-the-mnist-dataset"></a>Baixe o conjunto de dados MNIST

Baixe o conjunto de dados MNIST e salvar os arquivos em um `data` directory localmente.  Imagens e rótulos para treinamento e teste são baixados.


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Exibir algumas imagens de exemplo

Carregue os arquivos compactados em `numpy` matrizes. Em seguida, use `matplotlib` para plotar 30 imagens aleatórias do conjunto de dados com seus rótulos acima delas. Observe que esta etapa exige uma função `load_data` que está incluída em um arquivo `util.py`. Esse arquivo está incluído na pasta de exemplo. Verifique se ele foi colocado na mesma pasta que este Notebook. A função `load_data` simplesmente analisa os arquivos compactados em matrizes numpy.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Uma amostra aleatória de imagens é exibida:

![amostra aleatória das imagens](./media/tutorial-train-models-with-aml/digits.png)

Agora você tem uma ideia de como essas imagens se parecem e o resultado esperado da previsão.

### <a name="upload-data-to-the-cloud"></a>Carregar dados para a nuvem

Agora, torne os dados acessíveis remotamente enviando-os do computador local para o Azure, para que eles possam ser acessados para treinamento remoto. O armazenamento de dados é uma construção conveniente associada ao seu workspace para você fazer upload / download de dados e interagir com ele a partir de seus destinos de computação remotos. Ele tem suporte da conta de Armazenamento de Blobs do Azure.

Os arquivos MNIST são carregados em um diretório chamado `mnist` na raiz do armazenamento de dados.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Agora você tem tudo de que precisa para começar a treinar um modelo. 

## <a name="train-a-local-model"></a>Treinar um modelo local

Treine um modelo de regressão de logística simples usando scikit-learn localmente.

**O treinamento local pode levar um ou dois minutos**, dependendo da configuração do seu computador.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Em seguida, faça previsões usando o conjunto de testes e calcule a precisão. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

A precisão do modelo local é exibida:

`0.9202`

Com apenas algumas linhas de código, você tem 92% de precisão.

## <a name="train-on-a-remote-cluster"></a>Treinar em um cluster remoto

Agora você pode expandir esse modelo simples construindo um modelo com uma taxa de regularização diferente. Desta vez, você treinará o modelo em um recurso remoto.  

Para esta tarefa, envie o trabalho para o cluster de treinamento remoto que você configurou anteriormente.  Para enviar um trabalho é:
* Criar um diretório
* Criar um script de treinamento
* Criar um objeto avaliador
* Enviar o trabalho 

### <a name="create-a-directory"></a>Criar um diretório

Crie um diretório para entregar o código necessário do seu computador para o recurso remoto.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Criar um script de treinamento

Para enviar o trabalho para o cluster, primeiro crie um script de treinamento. Execute o seguinte código para criar o script de treinamento chamado `train.py` no diretório que você acabou de criar. Esse treinamento adiciona uma taxa de regularização ao algoritmo de treinamento, portanto, produz um modelo ligeiramente diferente da versão local.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Observe como o script obtém dados e salva modelos:

+ O script de treinamento lê um argumento para localizar o diretório que contém os dados.  Quando você envia o trabalho mais tarde, você aponta para o armazenamento de dados para este argumento: `parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')`

+ O script de treinamento salva seu modelo em um diretório chamado outputs. <br/>
`joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`<br/>
Qualquer coisa escrita neste diretório é automaticamente enviada para o seu workspace. Você acessará seu modelo desse diretório posteriormente no tutorial.
O arquivo `utils.py` é referenciado no script de treinamento para carregar o conjunto de dados corretamente.  Copie esse script na pasta de script para que ele possa ser acessado junto com o script de treinamento no recurso remoto.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Criar um estimador

Um objeto estimador é usado para enviar a execução.  Crie seu estimador executando o seguinte código para definir:

* O nome do objeto avaliador, `est`
* O diretório que contém seus scripts. Todos os arquivos neste diretório são carregados nos nós do cluster para execução. 
* O destino de computação.  Nesse caso, você usará o cluster de computação do Azure Machine Learning criado
* O nome do script de treinamento, train.py
* Parâmetros necessários do script de treinamento 
* Pacotes do Python necessários para treinamento

Neste tutorial, esse destino é o AmlCompute. Todos os arquivos na pasta de scripts são carregados em nós de cluster para execução. O data_folder está configurado para usar o armazenamento de dados (`ds.as_mount()`).

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>Enviar o trabalho para o cluster

Execute o experimento, enviando o objeto do avaliador.

```python
run = exp.submit(config=est)
run
```

Como a chamada é assíncrona, ela retorna um estado **Preparando** ou **Em execução** assim que o trabalho é iniciado.

## <a name="monitor-a-remote-run"></a>Monitorar uma execução remota

No total, a primeira execução leva **aproximadamente 10 minutos**. Mas para execuções subseqüentes, desde que as dependências do script não mudem, a mesma imagem é reutilizada e, portanto, o tempo de inicialização do contêiner é muito mais rápido.

Aqui está o que está acontecendo enquanto espera:

- **Criação de imagem**: Uma imagem do Docker é criada correspondendo ao ambiente Python especificado pelo estimador. A imagem é carregada no workspace. A criação e o envio de imagens leva **cerca de 5 minutos**. 

  Este estágio ocorre uma vez para cada ambiente Python, pois o contêiner é armazenado em cache para execuções subsequentes.  Durante a criação da imagem, os logs são transmitidos para o histórico de execução. Você pode monitorar o progresso da criação da imagem usando esses registros.

- **Dimensionamento**: se o cluster remoto requer mais nós para executar a execução que está disponível, mais nós são adicionados automaticamente. O dimensionamento normalmente leva **cerca de 5 minutos.**

- **Executando**: Neste estágio, os scripts e arquivos necessários são enviados para o destino de computação, em seguida, os armazenamentos de dados são montados / copiados e, em seguida, o entry_script é executado. Enquanto o trabalho está em execução, o stdout e o diretório ./logs são transmitidos para o histórico de execução. Você pode monitorar o progresso da corrida usando esses registros.

- **Pós-processamento**: o diretório ./outputs da execução é copiado para o histórico de execução em sua área de trabalho para que você possa acessar esses resultados.


Você pode verificar o andamento de um trabalho em execução de várias maneiras. Este tutorial usa um widget do Jupyter, bem como a `wait_for_completion` método. 

### <a name="jupyter-widget"></a>Widget de Jupyter

Assista ao progresso da corrida com um widget Jupyter.  Como o envio de execução, o widget é assíncrono e fornece atualizações ao vivo a cada 10 a 15 segundos até que o trabalho seja concluído.


```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Aqui está um instantâneo do widget mostrado no final do treinamento:

![widget de bloco de anotações](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Obter resultados de log após a conclusão

O treinamento e monitoramento do modelo acontecem em segundo plano. Aguarde até que o modelo tenha concluído o treinamento antes de executar mais código. Use `wait_for_completion` para mostrar quando o treinamento do modelo estiver concluído. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Exibir resultados de execução

Agora você tem um modelo treinado em um cluster remoto.  Recupere a precisão do modelo:

```python
print(run.get_metrics())
```
A saída mostra que o modelo remoto tem uma precisão ligeiramente superior ao modelo local, devido à adição da taxa de regularização durante o treinamento.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

No próximo tutorial, você explorará esse modelo com mais detalhes.

## <a name="register-model"></a>Registrar modelo

A última etapa do script de treinamento escreveu o arquivo `outputs/sklearn_mnist_model.pkl` em um diretório chamado `outputs` na VM do cluster em que a tarefa é executada. `outputs` é um diretório especial em que todo o conteúdo deste diretório é automaticamente carregado para o seu espaço de trabalho.  Esse conteúdo aparece no registro de execução no experimento em seu workspace. Portanto, o arquivo de modelo agora também está disponível em seu workspace.

Você pode ver os arquivos associados que são executados.

```python
print(run.get_file_names())
```

Registre o modelo no workspace para que você (ou outros colaboradores) possa consultar, examinar e implementar posteriormente esse modelo.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Você também pode excluir apenas o cluster de computação gerenciada do Azure. No entanto, como o dimensionamento automático está ativado e o mínimo de cluster é 0, esse recurso específico não incorrerá em encargos de computação adicional quando não estiver em uso.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial do serviço do Azure Machine Learning, você usou o Python para:

> [!div class="checklist"]
> * Configurar seu ambiente de desenvolvimento
> * Acesse e examine os dados
> * Treinar uma regressão logística simples localmente usando a popular biblioteca de aprendizado de máquina scikit-learn
> * Treinar vários modelos em um cluster remoto
> * Revise os detalhes do treinamento e registre o melhor modelo

Você está pronto para implementar este modelo registrado usando as instruções na próxima parte da série de tutoriais:

> [!div class="nextstepaction"]
> [Tutorial 2 - implantar modelos](tutorial-deploy-models-with-aml.md)
