# 4. Criando pastas e arquivos

Nesta seção, vamos criar alguns arquivos reais para trabalhar. Para evitar sobrescrever acidentalmente qualquer um dos seus arquivos reais, vamos começar criando um novo diretório, bem longe da sua pasta home, que servirá como um ambiente mais seguro para experimentar:

```sh
mkdir /tmp/tutorial
cd /tmp/tutorial
```

Observe o uso de um caminho absoluto, para garantir que criamos o diretório tutorial dentro de `/tmp`. Sem a barra no início, o comando `mkdir` tentaria encontrar um diretório `tmp` dentro do diretório de trabalho atual e, em seguida, tentar criar um diretório `tutorial` dentro dele. Se não encontrasse um diretório `tmp`, o comando falharia.

Caso você não tenha adivinhado, `mkdir` é uma abreviação de ‘make directory’ (criar diretório). Agora que estamos seguramente dentro da nossa área de teste (verifique novamente com `pwd` se não tiver certeza), vamos criar alguns subdiretórios:

```sh
mkdir dir1 dir2 dir3
```

Há algo um pouco diferente nesse comando. Até agora, vimos apenas comandos que funcionam sozinhos (`cd`, `pwd`) ou que têm um único item depois (`cd /`, `cd ~/Desktop`). Mas desta vez adicionamos três coisas após o comando `mkdir`. Essas coisas são chamadas de parâmetros ou argumentos, e diferentes comandos podem aceitar diferentes números de argumentos. O comando `mkdir` espera pelo menos um argumento, enquanto o comando `cd` pode funcionar com zero ou um, mas não mais. Veja o que acontece quando você tenta passar o número errado de parâmetros para um comando:

```sh
mkdir
cd /etc ~/Desktop
```

Voltando aos nossos novos diretórios. O comando acima terá criado três novos subdiretórios dentro da nossa pasta. Vamos dar uma olhada neles com o comando `ls` (listar):

```sh
ls
```

Se você seguiu os últimos comandos, seu terminal deve estar parecido com isto:

O terminal, após executar `mkdir` e `ls`

Observe que `mkdir` criou todas as pastas em um diretório. Ele não criou `dir3` dentro de `dir2` dentro de `dir1`, ou qualquer outra estrutura aninhada. Mas às vezes é útil poder fazer exatamente isso, e `mkdir` tem uma maneira:

```sh
mkdir -p dir4/dir5/dir6
ls
```

Desta vez, você verá que apenas `dir4` foi adicionado à lista, porque `dir5` está dentro dele, e `dir6` está dentro de `dir5`. Mais tarde, instalaremos uma ferramenta útil para visualizar a estrutura, mas você já tem conhecimento suficiente para confirmá-la:

```sh
cd dir4
ls
cd dir5
ls
cd ../..
```

O “-p” que usamos é chamado de opção ou switch (neste caso, significa “criar os diretórios pai também”). As opções são usadas para modificar a maneira como um comando opera, permitindo que um único comando se comporte de várias maneiras diferentes. Infelizmente, devido a peculiaridades da história e da natureza humana, as opções podem assumir diferentes formas em diferentes comandos. Você frequentemente as verá como caracteres únicos precedidos por um hífen (como neste caso), ou como palavras mais longas precedidas por dois hífens. A forma de caractere único permite que várias opções sejam combinadas, embora nem todos os comandos aceitem isso. E para complicar ainda mais, alguns comandos não identificam claramente suas opções, se algo é uma opção é ditado puramente pela ordem dos argumentos! Você não precisa se preocupar com todas as possibilidades, apenas saiba que as opções existem e podem assumir várias formas diferentes. Por exemplo, os seguintes comandos significam exatamente a mesma coisa:

```sh
# Não digite estes comandos, eles estão aqui apenas para fins demonstrativos
mkdir --parents --verbose dir4/dir5
mkdir -p --verbose dir4/dir5
mkdir -p -v dir4/dir5
mkdir -pv dir4/dir5
```

Agora sabemos como criar vários diretórios apenas passando-os como argumentos separados para o comando `mkdir`. Mas suponha que queremos criar um diretório com um espaço no nome? Vamos tentar:

```sh
mkdir another folder
ls
```

