O template editável único é `relatorio.html`, convertido de `reports/5. QualidadeAguaSuperficial_CampanhaUnitaria_P.docx`. Todas as partes e estilos estão nesse HTML. `../_shared/assets/` guarda os 11 arquivos originais sem alteração; as imagens são referenciadas por endereço. `contrato.json` distingue os três marcadores originais dos campos complementares criados para estruturar os dados.

Para gerar a prévia completa:

```sh
npm.cmd run report:p:build
```

Saídas em `.report-to-mustache/previews-superficial-unitaria-p/`: HTML paginado, `relatorio.pdf`, imagens de páginas e `validacao.json`. Esta prévia tem 23 páginas: 21 A4 verticais e 2 A3 horizontais, incluindo a continuação da tabela de resultados. O sumário e os rodapés são atualizados depois da paginação; o HTML salvo corresponde ao PDF.

Esta variante já está parcialmente preenchida no DOCX, com informações do Porto de São Francisco do Sul, março de 2025, coordenadas, laboratório, médias e nove gráficos. Esses dados foram copiados, sem recálculo ou validação científica. A prévia ainda contém pendências: mapa `QAG_17`, conformidade `QAG_31` e resultados `QAG_47`. Os valores individuais da Tabela 5 estão vazios na origem. Parecer e medidas também estão vazios. Não é um relatório técnico completo para emissão.

Para gerar com dados preenchidos, use outra pasta de saída:

```sh
node scripts/render-p-report.js caminho/dados.json saida-p
node scripts/export-report.js saida-p "5. QualidadeAguaSuperficial_CampanhaUnitaria_P.html"
```

O exportador usa Edge/Chrome instalado; `REPORT_BROWSER_PATH` permite indicar outro executável. Aguarda fontes e imagens e interrompe a exportação se alguma imagem estiver indisponível. Na geração com JSON fornecido, informe um mapa em `QAG_17` e as três fotos; gráficos presentes precisam de URL. Os demais campos pendentes precisam ser preenchidos pelo responsável antes da emissão.

Nos dados, endereços iniciados por `assets/` são resolvidos na pasta compartilhada `templates/_shared/assets/`. Aceitam o nome original registrado no manifesto ou o nome com hash usado no exemplo. Outros endereços relativos são resolvidos em relação ao HTML gerado. Também podem ser usadas URLs completas. A URL do logotipo é derivada de `ASSET_BASE` pelo renderizador.

Decisões de fidelidade:

- Mantida a marca original EC Projetos desta variante, sem importar a marca EC infra ou a faixa gráfica dos outros modelos. A imagem possui margens brancas; uma janela CSS enquadra o logotipo sem modificar o bitmap.
- Capa e páginas pares ficam sem cabeçalho e rodapé, conforme as referências de primeira página e páginas pares do DOCX. Páginas ímpares internas usam logotipo e rodapé; a paridade é aplicada após paginar.
- Preservados A4 vertical e A3 horizontal da seção da Tabela 5. Os gráficos ficam em A4 e continuam por páginas conforme a altura medida.
- Tabelas têm fundo branco e texto preto. As linhas são listas/objetos, sem HTML pronto nos dados.
- O sumário foi reconstruído a partir do corpo: inclui Introdução e Referências, remove as entradas de metais e HPA ausentes do corpo e renumera Indicadores de Desempenho para 7.2.
- A foto das figuras 2, 3 e 4 é a mesma no DOCX: os três desenhos apontam a `rId15`. Foi mantida, com campos separados para eventual substituição.
- Os nove gráficos são os bitmaps originais, sem redesenho. Alguns já têm partes cortadas no próprio arquivo de imagem; o template mantém o bitmap inteiro, sem acrescentar recorte.
- Normalizada a duplicação “Fonte: Fonte:”. Na Tabela 5, a linha de unidades foi alinhada às colunas DBO/pH/OD, mantendo `mg/L`, `-`, `mg/L`.

Campos originais:

| Campo | Formato |
| --- | --- |
| `QAG_17` | Endereço da imagem do mapa; vazio na prévia sem argumentos |
| `QAG_31` | Texto/número do percentual de conformidade, sem `%` porque o corpo já contém o símbolo |
| `QAG_47` | Lista de objetos com `INDICADOR`, `META`, `RESULTADO` |

