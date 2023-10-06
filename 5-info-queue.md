Obtendo informações sobre as filas[¶](#obtenha-informacoes-sobre-filas "Permanent link")
=========================================================================================

Você pode obter um resumo atualizado dos nós, filas e trabalhos visitando a página [HPC Status](/status/). No entanto, talvez você queira obter informações mais detalhadas para usar em seus scripts de trabalho. Você pode fazer isso usando o comando `qstat`.

Obtenção de informações sobre as filas disponíveis[¶](#obtenha-sobre-disponibilidade-filas "Permanent link")
-----------------------------------------------------------------------------------------------------------------

Para obter uma lista detalhada das filas e seus limites, use o `qstat`.

Há algumas filas de trabalho diferentes no HPC, smallq e workq são dois exemplos, e elas têm diferentes limitações de recursos. Para obter uma lista de todas as filas, execute o comando abaixo. Neste exemplo, você pode ver que há 28 trabalhos em execução na fila smallq, 5 trabalhos em execução na workq e 3 trabalhos enfileirados na workq.

```
$ qstat -Q

Queue        Max   Tot Ena Str   Que   Run   Hld   Wat   Trn   Ext Type
---------- ----- ----- --- --- ----- ----- ----- ----- ----- ----- ----
smallq         0    28 yes yes     0    28     0     0     0     0 Exec
expressq       0     0 yes yes     0     0     0     0     0     0 Exec
workq          0     8 yes yes     3     5     0     0     0     0 Exec
``` 

Para obter informações completas sobre o `smallq`, por exemplo, use o comando abaixo. Essa é a melhor maneira de obter informações atualizadas sobre as filas disponíveis, pois podemos modificar os limites máximos das filas para gerenciar os recursos.

```
$ qstat -Qf smallq

Queue: smallq
queue_type = Execution
total_jobs = 28
state_count = Transit:0 Queued:0 Held:0 Waiting:0 Running:28 Exiting:0 Begun:0 
resources_max.mem = 32gb ⇐ The most memory you can request 
resources_max.ncpus = 2 ⇐ The most CPUs you can request 
resources_max.walltime = 200:00:00
resources_default.walltime = 12:00:00
resources_assigned.mem = 101711872kb
resources_assigned.ncpus = 56
resources_assigned.nodect = 28
``` 

Para obter informações completas sobre todas as filas, incluindo seus cpus máximos, memória e tempos de espera, execute o comando abaixo.

```
$ qstat -Qf
```
