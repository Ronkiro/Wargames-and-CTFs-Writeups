# ~ Level 01 ~

Seguindo o passo-a-passo do jogo, realizei a conexão com o servidor do game via SSH.

```
ssh level1@io.netgarage.org

||i |||o || Welcome at IO!
||__|||__||
|/__\|/__\| If you have problems connecting please contact us on IRC. (irc.netgarage.org +6697)

level1@io.netgarage.org's password:
```

Então, utilizei a senha conforme informado pelo site.

Uma série de recomendações são então passadas, as quais prefiro destacar algumas, pois são importantes para o jogo.

```
Levels are in /levels
Passes are in ~/.pass
Readmes in /home/level1
```

O arquivo *tags* pode ser utilizado para "deixar sua marca".

O que for colocado nesse arquivo se torna acessível na área *Tags* do site principal, conforme a linha de comando abaixo

```
echo <texto> >> tags
```

"echo" representa o envio de um stream de bytes, enquanto >> redireciona para o arquivo *tags*, mantendo o conteúdo que estava nele anteriormente.

Nos READMEs podemos encontrar algumas recomendações extras, que auxiliam, principalmente, pessoas que estão à iniciar em War Games.

Decidi gastar um tempinho me ambientando e lendo os READMEs antes de iniciar o game realmente.

```
cd /levels
ls -la
drwxr-xr-x  3 root    root     4096 May 18 20:39 .
drwxr-xr-x 24 root    root     4096 Feb 18  2018 ..
drwxr-x--x  5 root    root     4096 Oct  4  2018 beta
-r-sr-x---  1 level2  level1   1184 Jan 13  2014 level01
-r-sr-x---  1 level3  level2   5329 Oct  4  2011 level02
...
```

Notei que não tenho permissão para acessar o level02. Enquanto ambos level01 e level02 são executáveis.
```
-bash: ./level02: Permission denied
```
(Tem que ter a confirmação, né...)

Ao executar o level01, temos a seguinte mensagem.

```
./level01
Enter the 3 digit passcode to enter:
```

Vemos então que temos que descobrir uma senha de três digitos para poder passar pelo level01.

Como já havia vasculhado anteriormente, eu tinha a certeza de que o level01 é o único arquivo que realmente tem alguma relevância dentro desse nível.

Os outros ou eram irrelevantes ou o usuário o qual eu estava (level01) não tinha permissão para acesso.

Então, decidi analisar o arquivo.

Por ser um executável, o primeiro pensamento que tive foi: "Vou usar o *gdb*".

```
gdb -q level01
Reading symbols from level01...(no debugging symbols found)...done.
```

Vamos começar pelo início, busquei o método main do level01.

```
(gdb) disassemble main
Dump of assembler code for function main:
   0x08048080 <+0>:     push   $0x8049128
   0x08048085 <+5>:     call   0x804810f
   0x0804808a <+10>:    call   0x804809f
   0x0804808f <+15>:    cmp    $0x10f,%eax
   0x08048094 <+20>:    je     0x80480dc
   0x0804809a <+26>:    call   0x8048103
End of assembler dump.
```

Notamos que ele faz chamada à algumas funções, mas a comparação ocorre na linha do *cmp*, a qual ele compara o 0x10F com o acumulador (*eax*).

Com o valor de retorno do *cmp* guardado, ele verifica se está tudo ok (condição *je*).

Então, decidi analisar melhor o 0x10F.

```
q
echo "ibase=16; 10F" | bc
271
```

271 parece ser a senha, então optei por tentar.

```
Enter the 3 digit passcode to enter: 271
Congrats you found it, now read the password for level2 from /home/level2/.pass
sh-4.3$
```

Conseguimos! :)

Entramos agora em uma shell própria, com acessos de level2.

Então, acesso o arquivo com *cat* e busco a senha.

```
cat /home/level2/.pass
#############
```

Agora precisamos apenas desconectar e acessar com a senha fornecida.

(Escondi com "##", para não dar spoilers :D)