Campos complementares:

| Campo | Formato |
| --- | --- |
| `TABELA_01` | Lista: `PONTO`, `UTM_E`, `UTM_N`, `CLASSE` |
| `TABELA_02` | Lista: `GRUPO`, `PARAMETRO`, `PERIODICIDADE` |
| `LABORATORIO` | Objeto: `NOME_FANTASIA`, `RAZAO_SOCIAL`, `CNPJ`, `ENDERECO`, `RESPONSAVEL`, `EMAIL`, `CONTATO` |
| `TABELA_04` | Lista: `TIPO`, `INDICADOR`, `UNIDADE`, `META` |
| `TABELA_05` | Matriz de resultados: `PARAMETROS` e `LINHAS` |
| `TABELA_06` | Lista: `PARAMETRO`, `TOTAL`, `SUPERFICIE`, `MEIO`, `FUNDO` |
| `FOTO_SONDA`, `FOTO_AMOSTRADOR`, `FOTO_TRANSPORTE` | Endereços das fotografias |
| `GRAFICOS` | Lista de objetos com `TITULO`, `URL`, na ordem desejada |
| `PARECER`, `MEDIDAS` | Texto opcional, com quebras de linha preservadas, inclusive nas continuações |
| `ANEXOS` | Lista: `TITULO`, e opcionais `TEXTO`, `IMAGEM`, `URL` |

A matriz da Tabela 5 segue este formato:

```json
{
  "PARAMETROS": [{ "NOME": "DBO", "UNIDADE": "mg/L" }],
  "LINHAS": [
    { "PONTO": "P01", "PROFUNDIDADE": "Superfície", "VALORES": [""] }
  ]
}
```

Cada linha deve ter um valor por parâmetro, na mesma ordem. O exemplo contém 69 linhas, com P01–P23 e três profundidades; não inventa resultados nas células vazias. O ponto é repetido nas linhas em vez de depender de células verticalmente mescladas, permitindo continuações sem perder a identificação. Cabeçalhos e legendas se repetem; a unidade aparece somente na última parte da tabela. Resultados abaixo de quantificação podem ser textos como `<LQ`; use um marcador explícito para ausências e não confunda com zero.

Os nomes PSFS/Controle da Tabela 1 e P01–P23 da Tabela 5 foram mantidos como estão no DOCX, sem presumir correspondência. Também foram preservadas as periodicidades: o texto é mensal e a Tabela 2 informa trimestral. Os valores da Tabela 6 e dos gráficos não foram recalculados nem conciliados com as células vazias da Tabela 5. Os textos já preenchidos de capa, identificação e narrativa permanecem no HTML; os campos complementares abrangem tabelas, fotos, gráficos, anexos, parecer e medidas.

O anexo é um link assinado copiado do documento para um arquivo identificado como `FICTICIO.pdf`. Não foi acessado, baixado ou incorporado. Atualize esse endereço quando necessário. Um link não equivale a incorporar páginas de um PDF. Imagens de anexos são ajustadas proporcionalmente; blocos indivisíveis maiores que uma página são rejeitados.

Os textos usam escape padrão Mustache. Não envie HTML nos campos. Médias, percentuais, limites, interpretações e gráficos devem chegar prontos nos dados; o gerador não executa cálculos científicos.

Validação reproduzível:

```sh
node scripts/validate-p-report.js
```

Gera saídas separadas em `.report-to-mustache/superficial-p-stress/`. O teste usa 100 pontos na Tabela 1, 120 linhas na Tabela 2, 210 linhas na matriz e parecer de 1.280 palavras com 16 quebras de parágrafo. Produziu 37 páginas, com 22 continuações. Verifica texto do DOCX, ordem e conteúdo das linhas, preservação das quebras, unidade apenas no final da tabela, nove gráficos, hashes dos 11 arquivos de imagem, contagem e dimensões reais das páginas do PDF e limites da área útil. Também confirma rejeição de matriz inconsistente e mapa obrigatório ausente.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
