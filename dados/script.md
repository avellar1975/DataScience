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
```
cat cabecalho parte01 > parte_01 && rm parte01`
(...)
cat cabecalho parte010 > parte_010 && rm parte010`

```
## No Python:
---

"""Script para selecionar dados para análise."""
import pandas as pd

lista_arquivos = [f'parte_0{x}' for x in range(11)]

provas = ['NU_NOTA_REDACAO',
          'NU_NOTA_CN',
          'NU_NOTA_CH',
          'NU_NOTA_LC',
          'NU_NOTA_MT']

frames = []

for arquivo in lista_arquivos:
    file_temp = pd.read_csv(f"{arquivo}", delimiter=";", encoding="ISO-8859-1")
    file_temp = pd.DataFrame(file_temp)
    print(f'Arquivo {arquivo} criado como DataFrame')
    file_temp['NU_NOTA_FINAL'] = file_temp[provas].sum(axis=1)
    print(f'Coluna NU_NOTA_FINAL criada')
    print(f'Total de linhas e colunas: {file_temp.shape}')
    file_temp = file_temp.query('NU_NOTA_FINAL > 2754.125')
    print(f'Query do {arquivo} concluída com total de registros: {file_temp.shape} ')
    frames.append(file_temp)

print('Iniciando concatenação...')
resultado = pd.concat(frames)

print('Salvando csv...')
resultado.to_csv('data_final.csv', index=False)
print('Fim da operação')
```
