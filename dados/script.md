# Script para dividir arquivos CSV grandes em pedaços menors

## No Linux (shell):

#### Contar a quantidade de linhas:

`wc -l arquivogrande`

#### Separar o arquivogrande em partes de x linhas
Para separar o arquivo grande em x partes no padrão parteszz,
sendo zz o número de cada parte, por exemplo se o arquivo tiver 102 linhas
e dividir em partes de 10 linhas, teremos 11 partes

`split -d --lines=x arquivogrande parte`

#### Gerar um arquivo com o cabeçalho

`head -1 parte00 > cabecalho`

#### Gerar todos os arquivos com o cabeçalho a partir do arquivo parte01

`cat cabecalho parte01 > parte_01 && rm parte01`

## No Python:

#### Ler os arquivos no python

`>>> parte_00 = pd.read_csv("parte_00", delimiter=";", encoding="ISO-8859-1")`

#### Transformar cada parte em um DataFrame

`>>> df00 = pd.DataFrame(parte_00)`

#### Fazer uma query em cada arquivo
```
>>> df_00 = df00.query('NU_IDADE <= 14')
>>> (...)
>>> df_10 = df10.query('NU_IDADE <= 14')
```
#### Concatenar os resultados
```
>>>frames = [df1, df2, df3, ...]

>>>result = pd.concat(frames)
```

#### Gerar arquivo csv

`>>> result.to_csv('df_final.csv', index=False)`
