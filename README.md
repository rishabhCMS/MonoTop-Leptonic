# MonoTop-Leptonic
This repository will contain resources for MonoTop Analysis in the leptonic channel

### A. Commants for the KISTI super cluster

1. **to log in**

````console
rishabh@uniyal:~$ ssh -P 4280 runiyal@ui10.sdfarm.kr
````

2. **to copy files from local computer to KISTI to /cms/ldap_home/runiyal/**

````console
rishabh@uniyal:~$ scp -P 4280 krb5.conf runiyal@ui10.sdfarm.kr:/cms/ldap_home/runiyal
````

3. **to create a dynamic port to KISTI**

````console
rishabh@uniyal:~$ ssh -L 9094:localhost:9094 -p 4280 runiyal@ui20.sdfarm.kr
````

### B. Further instruction to be written

````console
[runiyal@ui20 ~]$ cd /cms/scratch/runiyal
````
