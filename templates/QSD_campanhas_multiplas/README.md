O template editável único é `relatorio.html`, convertido de `reports/2. QualidadeSedimentos_CampanhaMultiplas.docx`. Todas as partes e estilos permanecem no HTML. `../_shared/assets/` contém os recursos originais e `contrato.json` rastreia os 59 marcadores `QSD_*` ao contexto do DOCX.

Para gerar a prévia completa com marcadores, a partir da raiz:

```sh
npm.cmd run report:sediments:multi:build
```

Saídas em `.report-to-mustache/previews-sedimentos-multiplas/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. A prévia tem 22 páginas, sendo 18 A4 verticais e 4 A3 horizontais. É uma validação de layout, sem resultados laboratoriais reais. O exemplo usa listas compactas na metodologia, quatro campanhas nas matrizes químicas e seis pontos na matriz de toxicidade, conforme a estrutura vazia do documento.

Para gerar com dados preenchidos, use uma pasta de saída própria:

```sh
node scripts/render-sediments-multi-report.js caminho/dados.json saida-sedimentos-multiplas
node scripts/export-report.js saida-sedimentos-multiplas "2. QualidadeSedimentos_CampanhaMultiplas.html"
```

O exportador usa Edge/Chrome instalado; `REPORT_BROWSER_PATH` permite indicar outro executável. Aguarda fontes e imagens e interrompe a exportação para imagens indisponíveis. Endereços relativos são resolvidos em relação ao HTML gerado. Com JSON fornecido, as seis imagens fixas são obrigatórias e os itens presentes na lista de gráficos precisam de `URL`. Listas de gráficos vazias indicam ausência de figuras fornecidas. Sem argumentos, imagens vazias exibem espaços reservados de validação.

Decisões: timbrado EC infra com remoção aprovada da marca antiga S8 arq.; tabelas brancas e texto preto; quatro tabelas químicas 5–8 em A3 horizontal, conforme a seção do DOCX. Demais páginas usam A4 vertical. O sumário inclui Introdução e Medidas de Mitigação, alinhado ao corpo. Páginas do sumário e rodapés são atualizadas após paginar; o HTML salvo e o PDF usam a mesma distribuição.

A numeração das figuras foi corrigida para eliminar a duplicidade da Figura 4, sem alterar os códigos dos campos: 1 mapa, 2 amostrador, 3 transporte, 4 inconformidades, 5 granulometria, 6 parâmetros químicos e 7 mortalidade. As referências a essas figuras no corpo e as legendas foram atualizadas em conjunto.

O conteúdo técnico e as referências foram preservados. As divergências da origem estão registradas no contrato: amônia ionizada em um parágrafo de interpretação versus não ionizada na metodologia; ano 2022 na referência CONAMA 454 versus 2012 no corpo; referência Almeida/Rocha repetida e referência ASTM sem endereço. Não houve revisão técnica ou normativa, classificação automática ou substituição silenciosa dos textos.

Campos estruturados:

| Campo | Formato |
| --- | --- |
| `QSD_17` | Endereço do mapa dos pontos |
| `QSD_18` | Lista: `PONTO`, `UTM_E`, `UTM_N`, `CLASSE` |
| `QSD_19` | Lista: `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `QSD_20` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `QSD_21`, `QSD_25` | Endereços das imagens de amostrador e transporte |
| `QSD_28` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `QSD_60` | Endereço do gráfico geral de inconformidades, Figura 4 |
| `QSD_68` | Endereço do gráfico de granulometria das campanhas, Figura 5 |
| `QSD_69` | Matriz de percentuais de inconformidade: metais e arsênio |
| `QSD_70` | Matriz de percentuais de inconformidade: pesticidas organoclorados, TBT e PCBs |
| `QSD_71` | Matriz de percentuais de inconformidade: HPAs |
| `QSD_72` | Matriz de percentuais de inconformidade: nutrientes e COT |
| `QSD_73` | Lista de gráficos químicos: `TITULO`, `URL`, Figura 6 |
| `QSD_74` | Endereço do gráfico de mortalidade, Figura 7 |
| `QSD_75` | Matriz de toxicidade por ponto e campanha |
| `QSD_47` | Lista: `INDICADOR`, `META`, `RESULTADO` |
| `QSD_48`, `QSD_49` | Parecer e medidas, como texto com quebras de linha preservadas |
| `QSD_50` | Lista de anexos: `TITULO` obrigatório; `TEXTO`, `IMAGEM`, `URL` opcionais; lista vazia mostra “Sem anexos” |

