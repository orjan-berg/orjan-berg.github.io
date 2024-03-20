---
title: "RobocopyBasics"
date: 2023-01-25T15:34:17+01:00
draft: false
toc: false
images:
tags:
  - untagged
---

Robocopy 'C:\Program Files (x86)\Visma\Business\' E:\Temp\Business /e /NFL /NDL /NS /NC /NJH /MT:2

## Parametere

### /s
*kopierer alle foldere ekskluder tomme*
### /e
*kopierer alle foldere inkludert tomme*
### /EF
*Eksludere filer f.eks *.pdf*
### /ED
*Eksludere directory*
### /NFL
*No File List - don't log file names.*
### /NDL
*No Directory List - don't log directory names.*
### /NJH
*No Job Header.*
### /NJS 
*No Job Summary.*
### /NP  
*No Progress - don't display percentage copied.*
### /NS  
*No Size - don't log file sizes.*
### /NC  
*No Class - don't log file classes.*
### /MAXAGE:n
*Exclude files older than n days/date.*
### /MINAGE:n
*Exclude files newer than n days/date.*

