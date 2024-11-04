# 5. Movendo e manipulando arquivos

Agora que temos alguns arquivos, vamos ver o tipo de tarefas do dia a dia que você pode precisar realizar com eles. Na prática, você ainda provavelmente usará um programa gráfico quando quiser mover, renomear ou excluir um ou dois arquivos, mas saber como fazer isso usando a linha de comando pode ser útil para mudanças em massa, ou quando os arquivos estão espalhados entre diferentes pastas. Além disso, você aprenderá mais algumas coisas sobre a linha de comando ao longo do caminho.

Vamos começar colocando nosso arquivo `combined.txt` no diretório `dir1`, usando o comando `mv` (mover):

```sh
mv combined.txt dir1
```

Você pode confirmar que o trabalho foi feito usando `ls` para ver que ele não está mais no diretório de trabalho, depois `cd dir1` para mudar para `dir1`, `ls` para ver que ele está lá, e depois `cd ..` para voltar ao diretório de trabalho. Ou você pode economizar muita digitação passando um caminho diretamente para o comando `ls` para obter a confirmação que está procurando:

```sh
ls dir1
```

Agora suponha que descobrimos que o arquivo não deveria estar em `dir1` afinal. Vamos movê-lo de volta para o diretório de trabalho. Poderíamos usar `cd` para entrar em `dir1` e depois usar `mv combined.txt ..` para dizer “mova combined.txt para o diretório pai”. Mas podemos usar outro atalho de caminho para evitar mudar de diretório. Da mesma forma que dois pontos (`..`) representam o diretório pai, um único ponto (`.`) pode ser usado para representar o diretório de trabalho atual. Como sabemos que há apenas um arquivo em `dir1`, também podemos usar “*” para corresponder a qualquer nome de arquivo nesse diretório, economizando mais algumas teclas. Nosso comando para mover o arquivo de volta para o diretório de trabalho, portanto, se torna este (note o espaço antes do ponto, há dois parâmetros sendo passados para `mv`):

```sh
mv dir1/* .
```

O comando `mv` também nos permite mover mais de um arquivo por vez. Se você passar mais de dois argumentos, o último é considerado o diretório de destino e os outros são considerados arquivos (ou diretórios) a serem movidos. Vamos usar um único comando para mover `combined.txt`, todos os nossos arquivos `test_n.txt` e `dir3` para `dir2`. Há um pouco mais acontecendo aqui, mas se você olhar para cada argumento de cada vez, deve ser capaz de entender o que está acontecendo:

```sh
mv combined.txt test_* dir3 dir2
ls
ls dir2
```

Com `combined.txt` agora movido para `dir2`, o que acontece se decidirmos que ele está no lugar errado novamente? Em vez de `dir2`, ele deveria ter sido colocado em `dir6`, que está dentro de `dir5`, que está em `dir4`. Com o que sabemos agora sobre caminhos, isso não é problema:

```sh
mv dir2/combined.txt dir4/dir5/dir6
ls dir2
ls dir4/dir5/dir6
```

Observe como nosso comando `mv` nos permitiu mover o arquivo de um diretório para outro, mesmo que nosso diretório de trabalho seja algo completamente diferente. Esta é uma propriedade poderosa da linha de comando: não importa onde você esteja no sistema de arquivos, ainda é possível operar em arquivos e pastas em locais totalmente diferentes.

Como parece que estamos usando (e movendo) esse arquivo muito, talvez devêssemos manter uma cópia dele em nosso diretório de trabalho. Assim como o comando `mv` move arquivos, o comando `cp` os copia (novamente, note o espaço antes do ponto):

```sh
cp dir4/dir5/dir6/combined.txt .
ls dir4/dir5/dir6
ls
```

Ótimo! Agora vamos criar outra cópia do arquivo, em nosso diretório de trabalho, mas com um nome diferente. Podemos usar o comando `cp` novamente, mas em vez de dar um caminho de diretório como o último argumento, daremos um novo nome de arquivo:

```sh
cp combined.txt backup_combined.txt
ls
```

Isso é bom, mas talvez a escolha do nome de backup pudesse ser melhor. Por que não renomeá-lo para que ele sempre apareça próximo ao arquivo original em uma lista ordenada. A linha de comando tradicional do Unix trata uma renomeação como se você estivesse movendo o arquivo de um nome para outro, então nosso velho amigo `mv` é o comando a ser usado. Neste caso, você só precisa especificar dois argumentos: o arquivo que você deseja renomear e o novo nome que deseja usar.

```sh
mv backup_combined.txt combined_backup.txt
ls
```

Isso também funciona em diretórios, dando-nos uma maneira de resolver aqueles difíceis com espaços no nome que criamos anteriormente. Para evitar redigitar cada comando após o primeiro, use a seta para cima para puxar o comando anterior no histórico. Você pode então editar o comando antes de executá-lo movendo o cursor para a esquerda e para a direita com as teclas de seta, e removendo o caractere à esquerda com Backspace ou o que o cursor está sobre com Delete. Finalmente, digite o novo caractere no lugar e pressione Enter ou Return para executar o comando quando terminar. Certifique-se de alterar ambas as aparências do número em cada uma dessas linhas.

