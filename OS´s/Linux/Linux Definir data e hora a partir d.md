# 1 - Linux Definir data e hora a partir de um prompt de comando

Defenindo a data e a hora do sistema a partir do prompt de comando (bash shell) é possivel?
O servidor linux não tem ambiente GUI (Graphical User Interfase), tenho acesso apenas via SSH. 
Como definir a data e hora do meu Sistema Operacional?

Use o comando date para exibir a data e hora atuais ou definir a data/hora do sistema pela sessão ssh. Você também pode executar o comando date do terminal X como usuário root. Você deve efetuar login como usuário root para usar o comando date. Isso é útil se a hora e/ou data do servidor Linux estiver errada, e você precisar defini-la para novos valores no prompt do shell.

Detalhes do tutorial
* Nível de dificuldade: **Fácil**
* Privilégios de root: **Sim**
* Requisitos: **Terminal Linux**
* Categoria: **Gestão de sistemas**
* Pré-requisitos: **comando de data**
* Compatibilidade do sistema operacional: **AlmaLinux • Alpine • Arch • Debian • Fedora • Mint • openSUSE • Pop!_OS • RHEL • Rocky • Stream • SUSE • Ubuntu • WSL**
* Tempo estimado de leitura: **5 minutos**

## 1.1 - Linux exibe data e hora atuais

Basta digitar o comando date:
###> # date

Exemplos de saída:
```> # Seg Jan 21 01:31:40 IST 2019```

## 1.2 - Linux exibe o relógio de hardware (RTC)

Digite o seguinte comando hwclock para ler o relógio do hardware e exibir a hora na tela:
###> # hwclock -r
```> # 2024-11-12 23:45:14.004211-03:00```

### OU
###> # hwclock --show
```> # 2024-11-12 23:45:54.229175-03:00```

### OU
###> # hwclock --show --verbose
```
hwclock from util-linux 2.36.1
System Time: 1731466012.158027
Trying to open: /dev/rtc0
Using the rtc interface to the clock.
Last drift adjustment done at 1663648625 seconds after 1969
Last calibration done at 1663648625 seconds after 1969
Hardware clock is on UTC time
Assuming hardware clock is kept in UTC time.
Waiting for clock tick...
...got clock tick
Time read from Hardware Clock: 2024/11/13 02:46:52
Hw clock time : 2024/11/13 02:46:52 = 1731466012 seconds since 1969
Time since last adjustment is 67817387 seconds
Calculated Hardware Clock drift is 0.000000 seconds
2024-11-12 23:46:51.806373-03:00
```

### OU 
Mostre-o em Tempo Universal Coordenado (UTC):
###> # hwclock --show --utc
```2024-11-12 23:47:53.619835-03:00```

---
## Comando `date`
Sintaxe para definir nova data e hora: **`date --set="STRING"`**

Exemplos de datas:
Defina nova data para `2 de outubro de 2006 18:00:00`, digite o seguinte comando como usuário root:

```###> # date -s "2 OCT 2006 18:00:00"```

### OU 
```###> # date --set="2 OCT 2006 18:00:00"```

Exemplos de hora:
Você também pode simplificar o formato usando a seguinte sintaxe:
```###> # date +%Y%m%d -s "20081128"```

Definindo a hora, usando a seguinte sintaxe:
```###> # date +%T -s "10:13:13"```

Onde,
```
10: Hora (hh)
13: Minuto (mm)
13: Segundo (ss)

Use %po equivalente local de AM ou PM, digite:
date +"%T%p" -s "6:10:30AM"
date +"%T%p" -s "12:10:30PM"
```
---

## Como faço para configurar o relógio do hardware?
Use a seguinte sintaxe para definir o relógio de hardware a partir do relógio do sistema e atualizar os timestamps no arquivo /etc/adjtime. 
Por exemplo:
```###> # hwclock --systohc```

### OU
```###> # hwclock -w```

Definindo o relógio do sistema a partir do relógio do hardware? Tente:
```###> # hwclock --hctosys```

### OU
```###> # hwclock -s```

---
## Uma nota sobre o sistema Linux baseado em systemd
```
Com o Sistema Linux baseado em systemd, você precisa usar o comando timedatectl para definir ou visualizar a data e hora atuais. 

A maioria das distros modernas, como RHEL/CentOS v.7.x+, Fedora Linux, Debian, Ubuntu, Arch Linux e outros sistemas baseados em systemd, precisam do utilitário timedatectl. 

Observe que o comando acima deve funcionar em sistemas modernos também.

Como sei que estou usando systemd ou sys v init ou OpenRC como um sistema init no Linux?

Execute o comando type ou command command para ver o que ele diz. 
Por exemplo:
> # type systemd

Você obtém alguma saída? 
Aqui está o que o Ubuntu 20.04 LTS retornou:


systemd é hash (/usr/bin/systemd)
Em outras palavras, estou usando systemd como init. O seguinte erro do Alpine Linux versão 3.18 indica que não estou usando distro Linux baseada em systemd para init:

-bash: tipo: systemd: não encontrado
Você pode ver a versão do systemd digitando o seguinte comando systemctl:
systemctl --version

Saídas:


sistemad 253 (253.4-1-arquitetura)
+PAM +AUDITORIA -SELINUX -APPARMOR -IMA +SMACK +SECCOMP +GCRYPT +GNUTLS +OPENSSL +ACL +BLKID +CURL +ELFUTILS +FIDO2 +IDN2 -IDN +IPTC +KMOD +LIBCRYPTSETUP +LIBFDISK +PCRE2 -PWQUALITY +P11KIT -QRENCODE +TPM2 +BZIP2 +LZ4 +XZ +ZLIB +ZSTD +BPF_FRAMEWORK +XKBCOMMON +UTMP -SYSVINIT hierarquia-padrão=unificado

OBSERVAÇÃO: Conforme explicado anteriormente, o comando timedatectl só funciona quando você confirma o uso de uma distribuição Linux baseada no sistema.
```

