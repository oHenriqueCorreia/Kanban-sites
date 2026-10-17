# Web Prospectador de Sites

Kanban local para importar bases de prospecção de diferentes nichos, qualificar cada negócio e acompanhar a abordagem comercial.

## Abrir

Use Node.js 24 ou superior. Execute `npm ci` e `npm run dev` nesta pasta e abra a URL exibida. Os arquivos publicados ficam em `dist/`; o site não exige servidor de aplicação nem serviços externos.

## Movimentar os cards

- **Computador:** arraste o card ou sua alça ⠿ até outra etapa ou posição da mesma coluna.
- **Celular:** arraste pela alça ⠿. O restante do card permite rolar a página normalmente.
- **Teclado:** use Tab para focar a alça, Espaço ou Enter para pegar, setas para escolher etapa/posição e Espaço ou Enter para soltar. Escape cancela.

O quadro rola automaticamente ao aproximar o card das bordas. A etapa e a ordem só são atualizadas após a gravação no navegador; se a gravação falhar, a posição anterior é mantida. Mover para a etapa “Sem site” não confirma a ausência de um site: confirme essa informação na ficha. Os filtros continuam ativos ao mover um card e podem ocultá-lo na nova etapa.

## Atualizar as bases

1. Abra **Importar bases** e escolha um arquivo `.csv`, `.tsv`, `.xlsx` ou `.json` de até 20 MB.
2. Se houver mais de uma aba, selecione a correta. Indique a linha dos títulos e confira o mapeamento das colunas.
3. Informe o nicho, a menos que a planilha tenha uma coluna de nicho preenchida.
4. Clique em **Importar leads**. Novos registros são adicionados; registros reconhecidos são atualizados, mantendo etapa, observações e próxima ação.

O arquivo `dist/modelo-importacao.csv` traz um exemplo de cabeçalhos. O painel lê `.xlsx` em navegadores com `DecompressionStream('deflate-raw')`; se o navegador não oferecer esse recurso, salve a planilha como CSV.

## Filtros e dados

Use nicho + **Sem site confirmado** + **Com celular** para encontrar o segmento desejado. O atalho **Advocacia · sem site · celular** aplica esses três filtros. O tipo celular vem da coluna própria ou é inferido do formato brasileiro do número; não comprova WhatsApp. Célula de site vazia não prova ausência de site.

A base inicial tem 1.000 registros do Google Maps: 978 triados como advocacia e 22 em revisão. A análise detalhada está em `ANALISE.md`.

As alterações são salvas no armazenamento do navegador. Use **Exportar leads em CSV** para obter a base atualizada e **Backup completo** para preservar as fichas e importações. O backup JSON pode ser restaurado em outro navegador.

Os dados não são sincronizados entre dispositivos ou endereços diferentes. Antes de trocar de endereço (por exemplo, do arquivo local para o Netlify), exporte um backup e restaure no novo endereço. A ordem do Kanban também é preservada no backup completo.

## Verificar o código e os dados

Execute `npm run check` para conferir importação, normalização, atualização, ordem do Kanban e sintaxe. Execute `npm run check:browser` para os testes de interação e responsividade. O teste de navegador usa Chrome/Chromium disponível na máquina ou o executável indicado em `BROWSER_EXECUTABLE_PATH`. O arquivo de origem inicial está em `data/`.

## Publicar no Netlify

O arquivo `netlify.toml` publica apenas `dist/`, executa as verificações antes do deploy e define os cabeçalhos de segurança. A pasta `.netlify/` fica ignorada no Git, assim como credenciais e resultados de teste.

Após `npm run check` e `npm run check:browser` passarem, conecte o repositório ao Netlify para publicação a partir de `main`, ou publique com `npx netlify-cli deploy --prod --dir=dist`. Em um site conectado ao Git, o próximo push para a branch de produção publica a nova versão e substitui um deploy manual anterior.