```sh
mv "folder 1" folder_1
mv "folder 2" folder_2
mv "folder 3" folder_3
mv "folder 4" folder_4
mv "folder 5" folder_5
mv "folder 6" folder_6
ls
```

## Excluindo arquivos e pastas

**Aviso**: Nesta próxima seção, vamos começar a excluir arquivos e pastas. Para ter certeza absoluta de que você não exclua acidentalmente nada na sua pasta home, use o comando `pwd` para verificar novamente se você ainda está no diretório `/tmp/tutorial` antes de prosseguir.

Agora sabemos como mover, copiar e renomear arquivos e diretórios. No entanto, como esses são apenas arquivos de teste, talvez não precisemos realmente de três cópias diferentes de `combined.txt`. Vamos arrumar um pouco, usando o comando `rm` (remover):

```sh
rm dir4/dir5/dir6/combined.txt combined_backup.txt
```

Talvez devêssemos remover alguns desses diretórios excessivos também:

```sh
rm folder_*
```

## Erro ao executar `rm` em diretórios

O que aconteceu aí? Bem, acontece que `rm` tem uma pequena rede de segurança. Claro, você pode usá-lo para excluir todos os arquivos em um diretório com um único comando, apagando acidentalmente milhares de arquivos em um instante, sem meios de recuperá-los. Mas ele não permitirá que você exclua um diretório. Suponho que isso ajude a evitar que você exclua acidentalmente milhares de arquivos, mas parece um pouco mesquinho para um comando tão destrutivo hesitar em remover um diretório vazio. Felizmente, existe um comando `rmdir` (remover diretório) que fará o trabalho para nós:

```sh
rmdir folder_*
```

## Erro ao executar `rmdir` em um diretório não vazio

Bem, isso é um pouco melhor, mas ainda há um erro. Se você executar `ls`, verá que a maioria das pastas se foi, mas `folder_6` ainda está por aí. Como você deve se lembrar, `folder_6` ainda tem uma pasta `folder_7` dentro dela, e `rmdir` só excluirá pastas vazias. Novamente, é uma pequena rede de segurança para evitar que você exclua acidentalmente uma pasta cheia de arquivos quando não pretendia.

Neste caso, no entanto, pretendemos. A adição de opções aos nossos comandos `rm` ou `rmdir` nos permitirá realizar ações perigosas sem a ajuda de uma rede de segurança! No caso de `rmdir`, podemos adicionar a opção `-p` para dizer a ele para também remover os diretórios pai. Pense nisso como o contraponto ao `mkdir -p`. Então, se você executar `rmdir -p dir1/dir2/dir3`, ele primeiro excluirá `dir3`, depois `dir2` e, finalmente, excluirá `dir1`. Ele ainda segue as regras normais do `rmdir` de excluir apenas diretórios vazios, então se houvesse também um arquivo em `dir1`, por exemplo, apenas `dir3` e `dir2` seriam removidos.

Uma abordagem mais comum, quando você está realmente, realmente, realmente certo de que deseja excluir um diretório inteiro e tudo dentro dele, é dizer ao `rm` para trabalhar recursivamente usando a opção `-r`, caso em que ele excluirá alegremente pastas, bem como arquivos. Com isso em mente, aqui está o comando para se livrar daquela pasta `folder_6` e do subdiretório dentro dela:

```sh
rm -r folder_6
ls
```

Lembre-se: embora `rm -r` seja rápido e conveniente, também é perigoso. É mais seguro excluir explicitamente arquivos para limpar um diretório e depois usar `cd ..` para o pai antes de usar `rmdir` para removê-lo.

**Aviso Importante**: Ao contrário das interfaces gráficas, `rm` não move arquivos para uma pasta chamada “lixeira” ou similar. Em vez disso, ele os exclui totalmente, completamente e irrevogavelmente. Você precisa ser extremamente cuidadoso com os parâmetros que usa com `rm` para garantir que está excluindo apenas o(s) arquivo(s) que pretende. Você deve ter um cuidado especial ao usar curingas, pois é fácil excluir acidentalmente mais arquivos do que pretendia. Um caractere de espaço errado em seu comando pode mudá-lo completamente: `rm t*` significa “excluir todos os arquivos começando com t”, enquanto `rm t *` significa “excluir o arquivo t, bem como qualquer arquivo cujo nome consista em zero ou mais caracteres”, o que seria tudo no diretório! Se você estiver em dúvida, use a opção `-i` (interativo) do `rm`, que solicitará que você confirme a exclusão de cada arquivo; digite `Y` para excluí-lo, `N` para mantê-lo e pressione `Ctrl-C` para interromper a operação completamente.
