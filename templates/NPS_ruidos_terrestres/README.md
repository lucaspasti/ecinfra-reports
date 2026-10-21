# 1. MonitoramentodeRuidosTerrestres.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/NPS_ruidos_terrestres
node scripts/export-report.js .report-to-mustache/previews-NPS_ruidos_terrestres relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.

- Marcador [NPS _09] usa a chave normalizada NPS_09; espaços originais preservados na prévia.
- Marcador [NPS _10] usa a chave normalizada NPS_10; espaços originais preservados na prévia.
- Marcador [NPS _12] usa a chave normalizada NPS_12; espaços originais preservados na prévia.
- Marcador [NPS _13] usa a chave normalizada NPS_13; espaços originais preservados na prévia.
- A fonte contém instruções editoriais ou lacunas no corpo; conferir antes de emissão técnica.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
