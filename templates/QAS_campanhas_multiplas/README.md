O arquivo editável único é `relatorio.html`, convertido de `reports/2. QualidadeAguaSuperficial_MultiplasCampanhas.docx`. `contrato.json` rastreia os 56 marcadores ao contexto original; `exemplo.json` demonstra os formatos, sem resultados laboratoriais reais.

Para gerar a validação completa, a partir da raiz:

```sh
npm.cmd run report:multi:build
```

Saídas em `.report-to-mustache/previews-multiplas-campanhas/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. São 25 páginas nesta prévia: 20 A4 verticais e 5 A3 horizontais. O sumário e os rodapés são atualizados após paginar.

Para preencher com dados reais, use uma pasta de saída própria:

```sh
node scripts/render-multi-report.js caminho/dados.json saida-multiplas
node scripts/export-report.js saida-multiplas "2. QualidadeAguaSuperficial_MultiplasCampanhas.html"
```

O exportador usa Edge/Chrome instalado; `REPORT_BROWSER_PATH` permite indicar outro executável. Imagens relativas são resolvidas em relação ao HTML gerado. O PDF é exportado após aguardar fontes e imagens; imagens indisponíveis interrompem a geração.

Decisões: manter o conteúdo e a sequência do DOCX, o timbrado EC infra com a remoção aprovada da marca S8 arq., tabelas brancas e texto preto. A numeração inclui Introdução e corresponde ao corpo e ao sumário. Tabelas 5 e 9 e figuras 6, 7 e 8 usam A3 horizontal; demais páginas usam A4 vertical, incluindo a tabela de metais. As imagens de timbrado estão em `../_shared/assets/`; `ASSET_BASE` é derivado pelo renderizador. Todas as partes e estilos permanecem no único HTML.

Campos simples são textos escapados pelo Mustache, mantendo os códigos do DOCX. `QAG_50`/`QAG_51` são mês/ano inicial, `QAG_52`/`QAG_53` mês/ano final, `QAG_55`/`QAG_56` identificam mês/ano da campanha de maior inconformidade e `QAG_57` completa a frase sobre a realização de dragagem. `QAG_04` permanece como ano nas fontes. `QAG_62` e `QAG_63` descrevem equipamento e recipientes de coleta. Os contextos dos demais campos simples estão no contrato.

Estruturas:

| Campo | Conteúdo interno |
| --- | --- |
| `QAG_17`, `QAG_22`, `QAG_23`, `QAG_24`, `QAG_58` | Endereço de imagem: mapa, sonda, equipamento de coleta, transporte e gráfico de inconformidades do período |
| `QAG_18` | Lista com `PONTO`, `UTM_E`, `UTM_N`, `CLASSE` |
| `QAG_19` | Lista com `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `QAG_20` | Objeto com `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QAG_26` | Lista com `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QAG_59`, `QAG_60`, `QAG_61` | Matrizes de percentuais de inconformidade por campanha e profundidade, respectivamente físico-químicos/biológicos, metais e HPA |
| `QAG_32`, `QAG_37`, `QAG_39` | Listas de médias: `PARAMETRO`, `TOTAL`, `SUPERFICIE`, `MEIO`, `FUNDO` |
| `QAG_29`, `QAG_36`, `QAG_49` | Listas de gráficos: `TITULO`, `URL` da imagem |
| `QAG_47` | Lista com `INDICADOR`, `META`, `RESULTADO` |
| `QAG_41`, `QAG_42` | Parecer e medidas, como texto; preservam quebras de linha |
| `QAG_43` | Lista de anexos: `TITULO` obrigatório; `TEXTO`, `IMAGEM`, `URL` opcionais; lista vazia exibe “Sem anexos” |

Cada matriz tem `PARAMETROS: [{NOME}]` e `LINHAS: [{CAMPANHA, PROFUNDIDADE, VALORES: [...]}]`. Cada linha deve ter exatamente um valor por parâmetro, na mesma ordem. Informe os percentuais já formatados, incluindo `%` quando necessário. Repita o identificador da campanha para cada profundidade, inclusive nas continuações. Para ausências use um marcador textual explícito, como `—`; não confunda ausência com zero. Nas médias, preserve unidade no nome do parâmetro. Resultados abaixo de quantificação podem vir como `<LQ`, escapados normalmente.

Os percentuais, médias, gráficos, pareceres e mapas devem ser fornecidos pelo responsável; o gerador não os calcula. Não envie HTML nos textos. Um link de anexo aparece como link e não incorpora páginas de PDF externo. Blocos indivisíveis, inclusive anexos compostos, maiores que uma página são rejeitados e devem ser divididos em itens menores.

Sem argumentos, o renderizador usa espaços reservados visuais. Para dados reais, remova ou defina como `false` as quatro opções `PREVIA_SEM_*`, preencha todos os endereços de imagens e as URLs dos gráficos. A imagem `QAG_58` vazia é permitida somente na prévia sem argumentos. Listas de gráficos vazias indicam ausência de figuras fornecidas; itens presentes exigem URL na geração preenchida.

Validação de volume:

```sh
node scripts/validate-multi-report.js
```

Gera arquivos separados em `.report-to-mustache/multi-stress/`, com 156 linhas em cada matriz, 120 parâmetros e parecer de 2.000 palavras. O teste produziu 46 páginas, com 21 continuações, preservando a sequência e o conteúdo de todos os parágrafos e linhas. O exportador também verifica cortes verticais, imagens e cores das tabelas. O teste confirma contagem e dimensões reais das páginas no PDF.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
