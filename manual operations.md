# Manual operation procedures

## Permanent procedures
### Things to check before submitting ACDC

-   check if ReqMgr status is `completed`
-   check if unified status is indeed `assistance-manual`, as sometimes there are delays in the cms-unified page
-   check the statistics; if it's 100%, ACDC won't help and the issue might be elsewhere.
-   look for existing jira tickets, if there is ongoing discussion, make sure the issues have been resolved before submitting ACDC
-   check if there is already an ongoing ACDC (Action: Pending on console)
-   check for solutions for specific errors in this [cheat sheet](https://docs.google.com/spreadsheets/d/12JBANxwzN0KWAV4o-yYxnA4Fe2juGXie5uiFXgF3bXo/edit#gid=0)


### Creating Jira tickets

-   copy the full url of the workflow on Dima's site into description.
    for example, `https://dmytro.web.cern.ch/dmytro/cmsprodmon/workflows.php?prep_id=task_TOP-RunIISummer20UL18NanoAODv2-00233`
-   put the prep-id as title
-   set priority
-   tag interested parties in the description and component filed. (e.g. pdmv-conveners)
-   **set labels** (e.g. T2_IN_TIFR, 8028-FallbackFileOpenError)

### dealing with irrecoverable errors

-   if workflow not `ReReco`, aiming 90% stats,
    -   if there are recoverable errors
        -   **submit ACDC**
    -   if only irrecoverable errors
        -   if stats < 90%, **contact pdmv**
        -   else, **bypass**
-   else, aiming 100% stats, same procedure as the above branch

### nLumis vs nEvts

When the number of lumisections is incorrectly setup,
workflows with all events completed will still end up in assistance-manual.

To confirm the cause, search for `lumi completion` in Unified log on Dima's page.

## Transient procedures
### `50115` for campaign `Run3Winter22XX`
-   Due to a bad xml parser in an old CMSSW version, bypass these workflows

### `50664` and `71304` workflows
-   Put (`50664-PerformanceKill` OR `71304-JobKilled`) AND (label for campaign e.g. ) as jira labels
-   wait for response
-   if no response for some time, reject

### `8028` for empty files
When input files were produced okay, but later became empty, one will see this error message:
```
Fatal Exception (Exit code: 8028)
An exception of category 'FallbackFileOpenError' occurred while
[0] Constructing the EventProcessor
[1] Constructing input source of type PoolSource
[2] Calling RootFileSequenceBase::initTheFile()
Additional Info:
[a] XrdCl::File::Open(name='[root://cmsxrootd.hep.wisc.edu//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root](root://cmsxrootd.hep.wisc.edu//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root)', flags=0x10, permissions=0660) => error '[ERROR] Server responded with an error: [3011] Too many attempts to gain dfs read access to the file
' (errno=3011, code=400). No additional data servers were found.
[b] Last URL tried: [root://cmsxrootd.hep.wisc.edu:1094//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root?tried=](root://cmsxrootd.hep.wisc.edu:1094//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root?tried=)
[c] Problematic data server: cmsxrootd.hep.wisc.edu:1094
[d] Disabled source: cmsxrootd.hep.wisc.edu:1094
[e] Input file [root://cmsxrootd.hep.wisc.edu//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root](root://cmsxrootd.hep.wisc.edu//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root) could not be opened.
Fallback Input file [root://cmsxrootd.fnal.gov//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root?source=glow](root://cmsxrootd.fnal.gov//store/mc/RunIISummer20UL18MiniAOD/HToSSTo2Mu2Hadrons_MH125_MS1p1_ctauS100_TuneCP2_13TeV-powheg-pythia8/MINIAODSIM/106X_upgrade2018_realistic_v11_L1v1-v1/2520000/A639C8D7-B843-F34E-98F7-8C077C24B6B2.root?source=glow) also could not be opened.
Original exception info is above; fallback exception info is below.
[f] Fatal Root Error: @SUB=TStorageFactoryFile::ReadBuffer
read from Storage::xread returned 0. Asked to read n bytes: 300 from offset: 0 with file size: 0
```
-   don't ACDC, instead apply `EmptyInputFile` as the jira label

# Sites

## site peculiarities

### TIFR site

There has been a long-standing issue at TIFR since end of 2021.  
To deal with workflows failed at TIFR while this site issue remains, we:
-   if error `99109`, **submit ACDC**
-   else, 
    -   for jobs failed with > 90% stats, **bypass** in jira tickets
    -   for jobs failed with < 90% stats,
        -   if number of jobs < O(100) (check on wmstats), **kill and clone**
        -   otherwise, open jira tickets and **tag pdmv**

### xrootd and HLT

enable xrootd in ACDC when:
-   sites where the input files reside in is in drain (bold red for site name on console)

when enabling xrootd, un-select CERN sites including HLT,
as HLT cannot read files from sites outside of CERN, but outside sites can read from CREN storage.

## report site issue in ggus

open a ggus ticket to report any site issue, and cc the workflow and site support team at:
-   [cms-comp-ops-workflow-team@cern.ch](mailto:cms-comp-ops-workflow-team@cern.ch)
-   [cms-comp-ops-site-support-team@cern.ch](mailto:cms-comp-ops-site-support-team@cern.ch)

## Opportunistic sites (T3)

To improve the utilization efficiency of our computing facilities, we would direct workflows to T3 sites
when they have spare computing resources.  
For these sites, the probability of having a site issue is higher.
Therefore, when irrecoverable errors are at T3 sites, try to run the workflows at non-opportunistic sites before bypassing or tagging pdmv.

# Input Datasets

## Pileup
pileup inputs are known as "secondary" inputs, and depending on how large they are,  
some of them can practically only be read locally due to the I/O load. 

There are different categories of pileup inputs, and they can be told apart by the starting word of their name:
-   Neutrino
-   MinBias
-   RelVal
-   Hydjet
Neutrino and MinBias are the major categories, and MinBias is particularly large so they are only stored at major sites and can only be read locally.

To check if a workflow has secondary inputs, look for `minbias` in the error report, or check `MCPileup` on ReqMgr.

WMCore-agent will only look for input blocks that are protected by their account.

# workflow information

## workflow composition

workflows are made of **tasks**.
Each task represents one step in the workflow production chain (e.g. GEN, SIM, DIGI...)

Tasks are divided into **jobs** to be run on a batching system (e.g. condor).

## checking workflow information

### prepid and request ID (workflow ID)

For MC workflows, prepid is contained in the request ID  
FOr `ReReco` workflows, enter the request ID on `ReqMgr` to get prepid in the JSON file

### worker node

To check the specific node (specific machine within a site) at which the workflow was run:
-   enter the workflow ID on [wmstats](https://cmsweb.cern.ch/wmstats/index.html),
-   click on the box under the "L" column
-   In the list of tasks under the workflow, click the "L" column box again for the task you want to check
-   In the information shown at the bottom of the page, search for "worker node"

# Computing Resources
Each workflow can only take up a limited amount of computing resource before hitting PerformanceKill walls (exit code `50664` and `50660`). 

To better understand how computing resource limitations on workflows,
one first needs to understand the [[manual operations#workflow composition|components of a workflow]].

The limitations are put on condor "slots".
Each condor slot hosts one job from a task of the workflow.
Jobs are killed if they exceed designated **CPU time** or **memory usage** for the condor slot.
To overcome these limitations, one could:
-   split the jobs into smaller sizes so it takes less resources for them to run through
    -   n.b. you can check the splitting algorithm used by tasks by looking at `SplittingAlgo` from the ReqMgr/JSON page of the workflow. If it is `EventAwareLumiBased`, then splitting will work; if it is `EventBased`, then splitting won't work.
-   increase the number of CPU cores and the amount of memory per core.
    -   n.b. changing nCore does not currently work in console (2022-04-05).

# Unified status
## agentfilemismatch
-   a grace period of 2 days before it moves to filemismatch
-   ignore them if show up in assistance-manual

## assistance-manual
-   workflows end up in this state if their statistics (percentage completed) ends up below a threshold.  
    The common thresholds are:
    -   100% for data
    -   99% for NanoAOD MC
    -   95% for other MC
    
## assistance-manual-recovered
-   workflows will end up in this state if manual operations for `assistance-manual` workflows failed

# Standard procedure for errors
## `8021-FileReadError` and `8028-FallbackFileOpenError`
In general, you should check dbs to track down the root cause. It might just be opportunistic, so ACDC without excluding the error site might already work.

## `50660`