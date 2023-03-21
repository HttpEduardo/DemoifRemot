# DemoifRemot

Este repositório contém duas demonstrações de IF relacionadas (mas separadas). Vou descrevê-las separadamente.

-----------------------------------------------------------------
Remote-IF: Demonstra a conexão de um interpretador IF no servidor (usando RemGlk) com uma biblioteca de exibição IF no cliente (GlkOte).

Escrito por Andrew Plotkin. O script remote-if.py está em domínio público.

Este script inicia um servidor web Tornado que pode gerenciar um jogo IF como um processo filho. O cliente da web se conecta ao jogo via biblioteca GlkOte (incluída).

Para testar isso …

Compile o Glulxe com o RemGlk. Use a versão mais recente do GitHub do RemGlk:    https://github.com/erkyrath/remglk
Baixe a versão Glulx de Colossal Cave:    http://ifarchive.org/if-archive/games/glulx/advent.ulx
Digite:    python3 remote-if.py --debug --command=‘glulxe advent.ulx’
Em seu navegador, visite:    http://localhost:4000/
Pressione “log in”, depois “play the game”.
A conexão entre GlkOte e o servidor é tratada com solicitações AJAX. Se você adicionar “–connect=ws”, ele usará solicitações Websocket. Ambos funcionam; Eu só queria fornecer código de amostra para ambos.

Para experimentar o depurador experimental Glulxe, compile o Glulxe com a opção VM_DEBUGGER. Em seguida, invoque-o assim:    python3 remote-if.py --debug --gidebug --command=‘glulxe -D advent.ulx’

(A opção --gidebug ativa o console de depuração HTML; a opção -D para o Glulxe ativa a função de depurador de back-end.)

Para experimentar um jogo com gráficos …

Baixe Sensory Jam:    http://eblong.com/zarf/glulx/sensory.blb
Também baixe o script Python BlorbTool:    http://eblong.com/zarf/blorb/blorbtool.py
Crie um subdiretório de recursos no diretório estático:    mkdir static/resource
Execute o BlorbTool para descompactar os dados de imagem no diretório de recursos:    python blorbtool.py sensory.blb giload static/resource    (isso criará os arquivos static/resource/pict-0.jpeg, etc.)
Digite:    python3 remote-if.py --debug --command=‘glulxer -ru http://localhost:4000/static/resource/ sensory.blb’
O argumento -ru diz à biblioteca de exibição para buscar imagens usando URLs do formato http://localhost:4000/static/resource/pict-0.jpeg. Consulte a documentação do RemGlk: http://eblong.com/zarf/glk/remglk/docs.html

(Você pode pensar que pode usar o argumento -rd para servir os arquivos diretamente do sistema de arquivos local. Isso não funciona, porque os navegadores modernos não permitirão que uma página da web http: carregue URLs de imagem de arquivo:)

Esta é uma demonstração, não uma solução pronta para produção.

Isso não tem um componente de banco de dados, nem qualquer outro tipo de persistência.   As instâncias do jogo IF são executadas como processos filhos do servidor. Se o servidor
