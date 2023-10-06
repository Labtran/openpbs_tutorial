Using qstat
=============================================

Getting More Information out of Qstat
-------------------------------------------------------------------------------------------------

You can gain a lot more information from qstat if you use its command line options. On the cluster type `man qstat` to read the man pages for further details on the qstat command. Below is a consise qstat usage:

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

Below are some examples of using these command options. These examples use qstats “Default Format” and “Alternate Format”. Also note that qstat will only display your own jobs. Examples of qstat using Default Format

The qstat command will display job status in default format when the following options are given:

```
qstat [-p] [-J] [-t] [-x] [ [job_identifier | destination] …]

(destination is any queue name)
-p The Time Use column is replaced with the percentage completed
-J Limits status information to job arrays.
-t Displays status information for jobs, job arrays and subjobs.
-x Displays status information for finished and moved jobs in addition to running and queued jobs.
``` 

In Default Format usage the column headers will be:

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

and

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

Example of listing all your jobs in the smallq with percentage completed.

```
$ qstat -p smallq

Job id            Name             User           % done  S Queue
----------------  ---------------- -------------  -------- - -----
6266.hpcnode0     fingerprint_rt.sh  999999        40%      R smallq          
6267.hpcnode0     fingerprint_rm.sh  999999        35%      R smallq          
....
``` 

Example of obtaining complete information on a job by using -f and the job_id. Note: if the job has already finished you will need to add a -x to show expired jobs (e.g. qstat -fx 6944)

```
$ qstat -f 6944

Job Id: 6944.hpcnode0
Job_Name = fingerprint_li.sh
 ...
comment = Job run at Mon Feb 24 at 14:46 on (hpcnode3:mem=44040192kb:ncpus=48)
etime = Mon Feb 24 14:46:13 2014
Submit_arguments = fingerprint_li.sh
``` 

Examples of qstat using Alternate Format

The qstat command will display job status in the alternate format if any of the following options are given:

`-a [job_id] All queued and running jobs are displayed. Must be before -n
-i [job] or [dest] information for queued, held or waiting jobs
-G Show size in gigabytes. Alternate format is used.
-H Displays information for all finished or moved jobs.
-M Show size in megawords. A word is considered to be 8 bytes.
-n The exec_host string is listed
-r If a destination is given, information for running or suspended jobs at that destination is displayed.
-s Any comment added by the administrator or scheduler is shown.
-u user_list
-T Displays estimated start time for queued jobs` 

In Alternate Format usage the column headers will be:

`Req’d Req’d Elap
Job ID Username Queue Jobname SessID NDS TSK Memory Time S Time
—— ——- —- ——- —— — — —— —- - —-` 

Example using -a shows all jobs in alternate format and the -n1 shows the node that the program is executing on.

```
$ qstat -an1 workq
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
69580.hpcnode0  111111   workq    hpc-hill2-  22234   1   8    5gb 120:0 R 23:47 hpcnode6/2*8
69581.hpcnode0  111111   workq    hpc-hill2-  22698   1   8    5gb 120:0 R 23:47 hpcnode6/3*8
```
