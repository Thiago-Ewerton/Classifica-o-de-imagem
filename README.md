# Classificação de Imagem de Estradas

Um projeto de **classificação de imagens** utilizando **Transfer Learning** com a arquitetura **ResNet18** para identificar e categorizar diferentes tipos de superfícies de estrada.

## 📋 Visão Geral

Este projeto aplica técnicas modernas de aprendizado profundo para classificar imagens de estradas em **3 classes**:
- **Asphalt** (Asfalto)
- **Belgian Blocks** (Paralelepípedos)
- **Offroad** (Fora da estrada)

## 🎯 Objetivo

Desenvolver um modelo robusto capaz de identificar tipos de superfícies de estrada com alta precisão, especialmente em cenários com **desbalanceamento significativo entre as classes**.

## 🏗️ Arquitetura e Metodologia

### Abordagem Utilizada: Transfer Learning

A escolha por **Transfer Learning** com **ResNet18** é justificada por:

1. **Dataset pequeno**: Treinar uma rede do zero exigiria muito mais dados e tempo computacional
2. **Pré-treinamento em ImageNet**: A rede já possui filtros treinados para detectar características visuais genéricas
3. **Eficiência**: Rápida convergência e bom desempenho com poucos recursos

### Tecnologias

- **PyTorch**: Framework de deep learning
- **Torchvision**: Utilitários para visão computacional
- **scikit-learn**: Métricas e processamento de dados
- **Matplotlib & Seaborn**: Visualização de resultados
- **Google Colab**: Ambiente de treinamento com GPU

## 🔍 Desafios Encontrados

### Desbalanceamento de Classes

O principal desafio foi o **forte desbalanceamento** entre as classes:
- **Asphalt**: Classe majoritária (~600 amostras)
- **Belgian Blocks**: Classe minoritária (~100 amostras) ⚠️
- **Offroad**: Classe intermediária (~200 amostras)

**Impacto**: O modelo tendia a enviesar para a classe majoritária, resultando em baixo recall para Belgian Blocks.

## 🧪 Experimentos Realizados

### **Baseline**
- Acurácia geral: **90%**
- ❌ F1-Score Belgian Blocks: **0.44** (Problemas graves)
- Recall Belgian Blocks: **0.28** (Muito baixo)

**Conclusão**: Modelo apresentava viés pela classe majoritária.

---

### **Experimento 1: Ajuste de Hiperparâmetros**

**Alterações**:
- Taxa de aprendizado reduzida: 0.001 → **0.0001** (10x menor)
- Pesos de classes suavizados: Utilizou `balanced` do scikit-learn
- Épocas aumentadas: 5 → **10**

**Resultados**:
- Acurácia: 88%
- ❌ F1-Score Belgian Blocks: **0.40** (Piorou!)
- Recall Belgian Blocks: **0.25** (Continuou baixo)

**Conclusão**: Suavizar os pesos sem enriquecer os dados não é suficiente quando a classe minoritária tem poucas amostras.

---

### **Experimento 2: Data Augmentation** ✅ **MELHOR RESULTADO**

**Alterações**:
- **Augmentação de dados no treinamento**:
  - Flip horizontal aleatório
  - Rotação (±15°)
  - Ajuste de brilho, contraste, saturação e tonalidade
- Mantidos os hiperparâmetros do Experimento 1

**Resultados**:
- Acurácia: **90%** (mantida)
- ✅ F1-Score Belgian Blocks: **0.79** (Melhoria de 98%!)
- ✅ Recall Belgian Blocks: **0.66** (Melhoria de 164%!)
- Macro Average: **0.84** (Muito mais equilibrado)
- Weighted Average: **0.90**

**Conclusão**: Data augmentation forçou o modelo a aprender características estruturais em vez de memorizar, resulando em melhor generalização.

## 📊 Resultados Finais

### Matriz de Confusão (Experimento 2)

```
                precision    recall  f1-score   support
       asphalt       0.97      0.91      0.94       218
belgian_blocks       1.00      0.66      0.79        32
       offroad       0.67      0.98      0.80        50

      accuracy                           0.90       300
     macro avg       0.88      0.85      0.84       300
  weighted avg       0.92      0.90      0.90       300
```

## 🚀 Como Usar

### Requisitos

```bash
pip install torch torchvision scikit-learn matplotlib seaborn numpy
```

### Estrutura do Dataset

```
dataset_processed/
├── train/
│   ├── asphalt/
│   ├── belgian_blocks/
│   └── offroad/
└── test/
    ├── asphalt/
    ├── belgian_blocks/
    └── offroad/
```

### Executar o Notebook

1. Abra `classificacaoDeEstradas.ipynb` no Google Colab
2. Configure o acesso ao Google Drive para carregar o dataset
3. Execute as células sequencialmente

## 💡 Principais Aprendizados

1. **Transfer Learning é poderoso**: Mesmo com pré-treinamento fixo, o modelo alcançou 90% de acurácia
2. **Data Augmentation é essencial**: Quando há desbalanceamento, aumentar artificialmente as amostras minoritárias é mais efetivo que ajustar apenas os pesos
3. **Métricas além da acurácia importam**: A acurácia geral esconde problemas graves em classes minoritárias

## 🔮 Próximos Passos

Para melhorias futuras:

1. **Coleta de mais dados** para a classe Belgian Blocks
2. **Implementação de focal loss** para penalizar mais os erros em amostras difíceis
3. **Fine-tuning progressivo** de mais camadas da ResNet
4. **Testagem com outras arquiteturas** (EfficientNet, Vision Transformer)
5. **Validação cruzada** para melhor avaliação de generalização
6. **SMOTE ou oversampling sintético** para gerar amostras balanceadas

## 📝 Conclusão

Este projeto demonstra que em problemas de **classificação com desbalanceamento de dados**, a combinação de:
- ✅ Transfer Learning robusto
- ✅ Data Augmentation estratégica
- ✅ Ajuste fino de hiperparâmetros

...resulta em modelos práticos e confiáveis, mesmo com datasets pequenos e desbalanceados.

## 📧 Autor

Thiago-Ewerton

