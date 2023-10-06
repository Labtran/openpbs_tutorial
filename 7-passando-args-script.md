Passando Argumentos para Job Scripts[¶](#passando-args-para-job-scripts "Permanent link")
=======================================================================================

Nesta seção, veremos uma maneira de passar argumentos da linha de comando para o script de envio de trabalho do PBS. Isso pode ser muito útil se você quiser definir alguma variável fora do script e passá-la para o script no momento do envio.

Argumentos na linha de comando[¶](#argumentos-na-linha-comando "Permanent link")
-------------------------------------------------------------------

O que é "passar argumentos"?   É quando fornecemos a um script ou programa uma ou mais informações adicionais na linha de comando. Por exemplo, aqui estamos passando um argumento de linha de comando para o script `myScript.sh`. O valor do argumento é `mygenome.fasta`.

```
$ myScript.sh mygenome.fasta
``` 

Mas não podemos passar diretamente esses argumentos de linha de comando quando nosso script é um argumento para o comando qsub. (args é a abreviação de arguments, os usuários de Linux/UNIX gostam de abreviações).

```
$ qsub myScript.sh mygenome.fasta   <== This won't work.
``` 

No entanto, há uma maneira de fazer isso. Passe os args como variáveis de ambiente para seu script de trabalho usando a opção `-v` do qsub.

```
$ qsub -v parameter_name=parameter_value[,par_name=par_value...] script.sh
``` 

Em seguida, `$parameter_name` pode ser usado como variável em seu script.

Passando em uma variável[¶](#passando-em-uma-variavel "Permanent link")
-------------------------------------------------------------------------------

Aqui está um exemplo de como passar uma variável de ambiente e usá-la em seu script.

Seu script `myScript.sh` poderia ter a seguinte aparência:

```
#!/bin/bash
#PBS -N Test
#PBS -l ncpus=4
#PBS -l walltime=0:02:00
#PBS -l mem=5G

# Run python program, make sure you pass in $input as an env variable.
echo "Processing $input"
some_program.py $input
``` 

Envie-o desta forma:

```
$ qsub -v input=mygenome.fasta myScript.sh
``` 

A saída após o término da execução será:
```
Processing mygenome.fasta
```

Passando Multiplas Variáveis[¶](#passando-multiplas-variaveis "Permanent link")
---------------------------------------------------------------------------------

Se você precisar passar mais de uma variável, use uma lista de strings separada por vírgulas.

```
#!/bin/bash
#PBS -N Test
#PBS -l ncpus=4
#PBS -l walltime=0:02:00
#PBS -l mem=5G

# Run python program, make sure you pass in the required env variables!
echo "Processing $input with args $foo and $bar"
some_program.py $input $foo $bar
``` 

Envie-o conforme abaixo. As múltiplas variáveis precisam ser uma lista separada por vírgulas. Não pode haver espaços nessa lista.

```
$ qsub -v input=mygenome.fasta,foo=A,bar=20  myScript.sh
``` 

A saída após o término da execução será: 
```
Processing mygenome.fasta with args A and 20
```

Particularidades[¶](#particularidades "Permanent link")
-----------------------------------

É improvável que você use vírgulas em sua variável, mas se o fizer, há algumas peculiaridades das quais você precisa estar ciente. A página de manual explica corretamente que o valor deve ser colocado entre aspas simples ou duplas, e o par `<variable>=<value>` deve ser colocado entre aspas do tipo que não foi usado para colocar o valor.

Aqui está um exemplo:

```
$ qsub -v input=mygenome.fasta,"foo='A,B'",bar=20  myScript.sh
``` 

No entanto, descobri que o seguinte também funciona. Você também pode usar espaços nesse formato.

```
$ qsub -v "input=mygenome.fasta,foo='A,B',bar=20"  myScript.sh
$ qsub -v "input=mygenome.fasta, foo='A,B', bar=20"  myScript.sh
``` 
