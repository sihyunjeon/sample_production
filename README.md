Moriond17 Campaign Production
====

# Setup
```bash
export VO_CMS_SW_DIR=/cvmfs/cms.cern.ch
source $VO_CMS_SW_DIR/cmsset_default.sh
export SCRAM_ARCH=slc6_amd64_gcc481
scram p CMSSW CMSSW_7_1_18
cd CMSSW_7_1_18/src
cmsenv
git-cms-init
git clone git@github.com:jedori0228/sample_production.git
cd sample_production
git checkout -b CMSSW_7_1_18 origin/CMSSW_7_1_18
```

#GEN-SIM (KISTI condor)
```bash
cd GEN-SIM
```
* Make a txt file of input lhe file path
  * (SE)  /store/<SOMEWHERE>
  * (Local file) file:/LOCAL/FILE/PATH
* Fix submit_batch.sh file properly
```bash
source submit_batch.sh
```

#GEN-SIM (KNU TIER 2 CRAB3)
* CRAB3 setup
```bash
source /cvmfs/cms.cern.ch/crab3/crab.sh
```

* Change two files in GEN-SIM/CRAB_KNU_TIER2/skeletons/  
* GEN-SIM_crab_skeleton.py :  
  * CRAB recommends using only one LHE file as an input LHESource.  
So, if you want to genearte 100k event, generate a lhe file containing (more than) 100k events.
  * Then, place the lhe file at SE.
  * If your lhe file is in KNU TIER 2, filename should be  
'root://cluster142.knu.ac.kr//store/user/\<USER\>/\<SOMEWHERE\>'
  * If your lhe file is in KISTI TIER 3, filename should be  
'root://cms-xrdr.sdfarm.kr:1094///xrd//store/user/\<USER\>/\<SOMEWHERE\>'

```python
...
process.source = cms.Source("LHESource",
    fileNames = cms.untracked.vstring(
##LHE_INPUTS_ARE_HERE## <- your lhe file path
    )
...
```  
* crab_skeleton.py : 
```python
...
##requestName## E.g., config.General.requestName = 'MajoranaNeutrinoToMuMuMu_M-40_CMSSW_7_1_18_GEN-SIM'
...
##psetName## E.g., config.JobType.psetName = 'GEN-SIM_crab.py'
...
##outputPrimaryDataset## E.g., config.Data.outputPrimaryDataset = 'MajoranaNeutrinoToMuMuMu_M-40'
...
```  
```bash
crab submit -c crab.py
```  
