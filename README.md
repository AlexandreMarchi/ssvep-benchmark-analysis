# Análise do SSVEP Benchmark Dataset

Projeto de análise exploratória de sinais EEG do **Benchmark Dataset para SSVEP-BCI**, disponibilizado pelo grupo de pesquisa em interfaces cérebro-computador da Universidade Tsinghua.

O objetivo inicial é carregar e documentar os dados, realizar verificações básicas e preparar uma estrutura reprodutível para análises e pré-processamento futuros.

## Fonte dos dados

- Página oficial de download: https://bci.med.tsinghua.edu.cn/download.html
- Dataset utilizado: **Benchmark Dataset**
- Os arquivos brutos devem ser obtidos diretamente da fonte oficial e mantidos localmente. Este repositório não armazena os arquivos EEG brutos.

## Estrutura do projeto

```text
ssvep-benchmark-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 01_load_dataset.ipynb
├── docs/
│   └── dataset_description.md
└── data/
    ├── raw/        # arquivos originais; não versionar
    └── processed/  # dados derivados; não versionar por padrão
```

Os nomes dos notebooks podem variar conforme as versões utilizadas. Mantenha-os na pasta `notebooks/`.

## Organização esperada dos dados

Coloque os arquivos extraídos do dataset em `data/raw/`. Para a análise inicial do participante 1, o arquivo esperado é `S1.mat`. Os arquivos de metadados disponibilizados na página oficial, como `Readme.txt`, `Sub_info.txt`, `64-channels.loc` e `Freq_Phase.mat`, também podem ser guardados nessa pasta para consulta.

Estrutura local de exemplo:

```text
data/raw/
├── S1.mat
├── Freq_Phase.mat
├── Sub_info.txt
├── 64-channels.loc
└── Readme.txt
```

A estrutura acima é apenas um exemplo dos arquivos utilizados nesta etapa. Os arquivos podem variar conforme o que foi baixado.

## Ambiente e execução

O notebook foi desenvolvido para Python e utiliza as bibliotecas:

- NumPy
- pandas
- Matplotlib
- SciPy

Instale as dependências no ambiente do Jupyter, caso ainda não estejam disponíveis. Para executar os notebooks:

1. Clone ou baixe este repositório.
2. Obtenha os dados pela página oficial e extraia-os localmente.
3. Coloque os arquivos em `data/raw/`.
4. Abra o Jupyter na raiz do projeto ou abra os notebooks pela pasta `notebooks/`.
5. Execute as células em ordem.

Os notebooks utilizam o caminho relativo `../data/raw`, considerando que estão dentro da pasta `notebooks/`. Se o diretório de trabalho do ambiente for diferente, ajuste a variável `DATA_DIR` no notebook.

## Resultados da inspeção inicial

Foi carregado o arquivo `S1.mat` e identificada a variável `data`, com as seguintes características:

| Propriedade | Resultado |
|---|---:|
| Formato da matriz | `(64, 1500, 40, 6)` |
| Tipo numérico | `float64` |
| Número total de valores | 23.040.000 |
| Tamanho aproximado da matriz | 175,78 MiB |
| Valores não finitos (`NaN`/infinitos) | 0 |

A interpretação utilizada para os eixos, conforme a organização esperada do Benchmark Dataset, é:

1. 64 canais EEG;
2. 1.500 amostras temporais por ensaio;
3. 40 alvos visuais;
4. 6 blocos experimentais.

Considerando a frequência de amostragem de 250 Hz informada para o dataset, 1.500 amostras correspondem a 6 segundos por ensaio.

### Estatísticas descritivas globais de `S1`

| Medida | Resultado |
|---|---:|
| Mínimo | -533,360901 |
| Máximo | 267,105469 |
| Média | 0,014452 |
| Desvio padrão | 8,738303 |
| Mediana | -0,071763 |

Os valores extremos devem ser investigados em análises posteriores. Eles não são classificados automaticamente como erros ou artefatos apenas com base nestas estatísticas.

### Observações dos gráficos iniciais

- Foi visualizado um sinal do canal 1, alvo 1 e bloco 1 ao longo de aproximadamente 6 segundos.
- O gráfico de amplitude RMS apresentou variação entre canais; os canais 2 e 3 tiveram os maiores valores RMS nesta inspeção.
- A verificação não encontrou valores não finitos nos dados do participante 1.

Esses resultados são exploratórios e referem-se somente ao participante `S1`. Não permitem generalizar conclusões para os demais participantes nem afirmar que um canal é defeituoso.

## Estado atual e próximas etapas

Concluído nesta versão:

- carregamento do arquivo `S1.mat`;
- identificação da variável `data` e de suas dimensões;
- cálculo de estatísticas descritivas globais;
- verificação de valores não finitos;
- visualização inicial de um sinal;
- cálculo e visualização da amplitude RMS por canal.

Próximas etapas possíveis:

- inspecionar `Freq_Phase.mat` e documentar os metadados dos estímulos;
- realizar análise no domínio da frequência;
- investigar os valores extremos e possíveis artefatos;
- definir, documentar e validar uma estratégia de pré-processamento;
- expandir a análise para outros participantes.

Nenhum filtro, normalização ou outra transformação de pré-processamento foi aplicado nesta etapa. Os dados brutos devem permanecer preservados para manter a rastreabilidade.

## Cuidados com o GitHub

Não envie os arquivos EEG brutos ao GitHub. Eles podem ser grandes e são dados de origem do estudo. Versione os notebooks, a documentação e o código; descreva no README como obter os dados na fonte oficial.

Inclua regras no `.gitignore` para evitar o envio acidental dos dados brutos e derivados, por exemplo:

```gitignore
data/raw/
data/processed/
.ipynb_checkpoints/
__pycache__/
```

Confira sempre o estado do Git antes de realizar um commit para garantir que os arquivos de dados não foram adicionados.