Provavelmente você nem precisou digitar esse comando para adivinhar o que aconteceria: duas novas pastas, uma chamada `another` e a outra chamada `folder`. Se você quiser trabalhar com espaços em nomes de diretórios ou arquivos, precisa escapá-los. Não se preocupe, ninguém está fugindo da prisão; escapar é um termo de computação que se refere ao uso de códigos especiais para dizer ao computador para tratar determinados caracteres de maneira diferente do normal. Digite os seguintes comandos para experimentar diferentes maneiras de criar pastas com espaços no nome:

```sh
mkdir "folder 1"
mkdir 'folder 2'
mkdir folder\ 3
mkdir "folder 4" "folder 5"
mkdir -p "folder 6"/"folder 7"
ls
```

Embora a linha de comando possa ser usada para trabalhar com arquivos e pastas com espaços nos nomes, a necessidade de escapá-los com aspas ou barras invertidas torna as coisas um pouco mais difíceis. Você pode frequentemente identificar uma pessoa que usa muito a linha de comando apenas pelos nomes dos arquivos: elas tendem a usar letras e números, e usar underscores (“_”) ou hífens (“-”) em vez de espaços.

## Criando arquivos usando redirecionamento

Nossa pasta de demonstração está começando a parecer bastante cheia de diretórios, mas está um pouco carente de arquivos. Vamos resolver isso redirecionando a saída de um comando para que, em vez de ser impressa na tela, ela acabe em um novo arquivo. Primeiro, lembre-se do que o comando `ls` está mostrando atualmente:

```sh
ls
```

Suponha que quiséssemos capturar a saída desse comando como um arquivo de texto que podemos visualizar ou manipular posteriormente. Tudo o que precisamos fazer é adicionar o caractere de maior que (“>”) ao final da nossa linha de comando, seguido pelo nome do arquivo para o qual escrever:

```sh
ls > output.txt
```

Desta vez, nada é impresso na tela, porque a saída está sendo redirecionada para o nosso arquivo. Se você executar `ls` sozinho, verá que o arquivo `output.txt` foi criado. Podemos usar o comando `cat` para ver seu conteúdo:

```sh
cat output.txt
```

Ok, então não é exatamente o que foi exibido na tela anteriormente, mas contém todos os mesmos dados, e está em um formato mais útil para processamento posterior. Vamos olhar para outro comando, `echo`:

```sh
echo "This is a test"
```

Sim, `echo` apenas imprime seus argumentos de volta (daí o nome). Mas combine-o com um redirecionamento, e você tem uma maneira fácil de criar pequenos arquivos de teste:

```sh
echo "This is a test" > test_1.txt
echo "This is a second test" > test_2.txt
echo "This is a third test" > test_3.txt
ls
```

Você deve usar `cat` em cada um desses arquivos para verificar seu conteúdo. Mas `cat` é mais do que apenas um visualizador de arquivos - seu nome vem de ‘concatenate’ (concatenar), que significa “ligar juntos”. Se você passar mais de um nome de arquivo para `cat`, ele exibirá cada um deles, um após o outro, como um único bloco de texto:

```sh
cat test_1.txt test_2.txt test_3.txt
```

Quando você quiser passar vários nomes de arquivos para um único comando, há alguns atalhos úteis que podem economizar muito tempo se os arquivos tiverem nomes semelhantes. Um ponto de interrogação (“?”) pode ser usado para indicar “qualquer caractere único” dentro do nome do arquivo. Um asterisco (“*”) pode ser usado para indicar “zero ou mais caracteres”. Estes são às vezes chamados de caracteres “curinga”. Alguns exemplos podem ajudar, os seguintes comandos fazem a mesma coisa:

```sh
cat test_1.txt test_2.txt test_3.txt
cat test_?.txt
cat test_*
```

## Mais necessidade de escapar

Como você deve ter adivinhado, essa capacidade também significa que você precisa escapar de nomes de arquivos com caracteres `?` ou `*` neles também. É geralmente melhor evitar qualquer pontuação em nomes de arquivos se você quiser manipulá-los a partir da linha de comando.

Se você olhar para a saída de `ls`, notará que os únicos arquivos ou pastas que começam com “t” são os três arquivos de teste que acabamos de criar, então você poderia simplificar ainda mais aquele último comando para `cat t*`, significando “concatenar todos os arquivos cujos nomes começam com um t e são seguidos por zero ou mais outros caracteres”. Vamos usar essa capacidade para juntar todos os nossos arquivos em um único novo arquivo e depois visualizá-lo:

