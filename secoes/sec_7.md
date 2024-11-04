# 7. A linha de comando e o superusuário

Uma boa razão para aprender alguns conceitos básicos da linha de comando é que as instruções online frequentemente favorecem o uso de comandos de shell em vez de uma interface gráfica. Quando essas instruções exigem mudanças na sua máquina que vão além de modificar alguns arquivos no seu diretório home, inevitavelmente você se deparará com comandos que precisam ser executados como administrador da máquina (ou superusuário na terminologia Unix). Antes de começar a executar comandos arbitrários que você encontra em algum canto obscuro da internet, vale a pena entender as implicações de executar como administrador e como identificar essas instruções que exigem isso, para que você possa avaliar melhor se são seguras para executar ou não.

O superusuário é, como o nome sugere, um usuário com superpoderes. Em sistemas mais antigos, era um usuário real, com um nome de usuário real (quase sempre “root”) que você poderia fazer login se tivesse a senha. Quanto aos superpoderes: root pode modificar ou excluir qualquer arquivo em qualquer diretório do sistema, independentemente de quem os possui; root pode reescrever regras de firewall ou iniciar serviços de rede que poderiam potencialmente abrir a máquina para um ataque; root pode desligar a máquina mesmo que outras pessoas ainda estejam usando. Em resumo, root pode fazer praticamente qualquer coisa, contornando facilmente as salvaguardas que geralmente são colocadas para impedir que os usuários ultrapassem seus limites.

Claro, uma pessoa logada como root é tão capaz de cometer erros quanto qualquer outra. Os anais da história da computação estão cheios de histórias de um comando digitado incorretamente que deletou todo o sistema de arquivos ou matou um servidor vital. Além disso, há a possibilidade de um ataque malicioso: se um usuário estiver logado como root e deixar sua mesa, não é muito difícil para um colega descontente acessar sua máquina e causar estragos. Apesar disso, a natureza humana sendo o que é, muitos administradores ao longo dos anos foram culpados de usar root como sua conta principal ou única.

## Não use a conta root

Se alguém pedir para você habilitar a conta root ou fazer login como root, desconfie muito das intenções dessa pessoa.

Em um esforço para reduzir esses problemas, muitas distribuições Linux começaram a incentivar o uso do comando `su`. Este é descrito de várias maneiras como sendo uma abreviação de ‘superuser’ ou ‘switch user’, e permite que você mude para outro usuário na máquina sem precisar fazer logout e login novamente. Quando usado sem argumentos, assume-se que você quer mudar para o usuário root (daí a primeira interpretação do nome), mas você pode passar um nome de usuário para ele a fim de mudar para uma conta de usuário específica (a segunda interpretação). Ao incentivar o uso do `su`, o objetivo era persuadir os administradores a passarem a maior parte do tempo usando uma conta normal, mudando para a conta de superusuário apenas quando necessário, e depois usar o comando `logout` (ou o atalho Ctrl-D) o mais rápido possível para retornar à sua conta de nível de usuário.

Minimizando o tempo gasto logado como root, o uso do `su` reduz a janela de oportunidade para cometer um erro catastrófico. Apesar disso, a natureza humana sendo o que é, muitos administradores foram culpados de deixar terminais de longa duração abertos nos quais usaram `su` para mudar para a conta root. Nesse aspecto, o `su` foi apenas um pequeno passo à frente para a segurança.

## Não use `su`

Se alguém pedir para você usar `su`, fique atento. Se você estiver usando Ubuntu, a conta root está desabilitada por padrão, então `su` sem parâmetros não funcionará. Mas ainda assim não vale a pena correr o risco, caso a conta tenha sido habilitada sem você perceber. Se você for solicitado a usar `su` com um nome de usuário, então (se você tiver a senha) terá acesso a todos os arquivos desse usuário e poderá excluí-los ou modificá-los acidentalmente.

