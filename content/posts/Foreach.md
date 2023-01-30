---
title: "Where-Object"
date: 2023-01-30T11:23:50+01:00
draft: false
toc: false
images:
tags:
  - Powershell
  - dbatools
---

Jeg hadde behov for å finne alle databaser som hadde et view med navnet "Actor".

I slike tilfeller har jeg min "go-to" modul i Powershell - dbatools.

Først setter jeg opp noen variabler

```
$sqlsrv = "nn-sql-02"
$sqlcred = Get-Credential -UserName visma
```

Sjekker at det er kontakt med server

```
Get-DbaDatabase -SqlInstance $sqlsrv -SqlCredential $sqlcred -Database F0805
```

Svaret fra server er positivt
```
ComputerName       : NN-SQL-02
InstanceName       : MSSQLSERVER
SqlInstance        : NN-SQL-02
Name               : F0805
Status             : Normal
IsAccessible       : True
```
Jeg kunne også benyttet funksjonen ``Connect-DbaInstance`` til dette formålet.

Jeg kan nå gå videre for å finne viewet jeg søker etter
```
$views = Find-DbaView -SqlInstance $sqlsrv -SqlCredential $sqlcred -Pattern 'actor' | Where-Object {$_.Name -like "Actor"} | Select-Object -Property Database, Name | ft -AutoSize
```
Jeg bruker her funksjonen ``Where-Object`` for å finne alle views med navn "Actor"

Ved å benytte ``$views`` finner jeg at det er kun 1 database, av totalt 103 databaser som har dette viewet.

```
Database Name
-------- ----
F0005    Actor
```
