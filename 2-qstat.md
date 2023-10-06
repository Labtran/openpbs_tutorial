Usando qstat
=============================================

Obtendo mais informações do Qstat
-------------------------------------------------------------------------------------------------

Você pode obter muito mais informações do qstat se usar suas opções de linha de comando. No cluster, digite `man qstat` para ler as páginas de manual e obter mais detalhes sobre o comando qstat. Abaixo está um exemplo de uso do qstat:

```
qstat [-f] [-J] [-p] [-t] [-x] [ job_identifier... | 
    destination... ]
qstat [-a|-i|-r|-H] [-J] [-t] [-u user] [-n] [-s] [-G|-M] [-R] [-1] [-w] [job_id... | 
    destination...]
qstat -Q [-f] [ destination... ]
qstat -q [-G|-M] [ destination... ]
qstat -B [-f] [ server_name... ]
qstat --version
``` 

Abaixo estão alguns exemplos de uso dessas opções de comando. Esses exemplos usam o "Formato padrão" e o "Formato alternativo" do qstat. Observe também que o qstat exibirá apenas seus próprios trabalhos. Exemplos de qstat usando o formato padrão

O comando qstat exibirá o status do trabalho no formato padrão quando as seguintes opções forem fornecidas:

```
qstat [-p] [-J] [-t] [-x] [ [job_identifier | destination] …]

(destination is any queue name)
-p A coluna Uso do tempo é substituída pela porcentagem concluída
-J Limita as informações de status às matrizes de trabalho.
-t Exibe informações de status de trabalhos, matrizes de trabalhos e subtrabalhos.
-x Exibe informações de status de trabalhos concluídos e movidos, além de trabalhos em execução e em fila.
``` 

No uso do Formato Padrão, os cabeçalhos das colunas serão:

```
Job id Name User Time Use S Queue
------ ---- ---- ---- --- - -----

$ qstat

Job id            Name             User           Time Use S Queue
----------------  ---------------- -------------  -------- - -----
6263.hpcnode0     fingerprint_li.sh  999999        8805:35 R workq           
6264.hpcnode0     fingerprint_lm.sh  999999              0 Q workq           
6266.hpcnode0     fingerprint_rt.sh  999999       386:38:3 R smallq          
6267.hpcnode0     fingerprint_rm.sh  999999       385:46:4 R smallq          
....
``` 

e

```
$ qstat -p

Job id            Name             User           % done  S Queue
----------------  ---------------- -------------  -------- - -----
6263.hpcnode0     fingerprint_li.sh  999999        90%     R workq           
6264.hpcnode0     fingerprint_lm.sh  999999              0 Q workq           
6266.hpcnode0     fingerprint_rt.sh  999999        40%     R smallq          
6267.hpcnode0     fingerprint_rm.sh  999999        35%     R smallq          
....
``` 

Exemplo de listagem de todos os seus trabalhos no smallq com a porcentagem concluída.

```
$ qstat -p smallq

Job id            Name             User           % done  S Queue
----------------  ---------------- -------------  -------- - -----
6266.hpcnode0     fingerprint_rt.sh  999999        40%      R smallq          
6267.hpcnode0     fingerprint_rm.sh  999999        35%      R smallq          
....
``` 

Exemplo de obtenção de informações completas sobre um trabalho usando -f e o job_id. Observação: se o trabalho já tiver sido concluído, será necessário adicionar um -x para mostrar os trabalhos expirados (por exemplo, qstat -fx 6944)

```
$ qstat -f 6944

Job Id: 6944.hpcnode0
Job_Name = fingerprint_li.sh
 ...
comment = Job run at Mon Feb 24 at 14:46 on (hpcnode3:mem=44040192kb:ncpus=48)
etime = Mon Feb 24 14:46:13 2014
Submit_arguments = fingerprint_li.sh
``` 

Exemplos de qstat usando o formato alternativo

O comando qstat exibirá o status do trabalho no formato alternativo se alguma das opções a seguir for fornecida:

```
-a [job_id] Todos os trabalhos em fila e em execução são exibidos. Deve estar antes de -n
-i [job] ou [dest] informações sobre trabalhos enfileirados, retidos ou em espera
-G Mostra o tamanho em gigabytes. É usado um formato alternativo.
-H Exibe informações de todos os trabalhos concluídos ou movidos.
-M Mostra o tamanho em megawords. Uma palavra é considerada como 8 bytes.
-n A cadeia de caracteres exec_host é listada
-r Se for fornecido um destino, serão exibidas informações sobre os trabalhos em execução ou suspensos nesse destino.
-s Qualquer comentário adicionado pelo administrador ou pelo agendador é mostrado.
-u lista_de_usuários
-T Exibe a hora de início estimada dos trabalhos em fila
```

No uso do formato alternativo, os cabeçalhos das colunas serão:

```
Req’d Req’d Elap
Job ID Username Queue Jobname SessID NDS TSK Memory Time S Time
—— ——- —- ——- —— — — —— —- - —-
``` 

O exemplo usando -a mostra todos os trabalhos em formato alternativo e o -n1 mostra o nó em que o programa está sendo executado.

```
$ qstat -an1 workq
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
69580.hpcnode0  111111   workq    hpc-hill2-  22234   1   8    5gb 120:0 R 23:47 hpcnode6/2*8
69581.hpcnode0  111111   workq    hpc-hill2-  22698   1   8    5gb 120:0 R 23:47 hpcnode6/3*8
```
