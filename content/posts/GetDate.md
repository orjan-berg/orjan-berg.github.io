---
title: "Get-Date"
date: 2023-03-14T12:05:14+01:00
draft: true
toc: false
images:
tags:
  - Powershell
---

Ofte har man behov for å filtrere data, slik som jeg når jeg skulle finne records i en sql tabell som var endret etter en gitt dato.

Jeg liker å bruke Powershell og spesielt modulen dbatools. Dette ble derfor mine foretrukne verktøy til å løse denne oppgaven.

Har Powershell en cmdlet for å arbeide med datoer?

For å finne ut av dette benyttet jeg ``Get-Command -CommandType -Name *date*``

Powershell kom med en rekke forslag,

![Example image](docs/images/GetDate01.png)

Av disse så fant jeg at ``Get-Date`` kunne gjøre det jeg var ute etter.

``Get-Date`` har properties for å angi Day, Month og Year. Disse brukte jeg for å angi datoen jeg ville ha.

``$chdt = Get-Date -Year 2022 -Month 11 -Day 17`` etter å ha satt variabelen $chdt slik jeg ville, var det på tide å hente fram dbtools.

Dbtools har funksjonen ``Invoke-DbaQuery``.

``invoke-DbaQuery -SqlInstance srv-prod-sql01 -Database VDC_SystemDB -query 'select * from VWUser'`` henter ut alle records i tabellen VWUser. Jeg er ikke ute etter alle felter. Jeg piper derfor resultatet til ``Select-Object``, på denne måten ``Select-Object -Property UserId, Username, LastUpdate``.

Nå har jeg de feltene jeg trenger, men fortsatt har jeg alt for mange records. Jeg piper derfor resultatet videre til ``Where-Object``. ``Where-Object {$_.LastUpdate.Date -ge $chdt}``

Helt til slutt vil jeg ha resultatet lagret i en csv fil. Det gjør jeg ved å pipe resultatet til ``Export-Csv``.

Den fulle onelineren blir derfor 
```
invoke-DbaQuery -SqlInstance srv-prod-sql01 -Database VDC_SystemDB -query 'select * from VWUser' | Select-Object -Property UserId, Username, LastUpdate | Where-Object {$_.LastUpdate.Date -ge $chdt} | Export-Csv F:\Exsitec_Temp\VDC_brukere.csv -NoTypeInformation
```