```sh
cat t* > combined.txt
cat combined.txt
```

O que você acha que acontecerá se executarmos esses dois comandos uma segunda vez? O computador reclamará porque o arquivo já existe? Ele anexará o texto ao arquivo, de modo que contenha duas cópias? Ou ele o substituirá completamente? Experimente para ver o que acontece, mas para evitar digitar os comandos novamente, você pode usar as teclas de seta para cima e para baixo para mover-se pelo histórico de comandos que você usou. Pressione a seta para cima algumas vezes para chegar ao primeiro `cat` e pressione Enter para executá-lo, depois faça o mesmo novamente para chegar ao segundo.

Como você pode ver, o arquivo parece o mesmo. Isso não é porque ele foi deixado intocado, mas porque o shell limpa todo o conteúdo do arquivo antes de escrever a saída do seu comando `cat` nele. Por causa disso, você deve ter muito cuidado ao usar redirecionamento para garantir que não sobrescreva acidentalmente um arquivo que você precisa. Se você quiser anexar, em vez de substituir, o conteúdo dos arquivos, dobre o caractere de maior que:

```sh
cat t* >> combined.txt
echo "I've appended a line!" >> combined.txt
cat combined.txt
```

Repita o primeiro `cat` mais algumas vezes, usando a seta para cima para conveniência, e talvez adicione alguns comandos `echo` arbitrários, até que seu documento de texto seja tão grande que não caiba todo no terminal de uma vez quando você usar `cat` para exibi-lo. Para ver o arquivo inteiro, agora precisamos usar um programa diferente, chamado pager (porque ele exibe seu arquivo uma “página” de cada vez). O pager padrão de antigamente era chamado `more`, porque ele coloca uma linha de texto na parte inferior de cada página que diz “–More–” para indicar que você ainda não leu tudo. Hoje em dia, há um pager muito melhor que você deve usar: porque ele substitui o `more`, os programadores decidiram chamá-lo de `less`.

```sh
less combined.txt
```

Ao visualizar um arquivo através do `less`, você pode usar as teclas de seta para cima, seta para baixo, Page Up, Page Down, Home e End para mover-se pelo arquivo. Experimente para ver a diferença entre elas. Quando terminar de visualizar seu arquivo, pressione `q` para sair do `less` e retornar à linha de comando.

## Uma nota sobre diferenciação entre maiúsculas e minúsculas

Sistemas Unix são sensíveis a maiúsculas e minúsculas, ou seja, eles consideram “A.txt” e “a.txt” como dois arquivos diferentes. Se você executar as seguintes linhas, acabará com três arquivos:

```sh
echo "Lower case" > a.txt
echo "Upper case" > A.TXT
echo "Mixed case" > A.txt
```

Geralmente, você deve tentar evitar criar arquivos e pastas cujo nome varia apenas em maiúsculas e minúsculas. Isso não só ajudará a evitar confusão, mas também evitará problemas ao trabalhar com diferentes sistemas operacionais. O Windows, por exemplo, não diferencia maiúsculas de minúsculas, então trataria todos os três nomes de arquivos acima como um único arquivo, potencialmente causando perda de dados ou outros problemas.

Você pode ser tentado a apenas pressionar a tecla Caps Lock e usar letras maiúsculas para todos os seus nomes de arquivos. Mas a grande maioria dos comandos do shell são em letras minúsculas, então você acabaria frequentemente tendo que ligar e desligar a tecla Caps Lock enquanto digita. A maioria dos usuários experientes da linha de comando tende a usar principalmente nomes em letras minúsculas para seus arquivos e diretórios, para que raramente precisem se preocupar com conflitos de nomes de arquivos ou com qual caso usar para cada letra no nome.

## Boas práticas de nomenclatura

Quando você considera tanto a sensibilidade a maiúsculas e minúsculas quanto a necessidade de escapar, uma boa regra é manter seus nomes de arquivos todos em letras minúsculas, com apenas letras, números, underscores e hífens. Para arquivos, geralmente há também um ponto e alguns caracteres no final para indicar o tipo de arquivo (referido como “extensão de arquivo”). Esta diretriz pode parecer restritiva, mas se você acabar usando a linha de comando com frequência, ficará feliz por ter seguido esse padrão.