As matrizes químicas seguem este formato:

```json
{
  "PARAMETROS": [{ "NOME": "[PARÂMETRO]" }],
  "LINHAS": [{ "CAMPANHA": "C1", "VALORES": ["[%]"] }]
}
```

Cada linha deve ter um valor por parâmetro, na mesma ordem. Informe percentuais já formatados, incluindo `%` quando necessário. Use um marcador explícito para ausência, sem confundir com zero. Essas quatro tabelas não têm linha de unidade, média ou VMP, pois expressam percentuais de inconformidade por campanha. Cabeçalhos e legendas se repetem nas continuações. O exemplo preserva os nomes dos 8, 13, 14 e 3 parâmetros dos quatro grupos, respectivamente.

A matriz de toxicidade usa campanhas nas colunas:

```json
{
  "CAMPANHAS": [{ "NOME": "C1" }, { "NOME": "C2" }],
  "LINHAS": [
    { "PONTO": "[PONTO]", "VALORES": ["[TOXICIDADE]", "[TOXICIDADE]"] }
  ]
}
```

Cada linha precisa de uma classificação por campanha, na ordem de `CAMPANHAS`. O exemplo demonstra C1–C4. Os valores são textos recebidos dos dados, sem interpretação automática do gerador. Matrizes muito mais largas podem exigir adaptação da largura ou divisão das colunas.

Os demais campos são textos simples com contexto no contrato. `QSD_52`/`QSD_53` são mês/ano inicial e `QSD_55`/`QSD_56` mês/ano final. `QSD_57`/`QSD_58` identificam mês/ano da campanha com mais inconformidades; `QSD_59` completa a frase sobre realização de dragagem. `QSD_04` permanece como ano nas fontes. `QSD_61`–`QSD_67` são os percentuais médios das sete frações granulométricas, e `QSD_76` é o percentual de pontos com sedimentos classificados como tóxicos; informe esses campos sem `%`, pois o texto já inclui o símbolo.

`QSD_22`, `QSD_24` e `QSD_23` descrevem amostrador, tipo de amostra e transporte. `QSD_26` contém procedimentos analíticos e `QSD_27` o organismo-teste. Médias, percentuais, classificação, gráficos e pareceres devem ser fornecidos pelo responsável; o gerador não executa cálculos científicos.

Textos usam escape padrão Mustache; não envie HTML arbitrário. Imagens são ajustadas proporcionalmente. Links de anexos aparecem como links e não incorporam páginas de PDF externo. Textos longos podem continuar por páginas; blocos indivisíveis maiores que a área útil são rejeitados.

Validação reproduzível:

```sh
node scripts/validate-sediments-multi-report.js
```

Usa o DOCX de origem e saídas separadas em `.report-to-mustache/sedimentos-multiplas-stress/`. O teste inclui 150 linhas em cada matriz química, 150 pontos na matriz de toxicidade, 110 parâmetros na metodologia e parecer de 1.280 palavras com 16 quebras de parágrafo. Produziu 53 páginas com 31 continuações, preservando textos, quebras e linhas de tabela. Confere conteúdo do DOCX com as correções de numeração documentadas, ordem das células de toxicidade, contagem e dimensões reais das páginas do PDF, limites de conteúdo e rejeição de matriz inconsistente ou imagem obrigatória ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
