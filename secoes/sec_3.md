# 3. Abrindo um terminal

Em um sistema Ubuntu 18.04, você pode encontrar um lançador para o terminal clicando no item Atividades no canto superior esquerdo da tela e digitando as primeiras letras de “terminal”, “comando”, “prompt” ou “shell”. Sim, os desenvolvedores configuraram o lançador com todos os sinônimos mais comuns, então você não deve ter problemas para encontrá-lo.

## Lançador de terminal no Ubuntu 18.04

Outras versões do Linux, ou outras variantes do Ubuntu, geralmente terão um lançador de terminal localizado no mesmo lugar que seus outros lançadores de aplicativos. Pode estar escondido em um submenu ou você pode ter que procurá-lo dentro do seu lançador, mas provavelmente estará lá em algum lugar.

Se você não conseguir encontrar um lançador, ou se quiser uma maneira mais rápida de abrir o terminal, a maioria dos sistemas Linux usa o mesmo atalho de teclado padrão para iniciá-lo: Ctrl-Alt-T.

Independentemente de como você lançar seu terminal, você deve acabar com uma janela de aparência um tanto monótona com um texto estranho no topo, muito parecido com a imagem abaixo. Dependendo do seu sistema Linux, as cores podem não ser as mesmas, e o texto provavelmente dirá algo diferente, mas o layout geral de uma janela com uma grande área de texto (principalmente vazia) deve ser semelhante.

## Uma nova janela de terminal no Ubuntu 18.04

Vamos executar nosso primeiro comando. Clique com o mouse na janela para garantir que é lá que suas teclas serão registradas, depois digite o seguinte comando, todo em minúsculas, antes de pressionar a tecla Enter ou Return para executá-lo.

```sh
pwd
```

Você deve ver um caminho de diretório impresso (provavelmente algo como `/home/SEU_USUARIO`), seguido por outra cópia daquele texto estranho.

## Resultado da execução do comando pwd

Há alguns conceitos básicos a entender aqui, antes de entrarmos nos detalhes do que o comando realmente fez. Primeiro, quando você digita um comando, ele aparece na mesma linha que o texto estranho. Esse texto está lá para dizer que o computador está pronto para aceitar um comando, é a maneira do computador de solicitar um comando. Na verdade, geralmente é chamado de prompt, e você pode às vezes ver instruções que dizem “traga um prompt”, “abra um prompt de comando”, “no prompt do bash” ou similar. Todos são apenas maneiras diferentes de pedir para você abrir um terminal para acessar um shell.

Sobre sinônimos, outra maneira de olhar para o prompt é dizer que há uma linha no terminal na qual você digita comandos. Uma linha de comando, se preferir. Novamente, se você vir menção de “linha de comando”, incluindo no título deste próprio tutorial, é apenas outra maneira de falar sobre um shell rodando em um terminal.

A segunda coisa a entender é que, quando você executa um comando, qualquer saída que ele produza geralmente será impressa diretamente no terminal, e então você verá outro prompt assim que ele terminar. Alguns comandos podem gerar muito texto, outros operarão silenciosamente e não gerarão nada. Não se assuste se você executar um comando e outro prompt aparecer imediatamente, pois isso geralmente significa que o comando foi bem-sucedido. Se você pensar nas conexões de rede lentas dos terminais dos anos 1970, aqueles primeiros programadores decidiram que, se tudo corresse bem, eles poderiam economizar alguns preciosos bytes de transferência de dados não dizendo nada.

## A importância da diferenciação entre maiúsculas e minúsculas

Tenha muito cuidado com a diferenciação entre maiúsculas e minúsculas ao digitar na linha de comando. Digitar `PWD` em vez de `pwd` produzirá um erro, mas às vezes a diferenciação errada pode resultar em um comando que parece estar funcionando, mas não faz o que você esperava. Vamos olhar mais sobre a diferenciação na próxima página, mas, por enquanto, apenas certifique-se de digitar todas as linhas seguintes exatamente como mostrado.

## Um senso de localização

Agora, sobre o comando em si. `pwd` é uma abreviação de ‘print working directory’ (imprimir diretório de trabalho). Tudo o que ele faz é imprimir o diretório de trabalho atual do shell. Mas o que é um diretório de trabalho?

Um conceito importante a entender é que o shell tem uma noção de um local padrão onde qualquer operação de arquivo ocorrerá. Este é o seu diretório de trabalho. Se você tentar criar novos arquivos ou diretórios, visualizar arquivos existentes ou até mesmo excluí-los, o shell assumirá que você está procurando por eles no diretório de trabalho atual, a menos que você tome medidas para especificar o contrário. Portanto, é bastante importante ter uma ideia de qual diretório o shell está “dentro” em qualquer momento, afinal, excluir arquivos do diretório errado pode ser desastroso. Se você estiver em dúvida, o comando `pwd` dirá exatamente qual é o diretório de trabalho atual.

Você pode mudar o diretório de trabalho usando o comando `cd`, uma abreviação de ‘change directory’ (mudar diretório). Tente digitar o seguinte:

```sh
cd /
pwd
```

Note que o separador de diretórios é uma barra (`/`), não a barra invertida que você pode estar acostumado em sistemas Windows ou DOS.

