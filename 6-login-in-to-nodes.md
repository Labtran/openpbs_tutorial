 Login em outros nós[¶](#login-em-outros-nos "Permanent link")
=======================================================================

Em geral, não é possível fazer login diretamente via ssh nos outros nós do cluster de HPC. No entanto, você pode fazer login em qualquer nó enviando um trabalho interativo do PBS ou pode fazer login via ssh em qualquer nó em que tenha um trabalho em execução. O caso de uso é que talvez seja necessário fazer login no nó que está executando o trabalho para depurar um problema com o trabalho do PBS ou talvez seja necessário fazer login em um nó de GPU para compilar o código, se ele usar CUDA. Consulte também [Accessing GPU Nodes](../../gpu/gpu/). Os trabalhos interativos **não** são para executar cálculos longos. Por isso, o tempo máximo de permanência é de 8 horas.

Submetendo um PBS Job interativo[¶](#submetendo-um-PBS-Job-interativo "Permanent link")
-----------------------------------------------------------------------------------------

O envio de um trabalho interativo do PBS permitirá que você faça login em qualquer nó por um período de tempo específico. Use a opção `-I` (que significa Interactive) para solicitar uma sessão "Interactive". Isso o colocará em um nó disponível com uma alocação de 1 núcleo de CPU e uma quantidade padrão de RAM e tempo de espera.

```
$ qsub -I
hpcnode03
``` 

Você também pode usar a opção `-l` (que significa lista de recursos) para especificar o host ou os recursos necessários. Por exemplo, o comando abaixo o colocará no _hpcnode07_ com uma alocação de 1 núcleo de CPU e uma quantidade padrão de RAM por 30 minutos de tempo de espera. Você pode usar isso apenas para fazer login nesse nó e monitorar um grande trabalho em execução nele.
```
$ qsub -I -l select=1:host=hpcnode07 -l walltime=00:30:00
hpcnode07 $
```
 
Se precisar de mais recursos, você pode especificar o número de cpus e de memória. O comando abaixo o colocará em um nó e alocará 4 núcleos, 120 GB de RAM e um tempo de espera de 30 minutos.

```
$ qsub -I -l select=1:ncpus=4:mem=120G -l walltime=00:30:00
hpcnode05
``` 

Para alguns nós que têm filas restritas, você também precisará adicionar o nome da fila ao comando.

```
$ qsub -I -l select=1:host=c3node03 -l walltime=00:30:00 -q c3b
c3node03
``` 

Digite `exit` para sair do shell interativo no host de execução. Você será levado de volta ao nó de login.

Você verá que usei uma especificação `"select=1"` nos exemplos acima. É assim que você especifica um _chunk_ de recursos _consumíveis_. Em seguida, você segue essa instrução select com uma lista de cada recurso _consumível_ separado por dois pontos. Qualquer tempo de parede precisa ser especificado com uma opção `-l` separada, pois é um recurso _não consumível_. Não é possível colocar recursos consumíveis e não consumíveis em uma única opção `-l`.

Note

Os logins interativos lhe fornecerão recursos da "fila interativa". Isso é limitado em tempo de execução. 
Veja [https://hpc.research.uts.edu.au/status/](https://hpc.research.uts.edu.au/status/)  
Para testar, use um conjunto mínimo de dados de teste e o mínimo de núcleos, memória e tempo.

Fazer login em um nó que está executando seu job[¶](#fazer-login-em-um-no-que-esta-executando-seu-job "Permanent link")
-----------------------------------------------------------------------------------------------------------------

Nesse caso, você não precisa enviar um trabalho interativo do PBS. Basta usar o ssh no nó. Primeiro, verifique em qual nó seu trabalho está sendo executado:

```
$ qstat -n1
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
215426.hpcnode0 876543   smallq   primes_job  95025   1   1    5gb 00:10 R 00:00 hpcnode09/1
``` 

Neste exemplo, ele está sendo executado no `hpcnode09` e, portanto, podemos usar o ssh diretamente nesse nó.

```
$ ssh hpcnode09
Last login: Wed Jul 17 16:19:13 2019 from hpcnode01
hpcnode09
``` 

Observação: quando o trabalho terminar nesse nó, sua conexão ssh com esse nó será automaticamente desconectada.

Se você entrar por ssh em um nó e não tiver um trabalho em execução nesse nó, em alguns segundos você será desconectado e receberá a mensagem "Connection to ..... closed.".