---

## Usando o comando timedatectl exibe a data e hora atuais
Digite: `timedatectl`

`Saída na console`
```
> # timedatectl
               Local time: Wed 2024-11-13 01:59:11 -03
           Universal time: Wed 2024-11-13 04:59:11 UTC
                 RTC time: Wed 2024-11-13 04:59:11
                Time zone: America/Fortaleza (-03, -0300)
System clock synchronized: no
              NTP service: n/a
          RTC in local TZ: no
```

Como altero a data atual usando o comando timedatectl?
Para alterar a data atual, digite o seguinte comando como usuário root:

```> # sudo timedatectl set-time YYYY-MM-DD```

Por exemplo, defina a data atual como 2015-12-01 (1º de dezembro de 2015): 

Exemplos de saída:
timedatectl set-time '2015-12-01'
timedatectl

`Saída na console`
```
 Hora local: terça-feira, 01/12/2015, 00:00:03 EST
  Horário universal: Ter 2015-12-01 05:00:03 UTC
        Horário RTC: Ter 2015-12-01 05:00:03
       Fuso horário: América/Nova_Iorque (EST, -0500)
     NTP habilitado: não
NTP sincronizado: não
 RTC em TZ local: não
      DST ativo: não
 Última alteração do horário de verão: o horário de verão terminou em
                  Dom 2015-11-01 01:59:59 EDT
                  Dom 2015-11-01 01:00:00 EST
 Próxima mudança de horário de verão: o horário de verão começa (o relógio avança uma hora) às
                  Dom 2016-03-13 01:59:59 EST
                  Dom 2016-03-13 03:00:00 EDT
Para alterar a data e a hora, use a seguinte sintaxe:
timedatectl set-time YYYY-MM-DD HH:MM:SS

Onde,

HH:Uma hora.
MM :Um minuto.
SS: Um segundo, todo digitado em formato de dois dígitos.
YYYY: Um ano de quatro dígitos.
MM: Um mês de dois dígitos.
DD: Um dia do mês com dois dígitos.
```

Por exemplos:

Definindo a data para '13 de novembro de 2024' e a hora como '2:20:03 am', digite:

```> # timedatectl set-time '2024-11-13 02:20:03'```
```> # date```
```Wed 13 Nov 2024 02:20:03 AM -03```

Como defino apenas a hora atual?
```> #  timedatectl set-time '02:30:10'```
```> # date```
```Wed 13 Nov 2024 02:30:11 AM -03```


### Como defino o fuso horário usando o comando timedatectl?
Para ver a lista de todos os fusos horários disponíveis, digite:
```> # timedatectl list-timezones```

Lista todas as timezones 
```> # timedatectl list-timezones | more```

Lista as timezones filtrando a saída por **america**
```> # timedatectl list-timezones | grep -i america```

Lista a timezone filtrando a saída por **america/for**
```> # timedatectl list-timezones | grep America/For```

Definir o fuso horário como 'America/Fortaleza', digite: 
```> # timedatectl set-timezone 'America/Fortaleza'```

Verifique: Saídas:
```> # timedatectl```

```
         Hora local: segunda-feira, 23/11/2015 08:17:04 IST
  Horário universal: Seg 2015-11-23 02:47:04 UTC
        Horário RTC: Seg 2015-11-23 13:16:09
       Fuso horário: Ásia/Kolkata (IST, +0530)
     NTP habilitado: não
   NTP sincronizado: não
    RTC em TZ local: não
          DST ativo: n/a
```

### Como sincronizo o relógio do sistema com um servidor remoto usando NTP?
Basta digitar o seguinte comando:
```> # timedatectl set-ntp yes```

Verifique:
```> # timedatectl```

Saídas de exemplo:
```
         Hora local: segunda-feira, 23/11/2015 08:18:49 IST
  Horário universal: Seg 2015-11-23 02:48:49 UTC
        Horário RTC: Seg 2015-11-23 02:48:50
       Fuso horário: Ásia/Kolkata (IST, +0530)
     NTP habilitado: sim
   NTP sincronizado: sim
    RTC em TZ local: não
          DST ativo: n/a
```

### Conclusão
Usuários Linux podem usar o comando date para imprimir ou definir a data e hora do sistema. Assim como também SO´s baseados em Systemd podem usar timedatectl para controlar a hora e data do sistema. Você também pode definir um novo timzone usando a linha de comando Linux . 
Leia as seguintes páginas do manual usando o comando man ou o comando help :

```> # man 8 hwclock```
```> # man 1 date```
```> # man 8 timedatectl```
```> # timedatectl --help | grep -Ew -- 'timezone|status'```


##### **por m4v0 (mavo) em 13/11/2024 02:45:00**
