O template editável único é `relatorio.html`, convertido de `reports/3. QualidadeAguaSubterranea_CampanhaUnitaria.docx`. Todas as partes e estilos ficam nesse HTML; as imagens originais ficam em `../_shared/assets/`. `contrato.json` rastreia os 54 marcadores `QAGS_*` aos contextos do documento.

Para gerar a prévia completa com marcadores, a partir da raiz:

```sh
npm.cmd run report:groundwater:build
```

Saídas em `.report-to-mustache/previews-subterranea/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. A prévia tem 22 páginas: 19 A4 verticais e 3 A3 horizontais. É uma validação de layout, sem dados laboratoriais reais. O exemplo usa listas compactas; linhas vazias de preenchimento do Word foram substituídas por listas iteráveis.

Para gerar com dados preenchidos, use uma pasta própria:

```sh
node scripts/render-groundwater-report.js caminho/dados.json saida-subterranea
node scripts/export-report.js saida-subterranea "3. QualidadeAguaSubterranea_CampanhaUnitaria.html"
```

O exportador usa Edge ou Chrome instalado. `REPORT_BROWSER_PATH` permite configurar outro executável. Ele aguarda fontes e imagens; imagens indisponíveis interrompem a exportação. Endereços relativos são resolvidos em relação ao HTML gerado. As quatro imagens fixas são obrigatórias na geração preenchida; itens de gráficos presentes precisam de `URL`. Listas de gráficos vazias indicam que nenhuma figura foi fornecida. Sem argumentos, imagens vazias mostram espaços reservados para validação.

Decisões mantidas: timbrado EC infra, remoção aprovada da marca antiga S8 arq., tabelas brancas com texto preto, A4 vertical e tabelas 5, 6 e 7 em A3 horizontal conforme as seções do DOCX. A figura de microrganismos foi corrigida de 7 para 8 no texto e na legenda, eliminando a duplicidade. Os títulos e a numeração seguem este documento, incluindo Referências no sumário. Páginas do sumário e rodapés são recalculadas depois da paginação.

O conteúdo técnico e as referências foram preservados. O original cita CONAMA 396/2008 no corpo e inclui CONAMA 357/2005 na bibliografia; essa divergência foi registrada em `contrato.json`, sem substituir a referência ou realizar revisão normativa.

Campos estruturados:

| Campo | Formato |
| --- | --- |
| `QAGS_17`, `QAGS_22`, `QAGS_23`, `QAGS_24` | Endereço das imagens: mapa, sonda, amostrador e transporte |
| `QAGS_18` | Lista: `PONTO`, `UTM_E`, `UTM_N`, `CLASSE`; `CLASSE` é a classe de uso da água |
| `QAGS_19` | Lista: `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `QAGS_20` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QAGS_26` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QAGS_27`, `QAGS_33`, `QAGS_35` | Matrizes de resultados: inorgânicos, orgânicos e agrotóxicos |
| `QAGS_67` | Matriz de resultados de microrganismos |
| `QAGS_29`, `QAGS_36`, `QAGS_37`, `QAGS_39` | Listas de gráficos: `TITULO`, `URL` |
| `QAGS_40` | Lista: `INDICADOR`, `META`, `RESULTADO` |
| `QAGS_41`, `QAGS_42` | Texto de parecer e medidas; preserva quebras de linha, inclusive nas continuações |
| `QAGS_43` | Lista de anexos: `TITULO` obrigatório; `TEXTO`, `IMAGEM`, `URL` opcionais; lista vazia exibe “Sem anexos” |

Matrizes usam esta estrutura:

```json
{
  "PARAMETROS": [
    { "NOME": "[PARÂMETRO]", "MEDIA": "[MÉDIA]", "VMP": "[VMP]", "UNIDADE": "[UNIDADE]" }
  ],
  "LINHAS": [
    { "PONTO": "[PONTO]", "VALORES": ["[VALOR]"] }
  ]
}
```

Cada linha deve ter um valor por parâmetro, na mesma ordem. A matriz de microrganismos usa `NOME` e `UNIDADE` nos parâmetros, sem `MEDIA` ou `VMP`, conforme a tabela original. Suas unidades de exemplo são `NMP/100 mL`. As outras três matrizes exibem Média, VMP e Unidade somente na última parte da tabela, após todas as linhas. Cabeçalhos e legendas são repetidos nas continuações.

`QAGS_31`, `QAGS_32`, `QAGS_34` e `QAGS_38` são os percentuais de conformidade dos quatro grupos, sem `%` porque o texto já inclui o símbolo. Médias, VMP, conformidade, mapas, gráficos e pareceres devem chegar prontos nos dados; o template não realiza cálculos científicos. Use `<LQ` ou outro texto documentado para valores abaixo da quantificação, e `—` para ausência de resultado, sem confundir com zero.

Os demais códigos são textos simples, com o contexto descrito no contrato e exemplos em `exemplo.json`. `QAGS_03`/`QAGS_04` são mês/ano da campanha; `QAGS_28`, `QAGS_57`, `QAGS_66`, `QAGS_68` descrevem os parâmetros dos gráficos; `QAGS_62`, `QAGS_65`, `QAGS_63`, `QAGS_64` descrevem equipamento, tipo de amostra, recipientes e transporte.

Textos usam escape padrão Mustache; não envie HTML. Imagens mantêm as proporções. Um link de anexo não incorpora páginas de PDF externo. Anexos são blocos de título, texto e imagem, permitindo continuação do texto; imagens maiores devem ser ajustadas para caber na área útil. Blocos indivisíveis maiores que uma página são rejeitados. A prévia e o PDF usam a mesma distribuição de conteúdo.

Validação reproduzível:

```sh
node scripts/validate-groundwater-report.js
```

Usa o DOCX de origem e gera arquivos separados em `.report-to-mustache/subterranea-stress/`. O teste usa 150 linhas em cada matriz, 110 parâmetros na tabela de metodologia e parecer de 1.280 palavras dividido em 16 parágrafos. Produziu 52 páginas com 30 continuações, preservando os textos, quebras de linha, linhas de tabela e resumos finais. Confere também o texto do DOCX, dimensões e contagem real de páginas no PDF, limites de conteúdo e rejeição de matriz inconsistente ou imagem obrigatória ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
