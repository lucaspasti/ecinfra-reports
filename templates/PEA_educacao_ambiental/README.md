# 1. EducaçaoAmbiental.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/PEA_educacao_ambiental
node scripts/export-report.js .report-to-mustache/previews-PEA_educacao_ambiental relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.

- Figura 003: campo adicional FIGURAS.FIGURA_003.URL; legenda sem marcador exclusivo no DOCX.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
