Obtendo informações sobre os nós
=======================================================================================

Você pode obter um resumo atualizado dos nós, filas e trabalhos visitando a página [HPC Status](/status/). No entanto, talvez você queira obter informações mais detalhadas para usar em seus scripts de trabalho. Você pode fazer isso usando o comando `pbsnodes`.

As páginas de manual do pbsnodes podem ser visualizadas com `man pbsnodes` e a execução do `pbsnodes` sem opções mostrará um resumo de uso.

Obtenção de informações sobre os nós disponíveis
---------------------------------------------------------------------------------------------------------------

Use o comando `pbsnodes -a` para consultar o status de todos os nós, mostrando o número de CPUs e a quantidade de memória em cada nó.

```
$ pbsnodes -a
``` 

Para consultar apenas um nó, forneça o nome do nó como uma opção. Abaixo está um exemplo de como consultar o hpcnode03. Algumas informações não essenciais foram omitidas.

```
$ pbsnodes hpcnode03
hpcnode03
  Mom = hpcnode03
  state = free
  pcpus = 56
  jobs = 110298.hpcnode0/0, 110298.hpcnode0/1, 110298.hpcnode0/2, 110298.hpcnode0/3, etc... 
  resources_available.host = hpcnode03
  resources_available.intel_node = yes
  resources_available.mem = 384000000kb
  resources_available.ncpus = 56
  resources_available.Qlist = normal
  resources_available.vnode = hpcnode03
  resources_assigned.hbmem = 0kb
  resources_assigned.mem = 278921216kb
  resources_assigned.ncpus = 32
  resources_assigned.vmem = 0kb
```

Obtenção de saída analisável por máquina
-----------------------------------------------------------------------------------------

Essas informações podem ser usadas para ajudá-lo a automatizar o envio de trabalhos do PBS. Nesse caso, talvez você queira usar uma saída mais estruturada, pois há bons analisadores Python para dados estruturados, como JSON (JavaScript Object Notation). Para imprimir a saída como JSON, use a opção `-F json` conforme abaixo.

```
$ pbsnodes -F json hpcnode03 
{
    "timestamp":1606173513,
    "pbs_version":"19.2.6.20200311140837",
    "pbs_server":"pbsserver",
    "nodes":{
        "hpcnode03":{
            "Mom":"hpcnode03",
            "ntype":"PBS",
            "state":"free",
            "pcpus":56,
            "jobs":[
                "110298.hpcnode0",
                "110609.hpcnode0"
            ],
            "resources_available":{
                "arch":"linux",
                "centos6_node":"no",
                "host":"hpcnode03",
                "intel_node":"yes",
                "mem":"384000000kb",
                "ncpus":56,
                "Qlist":"normal",
                "vnode":"hpcnode03"
            },
            "resources_assigned":{
                "mem":"278921216kb",
                "ncpus":32
            },
            "resv_enable":"True",
            "sharing":"default_shared",
            "last_state_change_time":1606093393,
            "last_used_time":1606111932
        }
    }
}
``` 

Se você desejar que a saída esteja no formato de valor separado por delimitador, use a opção `-F dsv`. O delimitador padrão é uma barra vertical ("|").

```
$ pbsnodes -F dsv hpcnode03
``` 

Uma especificação de delimitador opcional pode ser usada para alterar isso.

```
$ pbsnodes -F dsv -D ',' hpcnode03
``` 

Você também pode usar as opções acima e consultar todos os nós.

```
$ pbsnodes -F json -a
```
