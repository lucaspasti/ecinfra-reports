---
name: docx-report-template
description: "Converter relatórios DOCX em um template HTML Mustache único para geração automatizada e PDF, com fidelidade ao documento, tabelas iteráveis e validação visual. Use também para continuar ou ajustar um template de relatório existente."
---

Crie relatórios HTML destinados à impressão e ao preenchimento automático a partir do DOCX fornecido. Preserve o conteúdo técnico e transforme os marcadores em um contrato de dados explícito.

## Descobrir o documento e o projeto

- Localize o DOCX solicitado e confira as dependências, geradores, templates e prévias existentes antes de criar novos arquivos. Se houver vários documentos plausíveis, resolva qual é o alvo.
- Leia o texto completo, tabelas, estilos, seções, tamanho do papel, margens, cabeçalhos, rodapés e relações das imagens. Não deduza a paginação apenas pelo texto ou pelo sumário.
- Comentários de revisão não pertencem ao relatório renderizado. Consulte-os somente se o usuário pedir; mantenha suas anotações fora do HTML e do PDF.
- O helper [scripts/extract-docx.cjs](scripts/extract-docx.cjs) extrai o conteúdo e os recursos sem Word. Usa `jszip` e `@xmldom/xmldom` do projeto atual. Execute `node <caminho-da-skill>/scripts/extract-docx.cjs <arquivo.docx> <pasta-de-extração>` a partir da raiz do projeto. Leia [references/implementation.md](references/implementation.md) para detalhes de OOXML e geração do PDF.

## Estabelecer e manter as decisões

No início de um novo trabalho, esclareça apenas escolhas que afetem o resultado: fidelidade ou redesign e sintaxe dos dados, quando ainda não definidas. Progrida na inspeção enquanto aguarda respostas opcionais.

Para os relatórios deste usuário, adote estes padrões, salvo instrução diferente:

- Reproduza o DOCX com fidelidade, com as correções já aprovadas pelo usuário.
- Entregue **um único arquivo HTML Mustache editável**, contendo todas as partes e estilos. A validação visual pode ser separada por página; os templates de cada página não devem virar fontes independentes.
- Todas as tabelas têm fundo branco e texto preto, inclusive cabeçalhos. Preserve bordas e hierarquia de títulos adequadas ao original.
- Imagens são fornecidas por endereço; tabelas são listas/objetos iterados pelo Mustache. Preserve os identificadores originais dos marcadores.
- Use o formato do papel de cada seção do DOCX. Tabelas largas podem exigir A3 horizontal; não aplique esse tamanho a todos os documentos.
- Reutilize o timbrado e os logotipos do arquivo de origem, inspecionando-os visualmente. No modelo EC infra já aprovado, omita a marca antiga “S8 arq.”. Essa remoção não deve se tornar uma regra para outras marcas ou documentos.

Mantenha um registro curto dessas decisões junto ao contrato dos dados. Não peça de novo o que já foi decidido. Se o usuário autorizar concluir o restante por conta própria, siga as decisões aceitas e entregue o conjunto completo sem novas aprovações intermediárias.

## Construir o template e os dados

- Preserve títulos, ordem, referências, legendas e campos variáveis. Não invente resultados laboratoriais, percentuais, pareceres ou mapas.
- Transforme `[CODIGO_01]` em `{{CODIGO_01}}`, mantendo a rastreabilidade entre DOCX e HTML. Variáveis podem estar divididas entre vários runs do Word: una o texto do parágrafo antes de detectá-las.
- Defina listas para linhas de tabelas, matrizes para resultados e URLs para imagens. Não substitua tabelas estruturadas por strings de HTML pronto sem pedido do usuário.
- Crie um JSON de exemplo e documente os nomes dos campos internos. Para matrizes, valide que cada linha tem um valor para cada parâmetro na mesma ordem.
- Use o escape padrão do Mustache. Para textos longos, preserve quebras de linha sem aceitar HTML arbitrário. Os cálculos científicos devem vir dos dados ou de uma rotina explicitamente implementada e verificada.
- Separe marcadores da prévia dos dados reais. Espaços reservados de imagens e gráficos são úteis para aprovação do layout, mas devem dar lugar às imagens informadas na geração preenchida.
- Listas de gráficos podem conter título e endereço. Anexos podem conter título, texto, imagem ou link; um link a PDF não equivale a incorporar suas páginas.
- Use títulos coerentes entre corpo e sumário. Correções de numeração propagam-se às subseções e ao sumário. Não transplante a numeração de outro modelo.

## Validar com o usuário

Quando solicitado trabalho página por página:

1. Implemente a próxima página dentro do HTML único.
2. Gere a prévia HTML, uma imagem legível da página e o PDF acumulado.
3. Inspecione a imagem antes de apresentá-la. Confira imagens, cortes, tabelas, espaçamentos, cabeçalhos e rodapés.
4. Entregue links locais e uma pergunta visual específica, sem repetir escolhas já aprovadas.
5. Aguarde a validação dessa página antes de avançar. Se o usuário pedir o restante de uma vez, conclua todas as seções.

Uma página pode começar com a continuação da tabela anterior. Repita o cabeçalho, preserve a ordem das linhas e mantenha resumos no final da tabela; não recomece a seção só porque mudou a página.

## Gerar e conferir o PDF

- Mantenha comandos reproduzíveis para renderizar o Mustache e exportar o PDF, reaproveitando as ferramentas do projeto.
- Gere com Chromium/Edge/Chrome e CSS de impressão, respeitando tamanhos mistos. Aguarde fontes e imagens; informe erro para imagens indisponíveis em vez de emitir silenciosamente um PDF incompleto.
- Não confunda uma caixa CSS de altura fixa com paginação: divida conteúdo excedente e repita cabeçalhos. Evite títulos isolados e impeça cortes silenciosos. Blocos indivisíveis maiores que uma página devem receber ajuste de layout ou erro claro.
- Atualize a numeração e o sumário **após** paginar. O HTML final de validação e o PDF devem representar a mesma distribuição.
- Verifique todas as seções e todos os identificadores do DOCX, além de legendas, fontes, referências e campos estruturados.
- Confirme a contagem real de páginas e as orientações no PDF. Inspecione visualmente páginas representativas, especialmente tabelas largas, gráficos, parecer e referências.
- Se implementar ou alterar a paginação, teste volume maior de linhas e texto. Compare o conteúdo antes/depois para provar que não houve perda ou duplicação, além de verificar cortes.
- Ao gerar outra versão de validação ou teste, use saída separada. Não substitua a versão aprovada sem o usuário pedir. Um texto de exemplo “enxuto” deve aparecer poucas vezes, sem preenchimento repetitivo.

Entregue o PDF completo, o template HTML único, o exemplo de dados e instruções curtas de geração. Distinga claramente um PDF de validação com marcadores de um relatório preenchido com dados reais.
