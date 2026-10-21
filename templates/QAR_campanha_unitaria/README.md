O template editável único é `relatorio.html`, convertido individualmente de `reports/1. QualidadeAr_CampanhaUnitaria.docx`. Todos os estilos estão no HTML; `../_shared/assets/` contém os recursos originais. `contrato.json` rastreia 42 marcadores QAR ao contexto do DOCX.

Para gerar a prévia completa, a partir da raiz:

```sh
npm.cmd run report:air:unit:build
```

Saídas em `.report-to-mustache/previews-ar-unitaria/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. A prévia tem 17 páginas, sendo 16 A4 verticais e uma A3 horizontal. São marcadores de validação, sem dados laboratoriais reais. As listas de metodologia usam exemplos compactos; a matriz de resultados mantém 15 linhas de pontos, conforme a estrutura vazia do original.

Para preencher com dados reais e manter a prévia separada:

```sh
node scripts/render-air-report.js unit caminho/dados.json saida-ar-unit
node scripts/export-report.js saida-ar-unit "1. QualidadeAr_CampanhaUnitaria.html"
```

O exportador utiliza Edge/Chrome instalado; `REPORT_BROWSER_PATH` permite indicar outro executável. Aguarda fontes e imagens e rejeita imagens indisponíveis. Endereços relativos são resolvidos em relação ao HTML gerado. Na geração com JSON fornecido, as quatro imagens fixas são obrigatórias e os gráficos presentes precisam de URL. Sem argumentos, imagens vazias mostram espaços reservados. Listas de gráficos vazias indicam ausência de figuras fornecidas.

Decisões: timbrado EC infra com remoção aprovada da marca antiga S8 arq.; tabelas brancas e texto preto; A4 vertical e Tabela 5 em A3 horizontal conforme as seções do DOCX. Títulos, narrativa, unidades, legendas e referências foram preservados. O sumário segue o corpo, incluindo Introdução, Medidas de mitigação e Anexos na ordem efetiva. As páginas do sumário e os rodapés são recalculados após paginar. O HTML final corresponde ao PDF.

A Tabela 5 contém concentrações por ponto e uma linha de unidade exibida somente na última parte. As unidades do DOCX foram mantidas, inclusive ppm para Monóxido de Carbono.

O conteúdo técnico foi transcrito sem revisão normativa. Foram mantidas as divergências do original: Portaria IBAMA 85 datada em julho no corpo e em outubro nas referências; texto sobre PI-2 e seus prazos; limiar de 60% na Tabela 6 e texto sobre Ringelmann. O gerador não concilia esses conteúdos nem calcula conformidade, classificação, médias ou resultados. As observações estão registradas no contrato.

| Campo | Formato |
| --- | --- |
| QAR_17 | Endereço da imagem do mapa |
| QAR_18 | Lista: PONTO, UTM_E, UTM_N |
| QAR_19 | Lista: PARAMETRO, PERIODICIDADE |
| QAR_20 | Objeto: NOME_FANTASIA, RAZAO_SOCIAL, CNPJ, ENDERECO, RESPONSAVEL, EMAIL, CONTATO |
| QAR_21 | Endereço da imagem do amostrador |
| QAR_26 | Lista: TIPO, INDICADOR, UNIDADE, META |
| QAR_28 | Matriz da Tabela 5: PARAMETROS e LINHAS |
| QAR_29 | Lista de gráficos: TITULO, URL |
| QAR_30 | Endereço do gráfico de número de veículos por densidade de fumaça |
| QAR_33 | Lista: DATA, DENSIDADE, VEICULO |
| QAR_34 | Endereço do gráfico de tipologias e densidades de fumaça |
| QAR_35 | Lista: INDICADOR, META, RESULTADO |
| QAR_36, QAR_37 | Parecer e medidas; textos com quebras de linha preservadas |
| QAR_38 | Lista de anexos: TITULO obrigatório; TEXTO, IMAGEM, URL opcionais; lista vazia mostra “Sem anexos” |

Matriz de resultados:

```json
{
  "PARAMETROS": [{ "NOME": "[PARÂMETRO]", "UNIDADE": "[UNIDADE]" }],
  "LINHAS": [{ "PONTO": "[IDENTIFICADOR]", "VALORES": ["[VALOR]"] }]
}
```

Cada linha precisa de exatamente um valor por parâmetro, na mesma ordem. Use textos como <LQ para resultados abaixo de quantificação e um marcador explícito para ausência. A unidade aparece somente ao final da tabela. Cabeçalhos e legendas se repetem nas continuações. A matriz mantém os oito poluentes do DOCX; colunas adicionais podem exigir ajuste de largura ou divisão.

QAR_03/QAR_04 são mês/ano da campanha e QAR_27 o percentual geral de conformidade, sem o símbolo %. QAR_31 é o percentual de veículos em inconformidade, sem %, e QAR_32 o total de veículos monitorados. DENSIDADE é informada sem %, já indicado no cabeçalho. QAR_22, QAR_23 e QAR_25 são textos de amostragem/análise, e QAR_24 é o texto complementar do mapa. Os contextos de todos os campos simples estão no contrato.

Textos usam escape padrão Mustache; não envie HTML. As imagens são ajustadas proporcionalmente. Links de anexos aparecem como links e não incorporam páginas de PDF externo. Textos longos podem continuar por páginas; blocos indivisíveis maiores que a área útil são rejeitados.

Validação reproduzível:

```sh
node scripts/validate-air-report.js unit
```

Usa o DOCX de origem e saídas separadas em `.report-to-mustache/air-unit-stress/`. O teste inclui 150 linhas na matriz, 110 parâmetros de metodologia, 80 veículos e parecer de 1.280 palavras com 16 quebras de parágrafo. Produziu 34 páginas, com 17 continuações, preservando conteúdo e quebras. Confere texto do DOCX, títulos, sumário, células e ordem das matrizes, hashes dos recursos de imagem, contagem e dimensões reais do PDF, limites da área útil e rejeição de matriz inconsistente ou mapa obrigatório ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
