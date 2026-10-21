# 3. Bentos.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/BEN_bentos
node scripts/export-report.js .report-to-mustache/previews-BEN_bentos relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.



Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
