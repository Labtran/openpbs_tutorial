Escreva PBS Job Scripts
=====================================================================

Nesta seção, copiaremos alguns scripts de exemplo em nosso próprio diretório e enviaremos um trabalho para o agendador do PBS. O resultado final é que teremos calculado alguns números primos. Este exemplo também usará o diretório /scratch para ler e gravar nossos arquivos. Você deve sempre usar o diretório "scratch" para arquivos grandes.

Observação

É importante lembrar de algumas coisas:
  1. Não execute cálculos grandes no nó de login, use o PBS.
  2. Use o diretório /scratch/ para ler e gravar arquivos grandes.

O nó de login serve para que você possa fazer login, editar seu código, compilá-lo e, talvez, executar testes, usando um pequeno conjunto de dados de teste. Seu trabalho computacional real precisa ser executado usando um script de envio do PBS para que possa ser distribuído a um dos nós de computação dedicados.
Esta página explica como você pode fazer isso.

Resumo da execução de um job
-----------------------------------------------------------------------

* Determinar os recursos necessários para seu trabalho.
* Crie um script de trabalho, que envolve seu trabalho em um script de shell, informando ao PBS seus requisitos.
* Enviar o trabalho usando o qsub.
* Monitorar o trabalho usando o qstat.
* Excluir o trabalho, se necessário, usando o qdel.

Nunca execute programas grandes ou conjuntos de dados grandes diretamente no nó principal do cluster; use o PBS para agendar seu trabalho na fila. Isso garante a alocação eficiente de recursos para todos. Se você precisar testar um script, execute um conjunto menor de dados de teste, de preferência por meio do PBS.

Copie os scripts de exemplo para seu próprio diretório
-------------------------------------------------------------------------------------------------------------------

Os scripts de exemplo que você pode usar para praticar o envio de alguns trabalhos de teste curtos estão em `/shared/eresearch/pbs_job_examples/`.

Copie-os em seu próprio diretório usando os seguintes comandos:

```
$ cd            <-- Isso o levará ao topo do seu diretório pessoal.
$ mkdir jobs    <-- Isso cria o diretório "jobs".
$ cd jobs       <-- Isso muda para o diretório "jobs".
``` 

Agora você deve estar no novo diretório `jobs` e podemos copiar para lá os programas primes de exemplo. 
**No comando abaixo, não se esqueça do ponto no final.**

```
$ cp -r /shared/eresearch/pbs_job_examples/primes .
``` 

Agora, mude o diretório para o novo diretório "primes".

```
$ cd primes
``` 

Isso terá copiado recursivamente o diretório primes e seu conteúdo para o seu próprio diretório. Agora você estará no diretório "primes" e poderá dar uma olhada nos scripts que estão lá.

Para visualizar um arquivo como `primes.py` ou `primes_job.sh`, use o comando less `less primes.py`. Pressionar a barra de espaço move a página para baixo e pressionar a tecla `q` encerra o visualizador.

Determinar os requisitos de recursos
---------------------------------------------------------------------------------------------

Para fazer uso efetivo do sistema de filas do PBS, você precisará saber a quantidade de recursos que o seu trabalho usará. Quando o trabalho for iniciado, o PBS se certificará de que os recursos apropriados estejam disponíveis para que o trabalho seja executado até o máximo especificado.
The resources can be specified by:

* Núcleos de CPU - Se o seu aplicativo for multithread e puder usar mais de um núcleo, você precisará especificar o número de núcleos que o script usará.
    
* Memória - Esta é a quantidade de memória que seu aplicativo usará. Em um novo software ou conjunto de dados, talvez você não saiba o quanto será consumido. Nesse caso, comece com um número generoso e ajuste para baixo. Quanto mais preciso você for, maior será a probabilidade de seu trabalho ser agendado durante os períodos de maior movimento, em que há pequenas quantidades de memória disponíveis.
    
* Walltime - Esse é o tempo máximo pelo qual você deseja que o programa seja executado e, após esse tempo, o agendador do PBS _mata_ o trabalho. Comece estimando um tempo de parede generoso com base em execuções de teste e ajuste para baixo. Quanto menor o tempo de parede, maior a probabilidade de o trabalho ser agendado durante os períodos de maior movimento.

For the example `primes.py` program to calculate primes from **100,000** to **300,000** we will use 1 CPU, 5 GB RAM and set a wall time of 5 minutes.

Criar um script de Job
-------------------------------------------------------------

Seu script de trabalho configura os recursos de HPC que queremos que o PBS reserve para o nosso trabalho. Ele deve conter o seguinte:

* Um nome para o trabalho, para que você possa reconhecê-lo na lista qstat.
* Seus requisitos de recursos para que o PBS programe o trabalho - isso precisa estar no topo do script para que o PBS o leia, antes da primeira linha executável do script.
* Qualquer cópia de dados, configuração de diretórios de trabalho e outra administração pré-trabalho que precise ser realizada
* O trabalho em si
* Limpeza de dados temporários, cópia de dados para um diretório de longo prazo e outra administração pós-trabalho

Dê uma olhada no script de trabalho chamado `submit_typical.sh` do diretório de exemplos `/shared/eresearch/pbs_job_examples/primes/`. Ele é bem comentado.

Uma versão resumida, com menos comentários, é mostrada abaixo. Aqui, esse trabalho está solicitando 4 núcleos e até 30 minutos para ser concluído, portanto, especificamos um tempo de parede de 40 minutos para garantir que ele será concluído com o tempo de parede.

