# Manual operation procedures

## Things to check before submitting ACDC

-   check if ReqMgr status is `completed`
-   check if unified status is indeed `assistance-manual`, as sometimes there are delays in the cms-unified page
-   look for existing jira tickets, if there is ongoing discussion, make sure the issues have been resolved before submitting ACDC
-   check for solutions for specific errors in this [cheat sheet](https://docs.google.com/spreadsheets/d/12JBANxwzN0KWAV4o-yYxnA4Fe2juGXie5uiFXgF3bXo/edit#gid=0)
-   check if there is an ongoing ACDC (Action: Pending on console)

## Creating Jira tickets

-   copy the full url of the workflow on Dima's site into description.
    for example, `https://dmytro.web.cern.ch/dmytro/cmsprodmon/workflows.php?prep_id=task_TOP-RunIISummer20UL18NanoAODv2-00233`
-   put the prep-id as title
-   set priority
-   tag interested parties in the description and component filed. (e.g. pdmv-conveners)
-   **set labels** (e.g. T2_IN_TIFR, 8028-FallbackFileOpenError)

## dealing with irrecoverable errors

-   if workflow not `ReReco`, aiming 90% stats,
    -   if there are recoverable errors
        -   **submit ACDC**
    -   if only irrecoverable errors
        -   if stats < 90%, **contact pdmv**
        -   else, **bypass**
-   else, aiming 100% stats, same procedure as the above branch

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

# workflow composition
workflows are made of **tasks**.
Each task represents one step in the workflow production chain (e.g. GEN, SIM, DIGI...)

Tasks are divided into **jobs** to be run on a batching system (e.g. condor).

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