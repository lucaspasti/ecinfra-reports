# 2. MonitoramentoEfluentes_MultiplasCampanhas.docx

Template individual convertido do DOCX. Prévia com marcadores; não constitui relatório preenchido.

```powershell
node scripts/render-document-report.js templates/QEF_campanhas_multiplas
node scripts/export-report.js .report-to-mustache/previews-QEF_campanhas_multiplas relatorio.html
```

Passe JSON e pasta de saída opcionais ao renderizador. Imagens usam URL; mantenha resultados em tabelas estruturadas e preencha os campos TEXTO dos códigos com múltiplas funções. Consulte contrato.json para caminhos de cada ocorrência. CABECALHO se repete nas continuações; RODAPE aparece somente no final. Os formatos de papel seguem cada seção da fonte.

- Figura 006: campo adicional FIGURAS.FIGURA_006.URL; legenda sem marcador exclusivo no DOCX.
- Fonte contém números de tabela repetidos. Legendas e referências textuais preservadas para revisão técnica, sem correção ambígua automática.

Recursos centralizados: [assets compartilhados](../_shared/README.md). O manifesto mantém o nome original de cada recurso; `ASSET_BASE` aponta à pasta compartilhada.

Imagens extraídas são somente exemplos e não são usadas no relatório. Forneça as imagens reais por URL no JSON. `LOGOTIPO_URL` e `TIMBRADO_URL` são opcionais; sem URL, nenhuma imagem de identidade visual é exibida.
