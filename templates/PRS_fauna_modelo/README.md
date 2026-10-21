# 0. Fauna Modelo.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/PRS_fauna_modelo
node scripts/export-report.js .report-to-mustache/previews-PRS_fauna_modelo relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.

- Fonte incompleta: título XXXX, objetivos de resíduos e marcador QAR_01; seções finais vazias. Preservados sem inventar conteúdo.
- A fonte contém instruções editoriais ou lacunas no corpo; conferir antes de emissão técnica.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Logotipo e timbrado são fixos e compartilhados. Fotos, mapas e gráficos de exemplo não são usados automaticamente; forneça as imagens reais por URL.
