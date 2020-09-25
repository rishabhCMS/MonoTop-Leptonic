# W+Jets low pt study

## low pT samples
    2018: /WJetsToLNu_Wpt-0To50_TuneCP5_13TeV-amcatnloFXFX-pythia8/RunIIAutumn18MiniAOD-102X_upgrade2018_realistic_v15-v1/MINIAODSIM

    2017: /WJetsToLNu_Wpt-0To50_TuneCP5_13TeV-amcatnloFXFX-pythia8/RunIIFall17MiniAODv2-PU2017_12Apr2018_94X_mc2017_realistic_v14-v1/MINIAODSIM

    2016: /WJetsToLNu_Wpt-0To50_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv2-PUMoriond17_80X_mcRun2_asymptotic_2016_TrancheIV_v6-v1/MINIAODSIM

    2016: /WJetsToLNu_Wpt-0To50_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv3-PUMoriond17_94X_mcRun2_asymptotic_v3_ext1-v1/MINIAODSIM

    ##medium pT sample (not added yet)

    2016: /WJetsToLNu_Wpt-50To100_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv2-PUMoriond17_80X_mcRun2_asymptotic_2016_TrancheIV_v6-v1/MINIAODSIM

## Commands

## Setup CMSSW
````console
cmsrel CMSSW_10_2_18
export SCRAM_ARCH slc7_amd64_gcc700
cd CMSSW_10_2_18/src
cmsenv
````

## Change CMSSW to enable LZ4 compression
````console
git cms-addpkg PhysicsTools/NanoAOD
cp /uscms_data/d3/matteoc/CMSSW_10_2_18/src/PhysicsTools/NanoAOD/plugins/NanoAODOutputModule.cc PhysicsTools/NanoAOD/plugins/NanoAODOutputModule.cc
````
## Get customized NanoAOD producers
````console
git clone https://github.com/LPC-DM/NanoHarvester.git PhysicsTools/NanoTuples
````
## Compile
````console
scram b -j12
````
## Get config files
````console
cd PhysicsTools/NanoTuples/crab
````
This directory contains the config files to use for 2016 to 2018 data and mc NanoAOD production in CMSSW_10_2_18. Test one of the config files with the following command:
````console
cmsRun mc_NANO_2016.py
````

## Production
````console
cd $CMSSW_BASE/src/PhysicsTools/NanoTuples/crab
cmsenv
# set up grid proxy
voms-proxy-init -rfc -voms cms --valid 168:00
# set up CRAB env (must be done after cmsenv)
source /cvmfs/cms.cern.ch/crab3/crab_slc7.sh
````

**Step 0**: Choose the right python config file. This is the `-p` argument to `crab.py`. You will also need this to determine the splitting parameters (`Step 1 below`). See section above for how the configs were generated. Note that you need a different config file for `Run2018ABC` vs `Run2018D`. The eras for `2018` are also separated into two input text files with the dataset names.

**Step 1 **: Estimate appropriate splitting parameters.

Edit `testConfig.py` to change `config.JobType.psetName` to `mc_NANO_[year].py` or `data_NANO_[year].py` and to change `config.Data.inputDataset` to one of the datasets in `miniaod[year].txt` or `miniaod[year]_data.txt`.

Use the `crab --dryrun` option to get an estimate of the run time. This might take a few minutes to run.

````console
crab submit --config=testConfig.py --dryrun
````
Ignore a possible warning:

`Warning: Incompatible CRABClient version 3.3.1910.patch2`

Part of the output from this command should look like the following:

    For ~480 minute jobs, use:
    Data.unitsPerJob = 16737

Round this number to an appropriate value, and use it to set the `-n` option for `crab.py` in Step 2.

**Step 2**: use the `crab.py` script to submit the CRAB jobs. Replace `[year]` with the appropriate value

For MC:
````console
python crab.py -p mc_NANO_[year].py -o /store/group/lpccoffea/coffeabeans/NanoAODv6/nano_[year] -t NanoTuples-[year] -i miniaod[year].txt  --send-external -s EventAwareLumiBased -n [XX] --work-area crab_projects_mc_[year] --dryrun
````

**Step 3**: check job status

The status of the CRAB jobs can be checked with:
````console
./crab.py --status --work-area crab_projects_[mc/data]_[year]
````
Note that this will also resubmit failed jobs automatically.

The crab dashboard can also be used to get a quick overview of the job status: https://dashb-cms-job.cern.ch/dashboard/templates/task-analysis

More options of this crab.py script can be found with:

````console
./crab.py -h
````
