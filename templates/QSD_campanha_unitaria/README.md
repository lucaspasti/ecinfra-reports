O template editável único é `relatorio.html`, convertido de `reports/1. QualidadeSedimentos_CampanhaUnitaria.docx`. Todas as partes e estilos estão no HTML; `../_shared/assets/` contém os recursos originais. `contrato.json` rastreia os 52 marcadores `QSD_*` ao contexto do DOCX e registra as decisões de conversão.

Para gerar a prévia completa com marcadores, a partir da raiz:

```sh
npm.cmd run report:sediments:build
```

Saídas em `.report-to-mustache/previews-sedimentos/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. A prévia tem 20 páginas: 16 A4 verticais e 4 A3 horizontais. É uma validação de layout, com marcadores e sem resultados laboratoriais reais. Listas de metodologia e de pontos usam exemplos compactos; as quatro matrizes químicas mantêm 15 linhas de pontos, como na estrutura vazia do original.

Para gerar com dados reais, use uma pasta de saída própria:

```sh
node scripts/render-sediments-report.js caminho/dados.json saida-sedimentos
node scripts/export-report.js saida-sedimentos "1. QualidadeSedimentos_CampanhaUnitaria.html"
```

O exportador usa Edge/Chrome instalado; `REPORT_BROWSER_PATH` permite informar outro executável. Ele aguarda fontes e imagens e interrompe a exportação para imagens indisponíveis. Endereços relativos são resolvidos em relação ao HTML gerado. Na geração com JSON fornecido, as quatro imagens fixas são obrigatórias; itens presentes na lista de gráficos precisam de `URL`. Sem argumentos, os campos de imagem vazios mostram espaços reservados para validação.

Decisões: manter o timbrado EC infra com a remoção aprovada da marca antiga S8 arq., tabelas brancas com texto preto e imagens proporcionais. As tabelas químicas 5–8 ficam em A3 horizontal, conforme a seção do DOCX, incluindo nutrientes/COT; demais páginas usam A4 vertical. O sumário segue os títulos efetivamente presentes no corpo: inclui Introdução e Medidas de Mitigação, com numeração propagada às subseções. Sumário e rodapés são atualizados depois de paginar; o HTML final e o PDF representam a mesma distribuição. A tabela do laboratório fica inteira na prévia e o texto da metodologia é distribuído sem depender de parágrafos vazios do Word.

Conteúdo técnico e referências foram preservados, inclusive divergências da origem: o texto da caracterização ecotoxicológica menciona amônia ionizada, enquanto a tabela usa amônia não ionizada; a referência à CONAMA 454 informa 2022, enquanto o corpo e o endereço da fonte indicam 2012; Almeida/Rocha aparece duas vezes e o endereço ASTM está junto da referência seguinte. Essas ocorrências estão registradas no contrato; não houve revisão técnica ou normativa nem substituição silenciosa dos textos.

Campos estruturados:

| Campo | Formato |
| --- | --- |
| `QSD_17` | Endereço do mapa dos pontos |
| `QSD_18` | Lista: `PONTO`, `UTM_E`, `UTM_N`, `CLASSE` |
| `QSD_19` | Lista: `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `QSD_20` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QSD_21`, `QSD_25` | Endereços das imagens do amostrador e do transporte |
| `QSD_28` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QSD_29` | Endereço da imagem de composição granulométrica |
| `QSD_38` | Matriz da Tabela 5: metais pesados e arsênio |
| `QSD_39` | Matriz da Tabela 6: pesticidas organoclorados, TBT e PCBs |
| `QSD_40` | Matriz da Tabela 7: HPAs |
| `QSD_41` | Matriz da Tabela 8: nutrientes e COT |
| `QSD_42` | Lista: `PONTO`, `MORTALIDADE`, `AMONIA_NAO_IONIZADA`, `TOXICIDADE` |
| `QSD_47` | Lista: `INDICADOR`, `META`, `RESULTADO` |
| `QSD_48`, `QSD_49` | Parecer e medidas, como texto com quebras de linha preservadas |
| `QSD_50` | Lista de anexos: `TITULO` obrigatório e `TEXTO`, `IMAGEM`, `URL` opcionais; lista vazia mostra “Sem anexos” |
| `QSD_51` | Lista de gráficos químicos: `TITULO`, `URL` da imagem |

As quatro matrizes usam este formato:

```json
{
  "PARAMETROS": [{ "NOME": "[PARÂMETRO]", "UNIDADE": "[UNIDADE]" }],
  "LINHAS": [{ "PONTO": "[PONTO]", "VALORES": ["[VALOR]"] }]
}
```

Cada linha precisa de um valor por parâmetro, na mesma ordem. Cabeçalho e legenda se repetem nas continuações; a unidade aparece somente na última parte. O exemplo conserva os nomes e unidades das tabelas de origem: 8 parâmetros em metais, 13 em pesticidas/TBT/PCBs, 14 em HPAs e 3 em nutrientes/COT. O COT usa `%`; nitrogênio e fósforo usam `mg/kg`. Não há linhas de média ou VMP nas matrizes, pois não existem nessas tabelas do DOCX. Use texto como `<LQ` para resultados abaixo de quantificação e um marcador explícito para ausência, sem confundir com zero.

Os campos simples mantêm os identificadores originais e seus contextos estão no contrato. `QSD_03`/`QSD_04` são mês/ano da campanha. `QSD_22`, `QSD_24` e `QSD_23` descrevem amostrador, tipo de amostra e transporte. `QSD_26` contém procedimentos analíticos e `QSD_27` o organismo-teste. `QSD_30`–`QSD_36` contêm os sete percentuais médios de granulometria; `QSD_37` é o percentual de pontos em conformidade química e `QSD_43` o percentual de pontos classificados como tóxicos. Envie esses percentuais sem `%`, pois o corpo já contém o símbolo. `MORTALIDADE` também é informado sem `%`, indicado no cabeçalho.

O gerador não calcula médias, granulometria, conformidade ou classificação ecotoxicológica. Resultados, classificações, gráficos e pareceres devem chegar prontos nos dados, fornecidos pelo responsável. Não envie HTML nos textos: o Mustache usa escape padrão. Um link de anexo aparece como link e não incorpora páginas de PDF externo. Textos longos podem continuar por páginas; blocos indivisíveis maiores que a área útil são rejeitados.

Para repetir a validação:

```sh
node scripts/validate-sediments-report.js
```

Usa o DOCX de origem e saídas separadas em `.report-to-mustache/sedimentos-stress/`. O teste inclui 150 linhas em cada matriz, 110 parâmetros de metodologia e parecer de 1.280 palavras com 16 quebras de parágrafo. Produziu 46 páginas, com 26 continuações, preservando parágrafos, quebras de linha e linhas de tabela. Confere conteúdo do DOCX, unidades apenas no final, contagem e dimensões reais das páginas do PDF, limites da área útil e rejeição de matriz inconsistente ou imagem obrigatória ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