Ao usar `su`, toda a sua sessão de terminal é trocada para o outro usuário. Comandos que não precisam de acesso root, algo tão mundano quanto `pwd` ou `ls`, seriam executados sob os auspícios do superusuário, aumentando o risco de um bug no programa causar grandes problemas. Pior ainda, se você perder a noção de qual usuário está operando atualmente, pode emitir um comando que é bastante benigno quando executado como usuário, mas que poderia destruir todo o sistema se executado como root.

É melhor desabilitar completamente a conta root e, em vez de permitir sessões de terminal de longa duração com poderes perigosos, exigir que o usuário solicite especificamente direitos de superusuário por comando. A chave para essa abordagem é um comando chamado `sudo` (como em “switch user and do this command”).

## Usando `sudo`

O `sudo` é usado para prefixar um comando que precisa ser executado com privilégios de superusuário. Um arquivo de configuração é usado para definir quais usuários podem usar `sudo` e quais comandos eles podem executar. Ao executar um comando assim, o usuário é solicitado a fornecer sua própria senha, que é então armazenada em cache por um período de tempo (padrão de 15 minutos), para que, se precisar executar vários comandos de nível de superusuário, não continue sendo solicitado a digitá-la continuamente.

Em um sistema Ubuntu, o primeiro usuário criado quando o sistema é instalado é considerado o superusuário. Ao adicionar um novo usuário, há uma opção para criá-lo como administrador, caso em que ele também poderá executar comandos de superusuário com `sudo`. Nesta captura de tela do Ubuntu 18.04, você pode ver a opção no topo do diálogo:

Assumindo que você está em um sistema Linux que usa `sudo`, e sua conta está configurada como administrador, tente o seguinte para ver o que acontece quando você tenta acessar um arquivo considerado sensível (ele contém senhas criptografadas):

```sh
cat /etc/shadow
sudo cat /etc/shadow
```

Se você digitar sua senha quando solicitado, deverá ver o conteúdo do arquivo `/etc/shadow`. Agora limpe o terminal executando o comando `reset` e execute `sudo cat /etc/shadow` novamente. Desta vez, o arquivo será exibido sem solicitar uma senha, pois ainda está no cache.

## Tenha cuidado com `sudo`

Se você for instruído a executar um comando com `sudo`, certifique-se de entender o que o comando está fazendo antes de continuar. Executar com `sudo` dá a esse comando todos os mesmos poderes de um superusuário. Por exemplo, o site de um editor de software pode pedir para você baixar um arquivo e alterar suas permissões, depois usar `sudo` para executá-lo. A menos que você saiba exatamente o que o arquivo está fazendo, você está abrindo uma brecha pela qual malware poderia ser potencialmente instalado no seu sistema. O `sudo` pode executar apenas um comando por vez, mas esse comando pode, por sua vez, executar muitos outros. Trate qualquer novo uso do `sudo` como sendo tão perigoso quanto fazer login como root.

Para instruções direcionadas ao Ubuntu, uma aparição comum do `sudo` é para instalar novos softwares no seu sistema usando os comandos `apt` ou `apt-get`. Se as instruções exigirem que você primeiro adicione um novo repositório de software ao seu sistema, usando o comando `apt-add-repository`, editando arquivos em `/etc/apt`, ou usando um “PPA” (Personal Package Archive), você deve ter cuidado, pois essas fontes não são curadas pela Canonical. Mas muitas vezes as instruções apenas exigem que você instale software dos repositórios padrão, o que deve ser seguro.

## Instalando novos softwares

Existem muitas maneiras diferentes de instalar software em sistemas Linux. Instalar diretamente dos repositórios oficiais da sua distribuição é a opção mais segura, mas às vezes o aplicativo ou versão que você deseja simplesmente não está disponível dessa forma. Ao instalar por qualquer outro mecanismo, certifique-se de obter os arquivos de uma fonte oficial para o projeto em questão.

