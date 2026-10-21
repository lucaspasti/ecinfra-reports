# 2. MonitoramentodeRuidosSubAquaticos.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/RSA_ruidos_subaquaticos
node scripts/export-report.js .report-to-mustache/previews-RSA_ruidos_subaquaticos relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.

- Marcador [RSA _09] usa a chave normalizada RSA_09; espaços originais preservados na prévia.
- Marcador [RSA _10] usa a chave normalizada RSA_10; espaços originais preservados na prévia.
- Marcador [RSA _12] usa a chave normalizada RSA_12; espaços originais preservados na prévia.
- Marcador [RSA _13] usa a chave normalizada RSA_13; espaços originais preservados na prévia.
- Marcador [RSA _46] usa a chave normalizada RSA_46; espaços originais preservados na prévia.
- Figura 001: campo adicional FIGURAS.FIGURA_001.URL; legenda sem marcador exclusivo no DOCX.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
