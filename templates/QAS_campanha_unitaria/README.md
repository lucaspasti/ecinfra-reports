O template único é `relatorio.html`. Para gerar a prévia de validação:

```sh
npm run report:build
```

Gera o HTML e o PDF completos de validação em `.report-to-mustache/previews/`.
No PowerShell com restrição a scripts, use `npm.cmd` no lugar de `npm`.

Para gerar com dados preenchidos:

```sh
node scripts/render-report.js caminho/dados.json
node scripts/export-report.js
```

`exemplo.json` documenta o formato de todas as variáveis. Os resultados nele
são marcadores, não dados de uma campanha real. A execução sem argumentos
mantém os campos QAG visíveis para validação do layout.

O exportador usa Edge ou Chrome instalado. Em outro ambiente, defina
`REPORT_BROWSER_PATH` com o caminho do navegador. Ele verifica imagens,
divide tabelas e textos excedentes, repete os cabeçalhos das tabelas,
atualiza a numeração e o sumário e verifica se há conteúdo cortado.
Blocos indivisíveis maiores que a área útil são rejeitados com erro.
O HTML salvo após a exportação tem a mesma paginação do PDF.

`QAG_17` é uma string com o endereço da imagem do mapa, usada no atributo `src`.
Endereços relativos são resolvidos em relação ao HTML gerado.

`QAG_18` é uma lista de objetos, iterada com `{{#QAG_18}}`:

```json
{
  "QAG_17": "https://exemplo.com/mapa.png",
  "QAG_18": [
    { "PONTO": "P01", "UTM_E": "000000", "UTM_N": "0000000", "CLASSE": "Salobra — classe 1" }
  ]
}
```

Na geração com imagem real, omita `PREVIA_SEM_MAPA` ou defina como `false`.
Essa opção serve somente para mostrar o espaço reservado ao mapa na validação visual.

`QAG_19` é a lista de parâmetros da Tabela 2. Cada item contém `GRUPO`,
`PARAMETRO` e `PERIODICIDADE`; o template gera uma linha por item.

`QAG_27` contém a Tabela 5 em A3 horizontal, conforme a seção do DOCX:

- `PARAMETROS`: lista de objetos com `NOME`, `MEDIA`, `VMP` e `UNIDADE`.
- `LINHAS`: lista com `PONTO`, `PROFUNDIDADE` e `VALORES`.

`VALORES` deve ter o mesmo tamanho e seguir a mesma ordem de `PARAMETROS`.
A prévia usa 23 pontos, três profundidades e 13 parâmetros. As matrizes de
resultados `QAG_34` (metais) e `QAG_40` (orgânicos) usam o mesmo formato.
Linhas excedentes continuam em outra página horizontal; média, VMP e unidade
aparecem na última parte. Quantidades maiores de colunas podem exigir ajuste
da largura ou separação dos parâmetros.

Demais estruturas:

| Variável | Formato |
| --- | --- |
| `QAG_20` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QAG_21`, `QAG_25`, `QAG_28`, `QAG_33`, `QAG_48` | Texto com parâmetros ou procedimentos |
| `QAG_22`, `QAG_23`, `QAG_24` | Endereço da imagem do equipamento |
| `QAG_26` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QAG_29`, `QAG_36`, `QAG_49` | Lista de gráficos: `TITULO`, `URL` da imagem |
| `QAG_32`, `QAG_37`, `QAG_39` | Lista de médias: `PARAMETRO`, `TOTAL`, `SUPERFICIE`, `MEIO`, `FUNDO` |
| `QAG_31`, `QAG_35`, `QAG_38` | Percentual de conformidade, sem o símbolo `%` |
| `QAG_41`, `QAG_42` | Texto do parecer e medidas; aceita quebras de linha |
| `QAG_43` | Lista de anexos: `TITULO` e opcionais `TEXTO`, `IMAGEM`, `URL`; lista vazia exibe “Sem anexos” |
| `QAG_47` | Lista: `INDICADOR`, `META`, `RESULTADO` |

As opções `PREVIA_SEM_MAPA`, `PREVIA_SEM_SONDA`, `PREVIA_SEM_GARRAFA` e
`PREVIA_SEM_TRANSPORTE` devem ser omitidas ou `false` quando houver imagens
reais. Para gráficos, `URL` vazio mostra o espaço reservado da validação.
Links de anexos aparecem no documento; PDFs externos não são incorporados.

Os textos passam pelo escape padrão do Mustache. Não envie HTML nos campos
de texto; envie listas nos campos estruturados e endereços nos campos de imagem.
Os cálculos de médias e de conformidade devem chegar prontos nos dados.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
