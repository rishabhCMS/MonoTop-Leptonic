# W+Jets low pt study

## low pT samples
    /WJetsToLNu_Wpt-0To50_TuneCP5_13TeV-amcatnloFXFX-pythia8/RunIIAutumn18MiniAOD-102X_upgrade2018_realistic_v15-v1/MINIAODSIM

    /WJetsToLNu_Wpt-0To50_TuneCP5_13TeV-amcatnloFXFX-pythia8/RunIIFall17MiniAODv2-PU2017_12Apr2018_94X_mc2017_realistic_v14-v1/MINIAODSIM

    /WJetsToLNu_Wpt-0To50_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv2-PUMoriond17_80X_mcRun2_asymptotic_2016_TrancheIV_v6-v1/MINIAODSIM

    /WJetsToLNu_Wpt-0To50_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv3-PUMoriond17_94X_mcRun2_asymptotic_v3_ext1-v1/MINIAODSIM

    ##medium pT sample

    /WJetsToLNu_Wpt-50To100_TuneCUETP8M1_13TeV-amcatnloFXFX-pythia8/RunIISummer16MiniAODv2-PUMoriond17_80X_mcRun2_asymptotic_2016_TrancheIV_v6-v1/MINIAODSIM

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

## Private MC production

**Step 1**: Prepare txt file including eos path to privately produced MiniAODs. For example, if MiniAOD saved at lpc cluster, the path to the file should be

        root://cmseos.fnal.gov//store/path/to/the/MiniAOD

**Step 2**: Use the `crab_private.py` script to submit the CRAB jobs.
````console
# wjets_lowpt_2016
python crab.py -p mc_NANO_2016.py -o /store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016 -t NanoTuples-2016 -i wjets_lowpt_2016.txt  --send-external -s FileBased -n 1 --work-area crab_projects_mc_2016 --dryrun

# wjets_lowpt_2017
python crab.py -p mc_NANO_2017.py -o /store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2017 -t NanoTuples-2017 -i wjets_lowpt_2017.txt  --send-external -s FileBased -n 1 --work-area crab_projects_mc_2017 --dryrun

# wjets_lowpt_2018
python crab.py -p mc_NANO_2018.py -o /store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2018 -t NanoTuples-2018 -i wjets_lowpt_2018.txt  --send-external -s FileBased -n 1 --work-area crab_projects_mc_2018 --dryrun

# wjets_mediumpt_2016
python crab.py -p mc_NANO_2016.py -o /store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016 -t NanoTuples-2016 -i wjets_mediumpt_2016.txt  --send-external -s FileBased -n 1 --work-area crab_projects_mc_2016 --dryrun
````
