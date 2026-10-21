O arquivo editável único é `relatorio.html`, convertido de `reports/4. QualidadeAguaSubterranea_MultiplasCampanhas.docx`. Todas as partes e estilos estão nesse HTML; `../_shared/assets/` contém as imagens originais. `contrato.json` rastreia os 56 marcadores `QAGS_*` ao contexto do DOCX.

Para gerar a prévia completa com marcadores, a partir da raiz:

```sh
npm.cmd run report:groundwater:multi:build
```

Saídas em `.report-to-mustache/previews-subterranea-multiplas/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. A prévia tem 22 páginas: 19 A4 verticais e 3 A3 horizontais. Os valores são marcadores de validação, sem resultados laboratoriais reais. As linhas vazias do Word foram convertidas em listas; as listas de metodologia do exemplo são compactas.

Para gerar com dados preenchidos, use uma pasta de saída própria:

```sh
node scripts/render-groundwater-multi-report.js caminho/dados.json saida-subterranea-multiplas
node scripts/export-report.js saida-subterranea-multiplas "4. QualidadeAguaSubterranea_MultiplasCampanhas.html"
```

O exportador utiliza Edge ou Chrome instalado; `REPORT_BROWSER_PATH` permite informar outro executável. Aguarda fontes e imagens e interrompe a exportação em caso de imagem indisponível. Endereços relativos são resolvidos em relação ao HTML gerado. Na geração preenchida, as cinco imagens fixas são obrigatórias; os itens de gráficos presentes precisam de `URL`. Listas de gráficos vazias indicam ausência de figuras fornecidas. Sem argumentos, imagens vazias mostram espaços reservados de validação.

Decisões: timbrado EC infra, com remoção aprovada da marca antiga S8 arq.; tabelas brancas com texto preto. As tabelas 5, 6 e 7 usam A3 horizontal conforme as seções do DOCX; as demais páginas são A4 verticais. Os títulos e o sumário foram alinhados ao corpo, incluindo Introdução, que faltava no sumário original, e Medidas de Mitigação. As figuras seguem a sequência 1 a 9 do documento. Sumário e rodapés são recalculados após a paginação; o HTML final e o PDF têm a mesma distribuição.

O conteúdo técnico e as referências foram preservados. O corpo cita CONAMA 396/2008 e a bibliografia inclui CONAMA 357/2005; essa divergência original está registrada no contrato, sem substituição da referência ou revisão normativa.

Campos estruturados:

| Campo | Formato |
| --- | --- |
| `QAGS_17`, `QAGS_22`, `QAGS_23`, `QAGS_24` | Endereços de imagem: mapa, sonda, amostrador e transporte |
| `QAGS_58` | Endereço da Figura 5: gráfico geral de percentuais de inconformidades por campanha |
| `QAGS_18` | Lista: `PONTO`, `UTM_E`, `UTM_N`, `CLASSE`; classe de uso da água |
| `QAGS_19` | Lista: `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `QAGS_20` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QAGS_26` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QAGS_69`, `QAGS_70`, `QAGS_71`, `QAGS_72` | Matrizes de percentuais de inconformidade: inorgânicos, orgânicos, agrotóxicos e microrganismos |
| `QAGS_29`, `QAGS_36`, `QAGS_37`, `QAGS_39` | Listas de gráficos de concentração: `TITULO`, `URL` |
| `QAGS_40` | Lista: `INDICADOR`, `META`, `RESULTADO` |
| `QAGS_41`, `QAGS_42` | Texto de parecer e medidas; preserva quebras de linha, inclusive nas continuações |
| `QAGS_43` | Lista de anexos: `TITULO` obrigatório; `TEXTO`, `IMAGEM`, `URL` opcionais; lista vazia exibe “Sem anexos” |

Cada matriz usa este formato:

```json
{
  "PARAMETROS": [{ "NOME": "[PARÂMETRO]" }],
  "LINHAS": [{ "CAMPANHA": "[CAMPANHA]", "VALORES": ["[%]"] }]
}
```

Cada linha precisa de exatamente um valor por parâmetro, na mesma ordem. Informe percentuais já formatados, incluindo o símbolo `%` quando necessário. Para ausência de resultado, use um marcador explícito como `—`, sem confundir com zero. As matrizes não têm profundidade, média, VMP ou unidade em linhas de resumo, conforme as tabelas desta versão. Cabeçalhos e legendas se repetem nas continuações. O exemplo mantém 15, 10, 13 e 3 parâmetros e 10 campanhas em cada matriz, refletindo a estrutura vazia do DOCX.

`QAGS_50`/`QAGS_51` são mês/ano inicial e `QAGS_52`/`QAGS_53` mês/ano final. `QAGS_55`/`QAGS_56` identificam mês/ano da campanha com maior percentual de inconformidades. `QAGS_04` permanece como ano nas fontes. `QAGS_28`, `QAGS_57`, `QAGS_66` e `QAGS_68` descrevem os parâmetros selecionados para os quatro grupos de gráficos. `QAGS_62`, `QAGS_65`, `QAGS_63` e `QAGS_64` descrevem equipamento de coleta, tipo de amostra, recipientes e transporte. Os demais códigos simples têm seus contextos no contrato.

Percentuais, médias das campanhas presentes nos gráficos, VMP, mapas, gráficos e pareceres devem ser fornecidos pelo responsável. O gerador não realiza cálculos científicos. Textos usam escape padrão Mustache; não envie HTML arbitrário. As imagens preservam as proporções. Links de anexos aparecem como links e não incorporam páginas de PDF externo. Textos dos anexos podem continuar nas próximas páginas; blocos indivisíveis maiores que uma página são rejeitados.

Para repetir a validação de volume:

```sh
node scripts/validate-groundwater-multi-report.js
```

O teste usa o DOCX de origem e saídas separadas em `.report-to-mustache/subterranea-multiplas-stress/`: 150 linhas em cada matriz, 110 parâmetros na metodologia e parecer com 1.280 palavras e 16 quebras de parágrafo. Produziu 52 páginas, com 30 continuações, preservando parágrafos, quebras de linha e linhas de tabela. Verifica conteúdo do DOCX, contagem e dimensões reais das páginas do PDF, limites de conteúdo e rejeição de matriz inconsistente ou imagem obrigatória ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
