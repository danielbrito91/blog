---
title: "O Que Aprendi Com Minha Primeira Competição No Kaggle"
date: 2021-10-26T18:47:55-03:00
cover:
        image: "https://i.ytimg.com/vi/CHUl7pj5S_8/hqdefault.jpg"
        alt: ""
        caption: "Beth Carvalho - [YouTube](https://www.youtube.com/watch?v=CHUl7pj5S_8)"
draft: false
categories:
  - python
tags:
  - scikit-learn
  - classification
  - lightGBM
  - cardinality
  - encoding
  - kaggle
---

Neste mês de outubro fiz minha primeira participação em uma competição oficial do Kaggle. Até então, eu só havia utilizado alguns toy datasets da plataforma (ao exemplo do Titanic), sem ter ainda colocado a mão na massa em uma competição real. Resolvi arriscar no Porto Seguro Data Challenge e, para minha inicial decepção, meu resultado foi bem aquém do que eu gostaria. Após o desânimo inicial, inspirado pelo depoimento do saudoso [Mário Filho](https://www.youtube.com/watch?v=EqHyE9MYcmw&t=364s), resolvi [levantar, sacodir a poeira e, aos poucos, tentar dar a volta por cima](https://www.youtube.com/watch?v=CHUl7pj5S_8).


O dataset da competição apresentava vários desafios, alguns mais difíceis  do que outros: dados anonimizados, dados categóricos com um altíssimo número de valores distintos (particularmente meu maior inimigo dessas últimas semanas), target desbalanceado e dados faltantes. Resolvi escrever esse post para documentar meu processo de aprendizado, tentando levar para outras competições e projetos o que aprendi por aqui.

# A importância de um bom Baseline

Que um modelo inicial é importante não é novidade para quem se aventura na área de machine learning. Como toda atividade com tempo finito de execução, é importante medir o quanto nossos esforços estão de fato surtindo algum efeito na melhoria da nossa solução. Ainda assim, é tentador começar a se aprofundar na análise antes mesmos de se estabelecer esse primeiro marco. 

Nesse sentido, me chamou muito a atenção o notebook do [Felipe Fiorini](https://www.kaggle.com/felipefiorini/lgbm-baseline/notebook), em que o autor utiliza um classificador [LightGBM](https://lightgbm.readthedocs.io/en/latest/index.html) para estabelecer o baseline. Além de ser um framework que costuma trazer uma boa acurácia e um tempo de treino menor, ele lida diretamente com features categóricas (sendo necessário informar quais são essas variáveis junto ao parâmetro `categorical_feature`) e com dados faltantes.

# Além do OneHotEncoder e do OrdinalEncoder

O encoding de dados categóricos é uma etapa usual de preparação dos dados para treino. O scikit-learn oferece o [LabelEnconder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder), [OrdinalEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html#sklearn.preprocessing.OrdinalEncoder) e [OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder), sendo alternativas adotadas recorrentemente. Quando nos deparamos com dados que possuem uma alta cardinalidade, essas opções podem não ser suficientes. Como diz a própria documentação do LigthGBM, a adoção do one-hot eocoding pode gerar alguns problemas em algorítmos derivados de árvores de decisão:

> It is common to represent categorical features with one-hot encoding, but this approach is suboptimal for tree learners. Particularly for high-cardinality categorical features, a tree built on one-hot features tends to be unbalanced and needs to grow very deep to achieve good accuracy.
> 

O notebook do [Heitor Rapela Medeiros](https://www.kaggle.com/rapela/porto-seguro-data-challenge-tabnet), por exemplo, utilizou o `RareLabelEncoder` da biblioteca [Feature-engine](https://feature-engine.readthedocs.io/en/1.1.x/). O método pode ser mais adequado para esse tipo de problema.

# Tunar o threshold

A competição usou F1 como métrica, que consiste na média harmônica de precision e recall. É uma medida usual para problemas com targets desbalanceados. Ao invés de simplesmente adotar a classificação prevista pelo modelo, alguns competidores (ao exemplo da solução vencedora do time [Artificial Pscho Killer](https://www.kaggle.com/joaopmpeinado/1st-place-lightgbm-0-7007-private)) tunaram o threshold para encontrar o melhor cutoff.

# Não deixe para a última hora

Essa lição parece conselho de mãe, mas também se aplica às competições do Kaggle. Senti que, no desespero de última hora, tentei soluções mais apressadas e alguns "tiros no escuro" que acabaram sendo inócuos. Conseguir se dedicar tempo suficiente à competição avaliando bem o modelo antes de o submeter é importante para uma boa performance.