```
#!/bin/bash

#PBS -N primes
#PBS -l ncpus=4
#PBS -l mem=20gb
#PBS -l walltime=00:40:00

# Create a unique /scratch directory.
SCRATCH="/scratch/${USER}_${PBS_JOBID%.*}"
mkdir  ${SCRATCH}

# Change to the PBS working directory where qsub was started from.
cd  ${PBS_O_WORKDIR}

# Copy your input data to this scratch directory.
cp  input.dat  ${SCRATCH}

# Change directory to the scratch directory and run your program.
# my_program uses input.dat and creates an output file called output.dat
cd  ${SCRATCH}
/full_path_name/my_program

# Copy output results back to your working directory. 
mv  ${SCRATCH}/output.dat  ${PBS_O_WORKDIR}/

# Clean up
cd  ${PBS_O_WORKDIR}
rm  ${SCRATCH}/input.dat
rmdir  ${SCRATCH}
``` 

Submit your job
-----------------------------------------------------

Aqui enviamos nosso trabalho para a fila. Digite `man qsub` para as páginas de manual on-line.

```
$ qsub submit_typical.sh.sh
11153.hpcnode0
``` 

O Qsub retornará o ID do trabalho atribuído. Normalmente, trata-se de um número, seguido do nome do servidor do qual você enviou o trabalho. Você pode simplesmente se referir ao número no lugar do ID completo do trabalho.

Monitorar o status do seu job e listar jobs
-------------------------------------------------------------------------------------------------

Abaixo está um exemplo da saída que você verá. Digite `man qstat` para acessar as páginas do manual on-line.

```
$ qstat
Job id            Name             User            Time Use S Queue
----------------  ---------------- --------------  -------- - -----
211.hpcnode0      scaffold.build.  110234          570:36:5 R workq
235.hpcnode0      Histomonas.map.  100123                 0 Q workq
236.hpcnode0      run_job.sh       999777                 0 Q workq
```

Name é o nome do seu script enviado. User é o seu número de usuário da equipe da UTS. Time é o tempo de CPU usado. A coluna S indica o estado do trabalho, como na tabela abaixo:

```
Q : Job is queued.
R : Job is running.
E : Job is exiting after having run.
F : Job is finished.
H : Job is held.
S : Job is suspended.
``` 

A fila será workq, a menos que você tenha especificado outra fila para usar em seu script de envio de trabalho.

Mais informações podem ser listadas usando as opções de linha de comando do qstat, como **-n1**, que mostra o nó em que o programa está sendo executado.

```
$ qstat -n1 
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
69580.hpcnode0  111111   workq    primes      22234   1   8    5gb 120:0 R 23:47 hpcnode6/2*8
69581.hpcnode0  111111   workq    primes      22698   1   8    5gb 120:0 R 23:47 hpcnode6/3*8
$
``` 

Para listar seus trabalhos concluídos, use **-x** (para expirado). Assim, por exemplo:

```
$ qstat -x 
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
1152.hpcnode0   999777     workq  primes      56678   1   1  250gb   --  F 00:09
``` 

Para excluir ou cancelar seu job
-------------------------------------------------------------------------------

Para excluir seu trabalho da fila, use o comando qdel:

```
$ qdel job_id

e.g. "qdel 69580.hpcnode0"
``` 

Digite `man qdel` para obter as páginas do manual on-line.

Para obter informações detalhadas sobre seu trabalho
-----------------------------------------------------------------------------------------------------

Para exibir detalhes de um trabalho específico, use `qstat -f job_id`. Por exemplo, para o trabalho "primes", use:

```
$ qstat -f 1152.hpcnode0
Job Id: 1152.hpcnode0
    Job_Name = primes
    Job_Owner = 999777@hpcnode0
    resources_used.cpupercent = 0
    resources_used.cput = 00:01:20
    resources_used.mem = 3220kb
    resources_used.ncpus = 1
    resources_used.vmem = 315200kb
    resources_used.walltime = 00:01:59
    job_state = R
    queue = workq
    Error_Path = hpcnode0:/shared/homes/999777/jobs/primes/primes.e1152
    exec_host = hpc2/0
    Mail_Points = abe
    Mail_Users = 999777@uts.edu.au
    Output_Path = hpcnode0:/shared/homes/999777/jobs/primes/primes.o1152
    Rerunable = True
    Resource_List.mem = 250gb
    Resource_List.ncpus = 1
    Resource_List.nodect = 1
    Resource_List.place = pack
    Resource_List.select = 1:mem=250gb:ncpus=1:vmem=250gb
    Resource_List.vmem = 250gb
    stime = Wed Apr 10 15:25:50 2013
    jobdir = /shared/homes/999777
    Variable_List = PBS_O_SYSTEM=Linux,PBS_O_SHELL=/bin/bash,
    PBS_O_HOME=/shared/homes/999777,PBS_O_LOGNAME=999777,
    PBS_O_WORKDIR=/shared/homes/999777/jobs/primes/primes,
    PBS_O_LANG=en_US.UTF-8,
    PBS_O_PATH=/usr/local/bin:/bin:/usr/bin:/bin,
    PBS_O_MAIL=/var/spool/mail/999777,PBS_O_QUEUE=workq,PBS_O_HOST=hpcnode0
    comment = Job run at Wed Apr 10 at 15:25 on (hpc2:mem=262144000kb:ncpus=1)
    etime = Wed Apr 10 15:25:50 2013
    Submit_arguments = primes
``` 

Finalização
-----------------------------------------------

Uma cópia da saída dos fluxos **stdout** e **stderr** do seu trabalho do PBS é criada no diretório a partir do qual você chamou o PBS como arquivos denominados `*.e` e `*.o` com o `job_id` anexado.

Um exemplo do que o programa `primes.py` e o número de trabalho 1152 produziriam é:

`primes.e1152` \ - deve ser sempre de tamanho zero, ou seja, vazio, pois contém todos os erros que seu programa possa ter produzido.

`primes.o1152` \ - conterá qualquer saída de tela que o seu programa tenha produzido.
