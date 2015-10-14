# Überwachte Services in sharedlogic

* Aktuell wird das EV2 mit der CSP Version 1.33.5 produziert. Diese enthält nicht alle bekannten Verbesserungen an der Watchdog Infrastruktur.
* Einige wenige spezial EV2 (6 Wege IPV) werden mit der CSP Version 2.2.2 produziert
* Alle Versionen sind vom Jetty Memory Leak betroffen

***

# CSP 1.33.5

![alt text](images/1.33.5-watchdogStandV1.33.5.png "CSP 1.33.5")

***

# CSP 2.2.x

![alt text](images/2.2.x-watchdogStandV2.2.2.png "CSP 2.2.x")

***

# Aktuell CSP 2.8.0 (Apartimentum)

![alt text](images/2.8.0-watchdogDenialOfService.png "CSP 2.8.0")

***

# Auswirkungen Speicherleck

![alt text](images/speicherleak.png "Speicherleak")

# JVM Agent Schnittstelle

Die Java Virtual Machine hat eine Schnittstelle, die genutzt werden kann um, über die Aktivitäten der Garbage Collection informiert zu werden. Mit einem zusätzlichen C-Programm, das im Kontext der VM läuft, können wir in Zukunft solche Probleme detektieren. 

![alt text](images/speicherleakAgent.png "Speicherleak")
