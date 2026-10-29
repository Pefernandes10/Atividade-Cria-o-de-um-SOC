# Análise de incidentes em um SOC

Projeto de Ciência de Dados aplicada à Cibersegurança, desenvolvido para os Checkpoints 2 e 3 da faculdade. A proposta é analisar eventos de segurança e avaliar uma Regressão Logística como classificador inicial.

O trabalho foi feito em **ambiente de testes**, com um CSV de 50 registros. Não foi validado para uso em produção ou para substituir a análise de um SOC.

## Cuidados com os dados

**Não utilize informações confidenciais neste projeto.** Não publique nem envie ao Colab credenciais, dados pessoais, logs internos ou registros de clientes. Use apenas dados autorizados para compartilhamento. O CSV incluído é o material fornecido para esta atividade; sua origem não permite afirmar que se trata de dados reais ou sintéticos.

## O que foi analisado

- Proporção de eventos benignos e incidentes.
- Média e mediana das falhas de login em cada classe.
- Relação entre tráfego de saída e incidentes.
- Classificação com Regressão Logística e avaliação dos erros.

O modelo recebe `falhas_login`, `ips_origem`, `trafego_saida_mb` e `hora_evento` e prevê `incidente` (0: benigno; 1: incidente). A separação usa 80% dos eventos para treinamento e 20% para teste, com estratificação e `random_state=42`. A padronização é ajustada somente no treinamento, dentro de um pipeline.

## Como executar

[Abra o notebook no Google Colab](https://colab.research.google.com/github/Pefernandes10/Atividade-Cria-o-de-um-SOC/blob/main/CP2_CP3_Analise_Incidentes_SOC.ipynb), escolha **Ambiente de execução → Executar tudo** e envie o arquivo `dataset_defesa_cibernetica_50.csv` quando solicitado. O CSV está disponível neste repositório.

Para executar localmente, use Python 3.12 e um editor compatível com notebooks, como VS Code com a extensão Jupyter. Instale as dependências com:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt ipykernel
```

No Windows, ative o ambiente com `.venv\Scripts\activate`. Abra o notebook, selecione o ambiente criado como kernel e execute as células na ordem. Mantenha o CSV na mesma pasta do notebook.

## Resultados desta execução

O conjunto contém 34 eventos benignos e 16 incidentes. No teste com 10 eventos:

| Métrica | Resultado |
| --- | ---: |
| Acurácia | 90% |
| Precisão | 100% |
| Recall | 66,67% |
| F1-score | 80% |

A matriz de confusão registrou 7 eventos benignos e 2 incidentes classificados corretamente, nenhum falso positivo e 1 falso negativo. Esse incidente não identificado mostra por que a acurácia, sozinha, não basta para avaliar o modelo.

A amostra é pequena e o resultado corresponde a uma única divisão de treino e teste. Também há sobreposição no tráfego das duas classes: um evento benigno chega a 566 MB, enquanto há incidente com 113,8 MB. O volume de tráfego isolado não determina a classificação.

## Arquivos

- `CP2_CP3_Analise_Incidentes_SOC.ipynb`: análise, gráficos, modelo e respostas da atividade.
- `dataset_defesa_cibernetica_50.csv`: dados utilizados, sem alteração dos valores.
- `requirements.txt`: versões das bibliotecas usadas na execução validada.
