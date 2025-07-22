# Colorização Automática de Imagens com Deep Learning
Este repositório contém o código-fonte do projeto de Processamento de Imagens para a colorização automática de imagens em tons de cinza, utilizando Redes Neurais Convolucionais (CNNs). A abordagem transforma a colorização em um problema de classificação multimodal, permitindo que o modelo produza cores vibrantes e realistas.

O projeto foi desenvolvido por Renan Suana Grothe Garcia e Nathalie Santos Komatsu como parte da disciplina de Processamento de Imagens na Universidade Federal de São Carlos (UFSCar).

## 🔭 Visão Geral
O objetivo central é, a partir do canal de luminância `(L)` de uma imagem no espaço de cor CIE Lab, prever os canais de cromaticidade `('a' e 'b')` correspondentes. O problema, inerentemente mal posto, é tratado como uma tarefa de classificação multimodal.

As principais técnicas empregadas são:

- **Quantização do Espaço de Cor:** O espaço de cor ab é discretizado em 313 classes, transformando o problema de regressão em classificação.
- **Rebalanceamento de Classes:** Uma função de perda personalizada atribui maior peso a cores raras e vibrantes, combatendo a tendência dos modelos de produzir resultados dessaturados.
- **Inferência com Annealed-Mean:** Uma técnica para decodificar a distribuição de probabilidade de saída, ajustando a "temperatura" `(T)` para equilibrar a vivacidade e a coerência espacial das cores.

## 🏗️ Arquitetura do Modelo
Foram desenvolvidas e exploradas duas arquiteturas principais do tipo Encoder-Decoder:

- **ColorNet:** A arquitetura base, que utiliza uma série de blocos convolucionais para extrair características da imagem em diferentes escalas.
- **ColorNetRes:** Uma versão aprimorada que incorpora blocos residuais (Residual Blocks). Essa modificação facilita o fluxo de gradientes durante o treinamento, permitindo a construção de redes mais profundas e melhorando a capacidade de aprendizado de características complexas.

## ⚙️ Pré-requisitos
Para executar este projeto, você precisará ter o Python 3.8 (ou superior) e o gerenciador de pacotes pip instalados.

## 🚀 Instalação
Siga os passos abaixo para configurar o ambiente de desenvolvimento.

1. Clone o repositório:
```
git clone https://github.com/renangrothe/colorize.git
cd colorize
```

2. Crie e ative um ambiente virtual:
É altamente recomendável usar um ambiente virtual para isolar as dependências do projeto.
```
# Criar o ambiente
python -m venv venv

# Ativar no Linux/macOS
source venv/bin/activate

# Ativar no Windows (PowerShell)
.\venv\Scripts\Activate.ps1
```

3. Instale as dependências:
Instale as bibliotecas necessárias manualmente utilizando o pip.
```
pip install torch torchvision numpy scikit-image opencv-python matplotlib
```

## Como Usar
O projeto é dividido em duas etapas principais: treinamento do modelo e inferência (colorização de imagens).

### 1. Treinamento

Para treinar um novo modelo, utilize o script train.py. Você precisará de um dataset de imagens coloridas. O projeto foi treinado originalmente com o ImageWoof, um subconjunto do ImageNet.

**Comando de exemplo:**
```
python train.py --dataset_path /caminho/para/seu/dataset --epochs 100 --batch_size 16 --learning_rate 0.001
```

**Argumentos:**

- `--dataset_path`: Caminho para a pasta raiz do seu dataset de imagens.
- `--epochs`: (Opcional) Número de épocas de treinamento. Padrão: 100.
- `--batch_size`: (Opcional) Tamanho do lote. Padrão: 16.
- `--learning_rate`: (Opcional) Taxa de aprendizado. Padrão: 0.001.

O modelo treinado e os logs serão salvos em um diretório outputs.

### 2. Colorização (Inferência)

Para colorizar uma imagem em tons de cinza, utilize o script colorize.py, fornecendo o caminho para a imagem de entrada e para o modelo pré-treinado (.pth).

**Comando de exemplo:**
```
python colorize.py --input_image ./imagens/minha_foto_pb.jpg --model_path ./outputs/modelo_final.pth --output_image ./imagens/minha_foto_colorida.jpg
```

**Argumentos:**

- `--input_image`: Caminho para a imagem em tons de cinza que você deseja colorir.
- `--model_path`: Caminho para o arquivo do modelo pré-treinado (.pth).
- `--output_image`: (Opcional) Caminho onde a imagem colorida será salva. Se não for fornecido, a imagem será exibida na tela.

## 📊 Resultados

O modelo treinado demonstrou uma forte capacidade de generalização. Mesmo treinado exclusivamente com imagens de cães, foi capaz de colorir de forma convincente outras espécies de animais e objetos, indicando que a rede aprendeu associações robustas entre texturas, formas e cores prototípicas.

A análise de interpretabilidade com mapas de saliência confirmou que o modelo foca em características semanticamente relevantes (como olhos e focinhos) para tomar suas decisões de colorização.
