# Análise e previsão da produção mensal de cerveja

Projeto de análise de séries temporais desenvolvido em Python para estudar a produção mensal de cerveja na Austrália. O trabalho explora tendência, sazonalidade e comportamento de longo prazo, compara métodos de previsão e estima os valores dos próximos 12 meses.

## Arquivos

- `Trabalho02.ipynb`: notebook com as análises e gráficos dos sete itens do projeto.
- `datasets_56102_107707_monthly-beer-production-in-austr.csv`: dados mensais usados pelo notebook. Ele espera as colunas `Month` e `Monthly beer production`.

Mantenha o notebook e o CSV na mesma pasta para que a leitura dos dados funcione.

## Requisitos

- Python 3.8 ou superior
- Jupyter Notebook ou a extensão Jupyter do VS Code
- Bibliotecas: `pandas`, `numpy`, `matplotlib`, `statsmodels` e `pmdarima`

## Preparar o ambiente no Windows (PowerShell)

Na pasta do projeto, crie e ative um ambiente virtual:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Instale as dependências no ambiente virtual:

```powershell
python -m pip install --upgrade pip
python -m pip install pandas numpy matplotlib statsmodels pmdarima jupyter ipykernel
```

Se o PowerShell bloquear a ativação do ambiente, use o executável da venv diretamente:

```powershell
.\.venv\Scripts\python.exe -m pip install pandas numpy matplotlib statsmodels pmdarima jupyter ipykernel
```

## Executar o notebook

1. Abra esta pasta no VS Code ou inicie o Jupyter nela.
2. Abra `Trabalho02.ipynb`.
3. Selecione como kernel o Python localizado em `.venv\Scripts\python.exe`.
4. Execute as células em ordem.

O notebook carrega a série a partir do CSV local, exibe gráficos da série, decomposição, tendência, sazonalidade e médias anuais, compara Holt-Winters e ARIMA usando MAPE e, por fim, seleciona o método com menor MAPE para prever 12 meses. As respostas observadas na base incluída estão resumidas abaixo.

## Respostas da análise

### A série possui tendência? Se sim, que tipo?

Sim. A tendência de longo prazo é **não linear**: a produção média anual sobe de cerca de 86 megalitros em 1956 até valores próximos de 167 megalitros no fim dos anos 1970. Depois, oscila e apresenta uma trajetória geral de queda, chegando a cerca de 148 megalitros em 1994. Portanto, não há crescimento contínuo ao longo de todo o período.

### A série possui sazonalidade? Qual é o período?

Sim. Há um padrão sazonal que se repete a cada **12 meses**. Em média, a produção é maior no fim do ano, especialmente em novembro e dezembro, e menor no meio do ano, sobretudo em junho. Isso descreve o padrão observado, sem afirmar uma causa que o dataset não informa.

### A série apresenta um ciclo? Se sim, por qual razão?

Há oscilações de vários anos na média anual, incluindo uma alta até o fim dos anos 1970 e uma queda posterior. Porém, o gráfico não mostra um ciclo regular com duração fixa. **A razão dessas oscilações não pode ser determinada apenas com esta série**; seriam necessários dados externos, por exemplo sobre demanda, custos, produção agrícola ou condições econômicas.

### Qual método é mais adequado para previsão?

Na comparação feita pelo notebook, os últimos 20% da série (95 meses, de outubro de 1987 a agosto de 1995) são usados como teste. O ARIMA obteve MAPE de aproximadamente **6,28%**, contra **6,81%** do Holt-Winters. Como o erro foi menor, o notebook seleciona **ARIMA** para a previsão final. A seleção depende da divisão de treino e teste e dos parâmetros usados.

### Quais são as previsões para os próximos 12 meses?

Com os dados deste CSV até agosto de 1995, a execução seleciona ARIMA e estima aproximadamente:

| Mês | Produção prevista (Megalitros) |
| --- | ---: |
| Set/1995 | 136,35 |
| Out/1995 | 159,43 |
| Nov/1995 | 182,02 |
| Dez/1995 | 183,11 |
| Jan/1996 | 145,43 |
| Fev/1996 | 136,73 |
| Mar/1996 | 151,95 |
| Abr/1996 | 136,41 |
| Mai/1996 | 136,76 |
| Jun/1996 | 123,92 |
| Jul/1996 | 130,06 |
| Ago/1996 | 139,88 |

Os valores podem variar um pouco conforme as versões das bibliotecas e o ajuste do modelo.

### Que insight isso pode oferecer a um negócio?

A sazonalidade pode ajudar uma cervejaria a planejar produção, estoques e distribuição com antecedência, considerando maior produção no fim do ano e menor no meio do ano. A tendência de queda observada depois do fim dos anos 1970 também pode motivar uma investigação de mercado. A série, sozinha, não revela as causas nem permite concluir que os mesmos padrões atuais continuarão no futuro.

## Observações

- A primeira execução do ARIMA pode levar algum tempo, pois o `auto_arima` avalia combinações de parâmetros.
- O MAPE é calculado sobre os últimos 20% dos dados, reservados como conjunto de teste.
- A previsão final usa toda a série depois da comparação dos métodos.
- O código pressupõe dados mensais positivos e uma sazonalidade anual de 12 períodos.
