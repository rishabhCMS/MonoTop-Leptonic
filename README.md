# MonoTop-Leptonic
This repository will contain resources for MonoTop Analysis in the leptonic channel

### A. Commands for the KISTI super cluster

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

### B. creating files packs, compile weights and generating histograms 

````console
[runiyal@ui20 ~]$ cd /cms/scratch/runiyal 
````
1. **Initial settings**
````console
[runiyal@ui20 decaf]$ git clone https://github.com/rishabhCMS/decaf.git
[runiyal@ui20 decaf]$ cd decaf/
[runiyal@ui20 decaf]$ source setup_lcg.sh # just run it once
[runiyal@ui20 decaf]$ source env_lcg.sh   # run it everytime when you login

````

2. **Packing files into packs of 32 for 2018**

````console
[runiyal@ui20 analysis]$ python pack.py -y 2018 -p 32 
````

3. **Generating histograms**

    - **Compile weights**
    
      weights required by the [darkhiggs processor](https://github.com/rishabhCMS/decaf/blob/master/analysis/processors/darkhiggs.py#L1632)
    
    ```
    ..........
    corrections = load('data/corrections.coffea')
    ids         = load('data/ids.coffea')
    common      = load('data/common.coffea')
    ..........
    ```

      ````console
      [runiyal@ui20 analysis]$ python util/common.py 
      ````
      for 2018 edit the [correction.py](https://github.com/rishabhCMS/decaf/blob/master/analysis/util/corrections.py#L21)
      
      replace ```pu_hist=pu_files[year]['puWeights']```

      with    ```pu_hist=pu_files[year]['pu_weights_central']```
      
      ````console
      [runiyal@ui20 analysis]$ python util/corrections.py
      [runiyal@ui20 analysis]$ python util/ids.py
      ````
      
     - **Generate coffea processor**
     
     ````console
     [runiyal@ui20 analysis]$ python processors/darkhiggs.py -y 2018
     ````
     - **Running with condor**
     ````console
     [runiyal@ui20 analysis]$ python run_condor.py --processor=darkhiggs2018 --metadata=2018 --cluster=kisti -t
     [runiyal@ui20 analysis]$   python reduce_condor.py -f hists/monotop2018 -c kisti -t
     [runiyal@ui20 analysis]$  python merge_condor.py -f hists/monotop2018 -c kisti -t
     [runiyal@ui20 analysis]$  python merge.py -f hists/monotop2018 -p
     [runiyal@ui20 analysis]$  python scale.py -f hists/monotop2018.merged 
     ````
     
     - **Plotting**
      