Indicações de que os arquivos estão vindo de fora dos repositórios da distribuição incluem (mas não se limitam a) o uso de qualquer um dos seguintes comandos: `curl`, `wget`, `pip`, `npm`, `make`, ou quaisquer instruções que digam para você alterar as permissões de um arquivo para torná-lo executável.

Cada vez mais, o Ubuntu está fazendo uso de “snaps”, um novo formato de pacote que oferece algumas melhorias de segurança ao confinar mais de perto os programas para impedi-los de acessar partes do sistema que não precisam. Mas algumas opções podem reduzir o nível de segurança, então, se você for solicitado a executar `snap install` com quaisquer parâmetros além do nome do snap, vale a pena verificar exatamente o que o comando está tentando fazer.

Vamos instalar um novo programa de linha de comando dos repositórios padrão do Ubuntu para ilustrar esse uso do `sudo`:

```sh
sudo apt install tree
```

Depois de fornecer sua senha, o programa `apt` imprimirá várias linhas de texto para informar o que está fazendo. O programa `tree` é pequeno, então não deve demorar mais do que um ou dois minutos para baixar e instalar para a maioria dos usuários. Assim que você retornar ao prompt normal da linha de comando, o programa estará instalado e pronto para uso. Vamos executá-lo para obter uma visão geral melhor de como nossa coleção de arquivos e pastas se parece:

```sh
cd /tmp/tutorial
tree
```

A saída do comando `tree`

Voltando ao comando que realmente instalou o novo programa (`sudo apt install tree`), ele parece um pouco diferente dos que você viu até agora. Na prática, funciona assim:

O comando `sudo`, quando usado sem nenhuma opção, assumirá que o primeiro parâmetro é um comando para ser executado com privilégios de superusuário. Quaisquer outros parâmetros serão passados diretamente para o novo comando. As opções do `sudo` começam todas com um ou dois hífens e devem seguir imediatamente o comando `sudo`, para que não haja confusão sobre se o segundo parâmetro na linha é um comando ou uma opção.

O comando neste caso é `apt`. Ao contrário dos outros comandos que vimos, este não trabalha diretamente com arquivos. Em vez disso, espera que seu primeiro parâmetro seja uma instrução a ser executada (`install`), com o restante dos parâmetros variando com base na instrução.

Neste caso, o comando `install` diz ao `apt` que o restante da linha de comando consistirá em um ou mais nomes de pacotes a serem instalados dos repositórios de software do sistema. Normalmente, isso adicionará novos softwares à máquina, mas os pacotes podem ser qualquer coleção de arquivos que precisam ser instalados em locais específicos, como fontes ou imagens de desktop.

Você pode colocar `sudo` na frente de qualquer comando para executá-lo como superusuário, mas raramente há necessidade disso. Mesmo arquivos de configuração do sistema podem frequentemente ser visualizados (com `cat` ou `less`) como um usuário normal, e só exigem privilégios de root se você precisar editá-los.

## Cuidado com `sudo su`

Um truque com `sudo` é usá-lo para executar o comando `su`. Isso lhe dará um shell root mesmo se a conta root estiver desabilitada. Pode ser útil quando você precisa executar uma série de comandos como superusuário, para evitar ter que prefixá-los todos com `sudo`, mas isso o expõe exatamente aos mesmos tipos de problemas descritos para `su` acima. Se você seguir qualquer instrução que diga para executar `sudo su`, esteja ciente de que todos os comandos após isso serão executados como usuário root.

Nesta seção, você aprendeu sobre os perigos da conta root e como os sistemas Linux modernos, como o Ubuntu, tentam reduzir o risco de perigo usando `sudo`. Mas qualquer uso de poderes de superusuário deve ser considerado cuidadosamente. Ao seguir instruções que você encontra online, agora você deve estar em uma posição melhor para identificar aqueles comandos que podem exigir maior escrutínio.
