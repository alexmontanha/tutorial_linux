# 6. Um pouco de encanamento

Os computadores e telefones de hoje têm capacidades gráficas e de áudio que os usuários de terminais dos anos 70 nem poderiam imaginar. No entanto, o texto ainda prevalece como um meio de organizar e categorizar arquivos. Seja o próprio nome do arquivo, coordenadas GPS embutidas nas fotos que você tira no seu telefone, ou os metadados armazenados em um arquivo de áudio, o texto ainda desempenha um papel vital em todos os aspectos da computação. É uma sorte para nós que a linha de comando do Linux inclua algumas ferramentas poderosas para manipular conteúdo de texto e maneiras de unir essas ferramentas para criar algo ainda mais capaz.

Vamos começar com uma pergunta simples. Quantas linhas há no seu arquivo `combined.txt`? O comando `wc` (word count) pode nos dizer isso, usando a opção `-l` para indicar que queremos apenas a contagem de linhas (ele também pode fazer contagens de caracteres e, como o nome sugere, contagens de palavras):

```sh
wc -l combined.txt
```

Da mesma forma, se você quiser saber quantos arquivos e pastas estão no seu diretório home, e depois limpar depois de si mesmo, você poderia fazer isso:

```sh
ls ~ > file_list.txt
wc -l file_list.txt
rm file_list.txt
```

Esse método funciona, mas criar um arquivo temporário para armazenar a saída de `ls` apenas para excluí-lo duas linhas depois parece um pouco excessivo. Felizmente, a linha de comando do Unix fornece um atalho que evita a necessidade de criar um arquivo temporário, pegando a saída de um comando (referida como saída padrão ou STDOUT) e alimentando-a diretamente como entrada para outro comando (entrada padrão ou STDIN). É como se você tivesse conectado um tubo entre a saída de um comando e a entrada do próximo comando, tanto que esse processo é realmente chamado de encanamento dos dados de um comando para outro. Aqui está como canalizar a saída do nosso comando `ls` para `wc`:

```sh
ls ~ | wc -l
```

Observe que não há arquivo temporário criado e nenhum nome de arquivo necessário. Os pipes operam inteiramente na memória, e a maioria das ferramentas da linha de comando do Unix esperará receber entrada de um pipe se você não especificar um arquivo para elas trabalharem. Olhando para a linha acima, você pode ver que são dois comandos, `ls ~` (listar o conteúdo do diretório home) e `wc -l` (contar as linhas), separados por um caractere de barra vertical (“|”). Esse processo de canalizar um comando para outro é tão comumente usado que o próprio caractere é frequentemente referido como o caractere pipe, então se você vir esse termo, agora sabe que ele significa apenas a barra vertical.

Observe que os espaços ao redor do caractere pipe não são importantes, usamos eles para clareza, mas o seguinte comando funciona igualmente bem, desta vez para nos dizer quantos itens estão no diretório `/etc`:

```sh
ls /etc|wc -l
```

Ufa! Isso é bastante arquivos. Se quiséssemos listar todos eles, claramente preencheria mais de uma tela. Como descobrimos anteriormente, quando um comando produz muita saída, é melhor usar `less` para visualizá-la, e esse conselho ainda se aplica ao usar um pipe (lembre-se, pressione `q` para sair):

```sh
ls /etc | less
```

Voltando aos nossos próprios arquivos, sabemos como obter o número de linhas em `combined.txt`, mas dado que ele foi criado concatenando os mesmos arquivos várias vezes, eu me pergunto quantas linhas únicas há? O Unix tem um comando, `uniq`, que só exibirá linhas únicas no arquivo. Então, precisamos usar `cat` para exibir o arquivo e canalizá-lo através de `uniq`. Mas tudo o que queremos é uma contagem de linhas, então precisamos usar `wc` também. Felizmente, a linha de comando não limita você a um único pipe por vez, então podemos continuar encadeando quantos comandos precisarmos:

```sh
cat combined.txt | uniq | wc -l
```

Essa linha provavelmente resultou em uma contagem que está bem próxima do número total de linhas no arquivo, se não exatamente a mesma. Certamente isso não pode estar certo? Remova o último pipe para ver a saída do comando para ter uma ideia melhor do que está acontecendo. Se o seu arquivo for muito longo, você pode querer canalizá-lo através de `less` para torná-lo mais fácil de inspecionar:

```sh
cat combined.txt | uniq | less
```

Parece que poucas, se houver, de nossas linhas duplicadas estão sendo removidas. Para entender por quê, precisamos olhar a documentação do comando `uniq`. A maioria das ferramentas da linha de comando vem com um manual breve (e às vezes nem tão breve), acessado através do comando `man` (manual). A saída é automaticamente canalizada através do seu pager, que normalmente será `less`, para que você possa mover-se para frente e para trás através da saída e, em seguida, pressionar `q` quando terminar:

```sh
man uniq
```

A página do manual para `uniq`

Como esse tipo de documentação é acessado via comando `man`, você ouvirá isso referido como uma “man page”, como em “verifique a man page para mais detalhes”. O formato das man pages é frequentemente conciso, pense nelas mais como uma visão geral rápida de um comando do que um tutorial completo. Elas são frequentemente altamente técnicas, mas você geralmente pode pular a maior parte do conteúdo e apenas procurar os detalhes da opção ou argumento que está usando.

A man page do `uniq` é um exemplo típico, começando com uma breve descrição de uma linha do comando, passando para uma sinopse de como usá-lo, e depois uma descrição detalhada de cada opção ou parâmetro. Mas, embora as man pages sejam inestimáveis, elas também podem ser impenetráveis. Elas são melhores usadas quando você precisa de um lembrete de um switch ou parâmetro específico, em vez de um recurso geral para aprender a usar a linha de comando. No entanto, a primeira linha da seção DESCRIPTION para `man uniq` responde à pergunta de por que as linhas duplicadas não foram removidas: ele só funciona em linhas adjacentes correspondentes.

A questão, então, é como reorganizar as linhas em nosso arquivo para que as entradas duplicadas estejam em linhas adjacentes. Se ordenássemos o conteúdo do arquivo alfabeticamente, isso resolveria o problema. O Unix oferece um comando `sort` para fazer exatamente isso. Uma verificação rápida do `man sort` mostra que podemos passar um nome de arquivo diretamente para o comando, então vamos ver o que ele faz com nosso arquivo:

```sh
sort combined.txt | less
```

Você deve ser capaz de ver que as linhas foram reordenadas, e agora está adequado para canalizar diretamente para `uniq`. Finalmente, podemos completar nossa tarefa de contar as linhas únicas no arquivo:

```sh
sort combined.txt | uniq | wc -l
```

Como você pode ver, a capacidade de canalizar dados de um comando para outro, construindo longas cadeias para manipular seus dados, é uma ferramenta poderosa, além de reduzir a necessidade de arquivos temporários e economizar muita digitação. Por essa razão, você verá isso usado com bastante frequência nas linhas de comando. Uma longa cadeia de comandos pode parecer intimidante à primeira vista, mas lembre-se de que você pode dividir até a cadeia mais longa em comandos individuais (e olhar suas man pages) para obter uma melhor compreensão do que está fazendo.

## Muitos manuais

A maioria das ferramentas da linha de comando do Linux inclui uma man page. Tente dar uma olhada rápida nas páginas de alguns dos comandos que você já encontrou: `man ls`, `man cp`, `man rmdir` e assim por diante. Há até uma man page para o próprio programa `man`, que é acessada usando `man man`, é claro.
