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
4. **to copy files from KISTI to LPC**

````console
scp -r -P 4280 runiyal@ui10.sdfarm.kr:/cms/scratch/runiyal/decaf/analysis/hists/darkhiggs2018 .
````

### B. creating files packs, compile weights and generating histograms 

````console
[runiyal@ui20 ~]$ cd ../../scratch/runiyal/decaf 
````
1. **Initial settings**
edit the [run_condor.py](https://github.com/rishabhCMS/decaf/blob/master/analysis/run_condor.py#L45)

replace /tmp/x509up_u556950957 --->  /tmp/x509up_u556951020
````console

[runiyal@ui20 decaf]$ git clone https://github.com/rishabhCMS/decaf.git
[runiyal@ui20 decaf]$ cd decaf/
[runiyal@ui20 decaf]$ . ./setup_lcg.sh # just run it once
[runiyal@ui20 decaf]$ . ./env_lcg.sh   # run it everytime when you login

````

2. **Packing files into packs of 32 for 2018**

````console
[runiyal@ui20 analysis]$ python pack.py -y 2018 -p 32  #packs are done on lpc cluster 
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
     
     - **Running tests on individual file before submitting jobs**
     ````console
     #chek you coffea version first
     $python
     >>> import coffea
     >>> coffea.__version__
     # if the version is something else use the following command to install the relevant version
     pip install coffea==0.6.37 --user
     
     python run.py --metadata=2018 --dataset=MET____0_ --processor=darkhiggs2018
     
     python reduce.py -d ST_tW_top_5f_inclusiveDecays_TuneCP5_13TeV-powheg-pythia8 -f hists/darkhiggs2018/
     condor_q -better-analyze
     ````
     - **Running tests on individual file using condor**
     ````console
     if want to test a single condor job with condor submission
     python run_condor.py --processor=darkhiggs2018 --metadata=2018 --cluster=kisti --dataset=MET____0_
     python reduce_condor.py -d ST_tW_top_5f_inclusiveDecays_TuneCP5_13TeV-powheg-pythia8 -f hists/darkhiggs2018/ -c kisti -x -t

     # test 
     ````
     
     - **submitting all jobs with condor**
     ````console
     [runiyal@ui20 analysis]$  python run_condor.py --processor=darkhiggs2018 --metadata=2018 --cluster=kisti -t #this generates .futures files
     [runiyal@ui20 analysis]$  python reduce_condor.py -f hists/darkhiggs2018 -c kisti -t -x
     [runiyal@ui20 analysis]$  python merge_condor.py -f hists/darkhiggs2018 -c kisti -t -x
     [runiyal@ui20 analysis]$  python merge.py -f hists/darkhiggs2018 -p
     [runiyal@ui20 analysis]$  python scale.py -f hists/darkhiggs2018.merged 
     ````
     
     - **Plotting**
      
4. **Accessing Jupyter NB on KISTI**

    ````console
    rishabh@uniyal:~$ ssh -p 4280 -L 9009:localhost:9009 runiyal@ui20.sdfarm.kr                            
    [runiyal@ui20 ~]$ cd ../../scratch/runiyal/decaf
    [runiyal@ui20 decaf]$ source env_lcg.sh
    [runiyal@ui20 decaf]$ jupyter notebook --no-browser --port=9009 --ip 127.0.0.1 &>./start_jupyter.log &
    [runiyal@ui20 decaf]$ jupyter notebook list
    ````

Now there are two methods

        Method 1: go to your local browser and in the address bar type "127.0.0.1:9009" and enter the token number 
        Method 2. Copy paste on of the links you see after executing "jupyter notebook list"
