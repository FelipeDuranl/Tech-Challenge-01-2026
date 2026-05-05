# Tech-Challenge-01-2026

# Relatório Técnico

## 1. Estratégias de Pré-processamento

O pré-processamento foi a etapa principal para garantir que o modelo não fosse treinado com dados não normalizado e padronizados, levando a uma possivel falha no diagnostico. As seguintes estratégias foram aplicadas:

### Consolidação e Limpeza de Base de Dados
Foram baixados 6 datasets diferentes do Kaggle. Através de uma análise de similaridade, observamos que alguns eram duplicados. Construimos uma  fontes únicas, resultando em um dataset inicial de 4.310 registros, que após a remoção de duplicatas, foi reduzido para 778 registros únicos.

![Quantidade de Valores Zero por Coluna](image_4X+DDjAHwb/fkhLFTQfSIA==)

![EMBEDDEDIMAGE](placeholder-0)

### Tratamento de Valores Faltantes
Identificamos que colunas como Glucose, BloodPressure, IMC, Insulin e SkinThickness continham valores "0", o que é clinicamente impossível.

**Insulin (48.46% de valores igual a zero)** e **SkinThickness (29.43% de valores igual a zero)** apresentaram as maiores taxas de dados ausentes.

### Imputação por Mediana
Para não perder nosso dataset, substituímos os zeros por NaN e utilizamos o SimpleImputer com a estratégia de mediana, que substitui esses valores de zero para a mediana, ajudando a normalização do nosso dataset, resultando em um melhor treinamento do modelo e por fim seu diagnostico.

![Distribuições após imputação](image_wLm81g1CJSAbum757ua+JQ==)

![EMBEDDEDIMAGE](placeholder-1)

### Controle de Outliers
Analisamos que variáveis como Insulin e SkinThickness tinham valores extremos (máximos de 846 e 110, respectivamente). Aplicamos o **Capping**, limitando os valores para reduzir o ruído, estabilizando o desvio padrão da Insulina de 85.97 para 78.89, trazendo o valores mais para o centro.

![Distribuição e Outliers](image_248P3WL2/0AThJZg/HPA7Q==)

![EMBEDDEDIMAGE](placeholder-2)

| Antes do capping | Depois do capping |
|------------------|------------------|
| ![](image_rYk6DjkfJ3DU4KUoTN2h3w==) | ![](image_w8V93Ymfglv4jpCaD/8WSg==) |

![EMBEDDEDIMAGE](placeholder-3)

![EMBEDDEDIMAGE](placeholder-4)

---

## 2. Modelos Usados e Por Quê

Para este desafio de classificação binária de diabetes, Sim ou Não, os seguintes modelos são propostos para comparação:

**Random Forest:** Foi a escolhida pela sua estabiliade nos testes. Sua capacidade de lidar com dados não-lineares e trabalhar contra o overfitting foi um dos motivos para ser usado. Dado que o dataset possui muitas variáveis que interagem entre si e depende uma da outra, este modelo baseado em árvores tende a performar melhor.

**Regressão Logística:** Utilizada como modelo de baseline. É ideal por ser muito  interpretável, Permite que os médicos entendam a probabilidade por trás de cada diagnóstico.

**KNN (K-Nearest Neighbors):** Útil para identificar padrões de "pacientes similares". Se um novo paciente possui indicadores clínicos próximos a outros pacientes já diagnosticados, o KNN faz a classificação por proximidade.

**SVM (Support Vector Machines):** Utilizado para encontrar a melhor separação possível entre os grupos (diabético vs. saudável). É ideal por ser muito conciso contra erros e eficiente em lidar com exames médicos que não possuem uma divisão clara e direta.

---

## 3. Resultados e Interpretação dos Dados

A análise exploratória mostrou pontos para a interpretação clínica:

### Perfil de Distribuição
Outcome apresenta um desbalanceamento de aproximadamente **1.86:1** ou seja, 65% sem diabetes e 35% com diabetes. Isso indica que o modelo deve ser avaliado com atenção ao **F1-Score**, para evitar falsos negativos e falsos positivos  em pacientes doentes.

![Distribuição da Variável Alvo](image_3tb9Ly7QwH+Cv3baCFw+tg==)

![EMBEDDEDIMAGE](placeholder-5)

### Correlações Clínicas
Vimos que a **Glicose** e o **IMC** possuem distribuições que se deslocam para a direita no grupo positivo. A análise de correlação mostra como variáveis como idade e número de gestações impactam a probabilidade da doença.

![Correlação](image_sbsK+q03XfrT4f5WumpQ6Q==)

![EMBEDDEDIMAGE](placeholder-6)

![Distribuição das Features](image_Vy0Liec8CJSntgb/+TiLFw==)

![EMBEDDEDIMAGE](placeholder-7)

---

### Análise de Importância
Utilizando _Feature Importance_, espera-se que a Glicose seja o preditor mais forte. Através do **SHAP**, poderemos demonstrar ao médico não apenas "se" o paciente tem diabetes, mas "quais" indicadores (ex: IMC alto combinado com Glicose elevada) foram determinantes para aquela decisão da IA.

---

## Conclusão

Concluimos que o modelo Random Forest foi o mais estável dentre todos os modelos testados, por mais que o SVM seja o que obteve melhores métricas de Recall e F1-Score, ele não mostrou resultados estáveis dentre os nossos testes, após executarmos algumas vezes, ele não teve a mesma performace.

Portanto, decidimos seguir com o Random Forest, seus testes se mantiveram estaveis com as métricas importantes para disgnosticos de saúde. É um modelo que trabalha de forma mais acertiva e eficiencite em cima de dados não-lineares, e consegue interelacionar as variaveis. O Random Forest treina subconjuntos diferentes de dados  em cada árvore, e usa apenas um conjunto aleatorio  de variaveis em cada divisão, que posteriormente, para um novo dado, cada árvore da floresta da seu voto, a resposta final é a maioria dos votos (Classificação), ou a media da previsão (Regressão).

O modelo serve como uma **ferramenta de triagem**. Os resultados mostram que, embora a IA possa acelerar a análise, ainda precisamos garantir que o médico tenha a palavra final na validação dos exames laboratoriais.