Agora seu diretório de trabalho é “/”. Se você vem de um ambiente Windows, provavelmente está acostumado a cada unidade ter sua própria letra, com seu disco rígido principal sendo tipicamente “C:”. Sistemas semelhantes ao Unix não dividem as unidades dessa forma. Em vez disso, eles têm um único sistema de arquivos unificado, e unidades individuais podem ser anexadas (“montadas”) a qualquer local no sistema de arquivos que faça mais sentido. O diretório “/”, frequentemente chamado de diretório raiz, é a base desse sistema de arquivos unificado. A partir daí, tudo o mais se ramifica para formar uma árvore de diretórios e subdiretórios.

## Muitas raízes

Cuidado: embora o diretório “/” seja às vezes chamado de diretório raiz, a palavra “root” tem outro significado. `root` também é o nome usado para o superusuário desde os primeiros dias do Unix. O superusuário, como o nome sugere, tem mais poderes do que um usuário normal, então pode facilmente causar estragos com um comando digitado incorretamente. Vamos olhar a conta do superusuário mais detalhadamente na seção 7. Por enquanto, você só precisa saber que a palavra “root” tem múltiplos significados no mundo Linux, então o contexto é importante.

A partir do diretório raiz, o seguinte comando moverá você para o diretório “home” (que é um subdiretório imediato de “/”):

```sh
cd home
pwd
```

Para subir para o diretório pai, neste caso de volta para “/”, use a sintaxe especial de dois pontos (`..`) ao mudar de diretório (note o espaço entre `cd` e

Repos

, ao contrário do DOS, você não pode simplesmente digitar `cd..` como um comando único):

```sh
cd ..
pwd
```

Digitar `cd` sozinho é um atalho rápido para voltar ao seu diretório home:

```sh
cd
pwd
```

Você também pode usar

Repos

 mais de uma vez se precisar subir por vários níveis de diretórios pai:

```sh
cd ../..
pwd
```

Note que no exemplo anterior descrevemos uma rota a seguir pelos diretórios. O caminho que usamos significa “começando do diretório de trabalho, mova-se para o pai / a partir dessa nova localização, mova-se para o pai novamente”. Então, se quisermos ir diretamente do nosso diretório home para o diretório “etc” (que está diretamente dentro da raiz do sistema de arquivos), poderíamos usar esta abordagem:

```sh
cd
pwd

cd ../../etc
pwd
```

## Caminhos relativos e absolutos

A maioria dos exemplos que vimos até agora usa caminhos relativos. Ou seja, o lugar onde você acaba depende do seu diretório de trabalho atual. Considere tentar `cd` para a pasta “etc”. Se você já estiver no diretório raiz, isso funcionará bem:

```sh
cd /
pwd
cd etc
pwd
```

Mas e se você estiver no seu diretório home?

```sh
cd
pwd
cd etc
pwd
```

Você verá um erro dizendo “No such file or directory” (Nenhum arquivo ou diretório encontrado) antes mesmo de executar o último `pwd`. Mudar de diretório especificando o nome do diretório, ou usando

Repos

 terá efeitos diferentes dependendo de onde você começa. O caminho só faz sentido em relação ao seu diretório de trabalho.

Mas vimos dois comandos que são absolutos. Não importa qual seja o seu diretório de trabalho atual, eles terão o mesmo efeito. O primeiro é quando você executa `cd` sozinho para ir diretamente ao seu diretório home. O segundo é quando você usou `cd /` para mudar para o diretório raiz. Na verdade, qualquer caminho que comece com uma barra é um caminho absoluto. Você pode pensar nisso como dizendo “mude para o diretório raiz, depois siga a rota a partir daí”. Isso nos dá uma maneira muito mais fácil de mudar para o diretório `etc`, não importa onde estamos atualmente no sistema de arquivos:

```sh
cd
pwd
cd /etc
pwd
```

Isso também nos dá outra maneira de voltar ao seu diretório home, e até mesmo para as pastas dentro dele. Suponha que você queira ir diretamente para a sua pasta “Desktop” de qualquer lugar no disco (note o “D” maiúsculo). No comando a seguir, você precisará substituir `USERNAME` pelo seu próprio nome de usuário, o comando `whoami` lembrará você do seu nome de usuário, caso você não tenha certeza:

```sh
whoami
cd /home/USERNAME/Desktop
pwd
```

Há outro atalho útil que funciona como um caminho absoluto. Como você viu, usar “/” no início do seu caminho significa “começando do diretório raiz”. Usar o caractere til (“~”) no início do seu caminho significa de forma semelhante “começando do meu diretório home”.

```sh
cd ~
pwd

cd ~/Desktop
pwd
```

Agora aquele texto estranho no prompt pode fazer um pouco de sentido. Você notou que ele muda conforme você se move pelo sistema de arquivos? Em um sistema Ubuntu, ele mostra seu nome de usuário, o nome da rede do seu computador e o diretório de trabalho atual. Mas se você estiver em algum lugar dentro do seu diretório home, ele usará “~” como uma abreviação. Vamos passear um pouco pelo sistema de arquivos e ficar de olho no prompt enquanto fazemos isso:

```sh
cd
cd /
cd ~/Desktop
cd /etc
cd /var/log
cd ..
cd
```

Você deve estar entediado de apenas se mover pelo sistema de arquivos agora, mas uma boa compreensão de caminhos absolutos e relativos será inestimável à medida que avançamos para criar algumas novas pastas e arquivos!
