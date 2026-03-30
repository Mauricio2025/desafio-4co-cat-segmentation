# 🐈 Segmentação de Instância (YOLOv11) | Desafio Técnico 4co

![YOLOv11](https://img.shields.io/badge/YOLOv11-Ultralytics-blue)
![Python](https://img.shields.io/badge/Python-3.12-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep_Learning-EE4C2C)
![Roboflow](https://img.shields.io/badge/Roboflow-Data_Centric-purple)

Este repositório contém o pipeline completo de **MLOps** desenvolvido para o desafio técnico da **4co**, com foco em **Segmentação de Instância** (Instance Segmentation) de felinos.

## 🎯 Objetivo do Projeto
Construir um modelo leve, rápido e de altíssima precisão capaz de identificar gatos em imagens inéditas e gerar máscaras poligonais exatas ao redor de suas silhuetas. 

A arquitetura escolhida foi a **YOLOv11 Nano (`yolo11n-seg`)** por sua excelente relação entre custo computacional e performance (ideal para edge devices).

## 🧠 Metodologia: A Abordagem Data-Centric
Em projetos de Visão Computacional, a qualidade do dado supera o volume bruto. A estratégia adotada baseou-se inteiramente no conceito de **Data-Centric AI**:

1. **Curadoria e Aumento do Dataset:** O treinamento inicial com 50 imagens resultou em *underfitting* de contexto (o modelo acertava, mas com baixa confiança matemática). A solução técnica imediata foi dobrar o dataset para **100 imagens**, aumentando exponencialmente a variância de cenários, iluminação, ângulos e oclusões.
2. **Anotação Cirúrgica (SAM):** A rotulagem foi realizada na plataforma Roboflow. Para garantir que as máscaras não incluíssem ruídos do *background* (Lixo entra, Lixo sai), utilizou-se a assistência do modelo SAM (*Segment Anything Model*) para contornos perfeitamente rentes aos pixels dos objetos.
3. **Data Split Seguro:** O conjunto foi dividido matematicamente em `70% Treino`, `20% Validação` e `10% Teste` para evitar *overfitting* de validação cruzada.

## ⚙️ Stack Tecnológico Integrado
* **Roboflow:** Hospedagem, anotação poligonal assistida por IA e versionamento do dataset.
* **Google Colab (Tesla T4 GPU):** Ambiente de treinamento em nuvem acelerado por hardware.
* **Ultralytics:** Framework base para o treinamento e inferência.

## 🚀 Métricas e Resultados (100 Épocas)
O modelo foi treinado por 100 épocas (aprox. 3 minutos na T4 GPU), atingindo métricas de nível de produção:

* **mAP50:** `0.964` (Altíssima precisão na localização e identificação da bounding box).
* **mAP50-95:** `0.736` (Qualidade cirúrgica do contorno da máscara isolando o background).

**Inferência Cega:** Ao ser testado contra as imagens inéditas do conjunto de validação com o limiar de confiança padrão para produção (`conf=0.25`), o modelo detectou e mascarou com sucesso **100% das instâncias**.

---

## 💻 Como Reproduzir e Executar o Pipeline

Todo o processo de ETL de dados, treinamento e inferência está contido em um único arquivo Jupyter Notebook, desenhado para ser executado de forma fluida.

### Requisitos:
* Conta Google (para uso do Google Colab).
* Nenhuma configuração local complexa é necessária.

### Passos de Execução:
1. Faça o download do arquivo `.ipynb` principal deste repositório.
2. Acesse o [Google Colab](https://colab.research.google.com/) e faça o upload do arquivo.
3. No menu superior, vá em **Ambiente de Execução** > **Alterar o tipo de ambiente de execução** e certifique-se de que o Acelerador de Hardware está configurado para **T4 GPU**.
4. O notebook está dividido em blocos sequenciais:
   * **Bloco 1:** Faz o download e instalação das dependências (Ultralytics, Roboflow).
   * **Bloco 2:** Importa o dataset versionado diretamente via API.
   * **Bloco 3:** Inicia o treinamento (100 épocas).
   * **Bloco 4:** Executa a inferência em loop contra o diretório de validação e exibe os resultados na tela.
5. Basta executar os blocos sequencialmente (`Shift + Enter`).

---
*Desenvolvido por Mauricio Souza para a 4co.*
