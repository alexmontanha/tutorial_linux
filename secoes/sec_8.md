# 8. Arquivos ocultos

Antes de concluirmos este tutorial, vale a pena mencionar os arquivos (e pastas) ocultos. Estes são comumente usados em sistemas Linux para armazenar configurações e dados de configuração, e são tipicamente ocultos simplesmente para não desordenar a visualização dos seus próprios arquivos. Não há nada de especial em um arquivo ou pasta oculto, além do seu nome: simplesmente começar um nome com um ponto (“.”) é suficiente para fazê-lo desaparecer.

```sh
cd /tmp/tutorial
ls
mv combined.txt .combined.txt
ls
```

Você ainda pode trabalhar com o arquivo oculto certificando-se de incluir o ponto ao especificar seu nome de arquivo:

```sh
cat .combined.txt
mkdir .hidden
mv .combined.txt .hidden
less .hidden/.combined.txt
```

Se você executar `ls`, verá que o diretório `.hidden` está, como esperado, oculto. Você ainda pode listar seu conteúdo usando `ls .hidden`, mas como ele contém apenas um único arquivo que também está oculto, você não obterá muita saída. Mas você pode usar a opção `-a` (mostrar tudo) do `ls` para fazer com que ele mostre tudo em um diretório, incluindo os arquivos e pastas ocultos:

```sh
ls
ls -a
ls .hidden
ls -a .hidden
```

Observe que os atalhos que usamos anteriormente, `.` e Repos
, também aparecem como se fossem diretórios reais.

Quanto ao nosso comando `tree` recentemente instalado, ele funciona de maneira semelhante (exceto sem a aparição de `.` e Repos):

```sh
tree
tree -a
```

Volte para o seu diretório home (`cd`) e tente executar `ls` sem e depois com a opção `-a`. Canalize a saída através de `wc -l` para ter uma ideia mais clara de quantos arquivos e pastas ocultos estiveram bem debaixo do seu nariz todo esse tempo. Esses arquivos geralmente armazenam sua configuração pessoal, e é assim que os sistemas Unix sempre ofereceram a capacidade de ter configurações em nível de sistema (geralmente em `/etc`) que podem ser substituídas por usuários individuais (cortesia de arquivos ocultos em seu diretório home).

Você geralmente não precisará lidar com arquivos ocultos, mas ocasionalmente as instruções podem exigir que você entre em `.config` ou edite algum arquivo cujo nome comece com um ponto. Pelo menos agora você entenderá o que está acontecendo, mesmo quando não puder ver facilmente o arquivo em suas ferramentas gráficas.

## Limpando

Chegamos ao final deste tutorial, e você deve estar de volta ao seu diretório home agora (use `pwd` para verificar e `cd` para ir para lá se não estiver). É educado deixar seu computador no mesmo estado em que o encontramos, então, como um passo final, vamos remover a área experimental que estávamos usando anteriormente e depois verificar se ela realmente se foi:

```sh
rm -r /tmp/tutorial
ls /tmp
```

Como último passo, vamos fechar o terminal. Você pode simplesmente fechar a janela, mas é melhor prática sair do shell. Você pode usar o comando `logout` ou o atalho de teclado Ctrl-D. Se você planeja usar muito o terminal, memorizar Ctrl-Alt-T para lançar o terminal e Ctrl-D para fechá-lo logo fará com que ele pareça um assistente útil que você pode chamar instantaneamente e dispensar com a mesma facilidade.
