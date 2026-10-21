# Extração e implementação

## Ler OOXML sem perder a estrutura

DOCX é um ZIP. `word/document.xml` contém os parágrafos e as tabelas; os
comentários ficam em arquivos próprios. Texto de tabela também está em `w:p`:
concatenar indiscriminadamente todos os parágrafos é útil para leitura, mas
perde linhas, células e mesclagens. Mantenha também uma representação estrutural.

O helper extrai:

- `document.json`: blocos em ordem, estilos básicos, seções, referências a
  cabeçalhos/rodapés, relações e marcadores encontrados.
- `assets/`: recursos de `word/media`, sem alteração da imagem original.
- `document.xml`, `styles.xml` e `theme.xml`, quando disponíveis: suporte à
  inspeção de detalhes que o resumo não resolve.

Não use contagem de parágrafos vazios como sistema de layout. As posições do
Word podem depender de estilos herdados e âncoras de desenhos. `w:sectPr`
define a seção terminada naquele parágrafo; o último `w:sectPr` define a
seção final. `w:lastRenderedPageBreak` é uma pista da última renderização,
não uma garantia de paginação atual.

Tamanhos e margens em `w:pgSz` e `w:pgMar` usam twips:
`mm = twips × 25,4 / 1440`. Desenhos usam EMU: `mm = EMU / 36000`.
Consulte o tema quando a fonte do estilo for referenciada por nome de tema.

As imagens de timbrado podem incluir marcas antigas e grandes áreas brancas;
o Word pode sobrepor logotipos e retângulos. Abra as imagens antes de copiá-las
ao layout. Uma máscara CSS pode reproduzir a sobreposição aprovada sem editar
o bitmap. Evite esticar uma imagem com logotipos ao mudar a orientação;
prefira componentes separados quando o original permitir.

## Contratos de dados reutilizáveis

Exemplos de formatos; adapte nomes internos ao projeto e preserve os códigos
originais do documento:

```json
{
  "MAPA": "https://servidor.exemplo/mapa.png",
  "PONTOS": [
    { "PONTO": "P01", "UTM_E": "000000", "UTM_N": "0000000", "CLASSE": "[CLASSE]" }
  ],
  "RESULTADOS": {
    "PARAMETROS": [
      { "NOME": "[PARÂMETRO]", "MEDIA": "[MÉDIA]", "VMP": "[VMP]", "UNIDADE": "[UNIDADE]" }
    ],
    "LINHAS": [
      { "PONTO": "P01", "PROFUNDIDADE": "Superfície", "VALORES": ["[VALOR]"] }
    ]
  },
  "GRAFICOS": [{ "TITULO": "[PARÂMETRO]", "URL": "" }]
}
```

Uma lista é iterada com `{{#PONTOS}}...{{/PONTOS}}`; valores textuais usam
`{{CAMPO}}`. Para matriz, itere os parâmetros no cabeçalho e os valores na
linha; mantenha a ordem consistente. Documente como representar resultados
abaixo do limite de quantificação e células sem resultado, quando aplicável.

Não embuta os dados de exemplo no template final. O renderizador recebe o
JSON e deriva apenas os campos necessários à apresentação, como páginas de
continuação. Documente que gráficos devem chegar como imagens quando o
escopo não inclui calculá-los e desenhá-los.

## Paginação de impressão

Use unidades físicas e `@page` nomeado para misturar A4 vertical e outros
formatos reais do DOCX. `preferCSSPageSize: true` respeita essas regras.
Defina a área útil de conteúdo separada do cabeçalho e do rodapé.

Ao paginar tabelas, mantenha cabeçalho e legenda nas continuações, mova linhas
inteiras e deixe `tfoot` com média/VMP/unidade na última parte. Não fixe
quantidades como “16 linhas” como solução universal: alturas dependem dos
dados, da fonte e da largura. Meça a renderização e confira os limites da
área útil. Se houver divisão inicial específica ao modelo, documente-a e
trate excessos adicionais.

Para blocos de texto, preserve a sequência e as quebras de parágrafo ao
dividir. Para anexos compostos, divida seus blocos internos se necessário.
Imagens devem caber proporcionalmente; não cortar mapas para preencher caixas.

Associe cada entrada do sumário a um título estável. Calcule suas páginas
depois de inserir as continuações. Regere a numeração dos rodapés também.
Confirme a contagem de páginas no PDF, não apenas o número de elementos HTML.

## Ambiente Windows

Se o alias `python` não funcionar, não dependa de conversão por Python:
use Node com as dependências do projeto ou `System.IO.Compression` e XML do
PowerShell para leitura. Se `npm.ps1` for bloqueado pela política do shell,
use `npm.cmd`, sem alterar a política da máquina. Navegadores instalados
podem ser usados pelo `playwright-core`; aceite um caminho configurável e
não dependa de um diretório de usuário fixo.

Preserve arquivos aprovados quando fizer testes de paginação. Use uma pasta
separada para entradas e saídas de teste e outra para variantes de validação.
