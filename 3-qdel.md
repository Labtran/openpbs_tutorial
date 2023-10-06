Usando qdel
===========================================

Para Deletar ou Cancelar seu Job
-------------------------------------------------------------------------------

Descubra o número do trabalho no qstat:

```
$ qstat -an1
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
69580.hpcnode0  111111   workq    bigprimes   22234   1   8    5gb 120:0 R 23:47 hpcnode6/2*8
69581.hpcnode0  111111   workq    bigprimes   22698   1   8    5gb 120:0 R 23:47 hpcnode6/3*8
``` 

Em seguida, exclua seu trabalho da fila usando o comando qdel: `qdel job_id`.  
Exemplo:

```
$ qdel 69580.hpcnode0
``` 

Se o seu trabalho for um trabalho de matriz, especifique a ID do trabalho de matriz ou a ID do subtrabalho, por exemplo:

```
$ qdel 28846[].hpcnode0
``` 

or

```
$ qdel 28846[5].hpcnode0
``` 

Type `man qdel` for the online manual pages.
