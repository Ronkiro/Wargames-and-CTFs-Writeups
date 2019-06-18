# ~ Level 02 ~

```
cd /levels
./level02
source code is available in level02.c
```

Desta vez o game foi um pouco mais expl�cito.

Analisei o c�digo em *level02.c*

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

** Pare sua leitura por aqui se n�o quiser spoilers ;) **

===========================================
## DICA 01:
O source code � real.
===========================================

===========================================
## DICA 02:

Voc� quer for�ar o software � chamar o m�todo *catcher*.
===========================================

## Resolu��o:

Vamos analisar melhor.

Vemos que o m�todo catcher nos leva � resolu��o do n�vel. Ent�o temos de utilizar artif�cios para cham�-lo.

```
signal(SIGFPE, catcher);
```

Esta linha atribue uma conex�o do sinal SIGFPE ao m�todo catcher, isto significa que lan�ar um sinal SIGFPE � o que iremos buscar.

(**NOTA:** SIGFPE � a representa��o de uma exce��o fatal error, uma chamada lan�ada pelo sistema. Conseguimos este por exemplo com divis�es de zero, overflows e etc)

Notamos que o argv[1] � divido pelo argv[2].

Todavia, n�o conseguimos for�ar o envio do sinal SIGFPE atrav�s de uma divis�o por zero, neste caso.

Ent�o buscamos sobre o m�todo *atoi*.

*If the converted value would be out of the range of representable values by an int, it causes undefined behavior. *

Ref: http://www.cplusplus.com/reference/cstdlib/atoi/

Encontramos algo interessante para se exploitar.

Vamos verificar os limites de um *int*. Notei que o mesmo vai de -2147483648 � +2147483647.

Ref: https://www.tutorialspoint.com/c_standard_library/limits_h.htm

(Vari�veis *INT_MAX* e *INT_MIN*)

Lendo sobre o m�todo *signal*, temos um outro aux�lio:

``` 
Integer division by zero has undefined result. On some architectures it will generate a SIGFPE signal. (Also dividing the most negative integer by -1 may generate SIGFPE.)
```

Ref: https://linux.die.net/man/2/signal 

J� que n�o conseguimos a divis�o por zero, podemos tentar o segundo caso.

POR�M, temos o m�todo *abs*, evitando que consigamos essa divis�o desta forma.

E se tentarmos inverter a l�gica?

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