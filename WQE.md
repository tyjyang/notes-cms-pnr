[page for stuck WQE element](https://cmsweb.cern.ch/couchdb/workqueue/_design/WorkQueue/_rewrite/stuckElementsInfo)

there are 2 major reasons why a workflow can end up in stuck:

## Input file issue
If the input files are missing, the workflows could get stuck.  
The typical symptom would be a blank input site list on the Workquene page of the problematic task.  
If the stats are high (for MC), and the workflow is old, then we just bypass.

To do that:
-   find the `prep-id` in the `Request Name` on the Stuck WQE Element page, go to Dima's site for that workflow.
-   then click on the WorkQuene button for the task with `ReqManager Status` that is `running-closed`.
-   In the new page, locate the row (element) that has Status `avaiable` and empty Sites, check the Inputs in rucio. 
-   on lxplus, check rucio status by:
    -   `rucio list-rules cms:/[input_block/dataset]` check commands (rules) for rucio to transfer data to designated sites to start workflow
    -   `rucio list-dataset-replicas cms:/[input_block/dataset]` find locations where replica of datasets are stored
-   if input file is missing, as shown by `rucio list-dataset-replica`, `force-complete` on `ReqMgr`
-   `force-complete` on ReqMgr will send workflow to `assistance-manual`, which is undesirable,
    just remember to change them to `closed-out` after they are announced, which typically takes $\cal{O}(\text{5 min})$.
 -   unified will automatically bypass the workflow for you after workflows transition to `closed-out`
 
Note:
-   To set up rucio on lxplus:
    ```
    source /cvmfs/cms.cern.ch/cmsset_default.sh
    voms-proxy-init -voms cms -rfc -valid 192:00
    source /cvmfs/cms.cern.ch/rucio/setup.sh
    export RUCIO_ACCOUNT=`whoami` # Or your CERN username if it differs
    ```
-   rucio as a data management service is designed to replace PhEDEx
-   dataset vs block: block is part of a dataset, indicated by `#xxxxx-xxx...` as a suffix to the dataset name
-   unified would do validation automatically after a workflow reaches `ReqMgr` status `completed`
-   `cmsunified please do bypass` works only for completed workflows, so you need to `force-complete` first on `ReqMgr` before bypassing
-   workflows with completion below a certain threshold (e.g. 95% for MC, 99% for NanoAOD, 100% for ReReco) will enter `assistance-manual` state
-   workflows get divided into elements, which is identified by their `id` within the workflow. 
    that information is found in the rightmost column of the Stuck WQE Elements page.
-   there could be other reasons why inputs are missing. For example, a workflow could be created before the rucio transfer from tape to disk is completed.  
    for new workflows with low completion, it might be worth investigating the reason for missing inputs rather than just bypass.

## ACDC issue
If ACDCs are submitted with incorrect configuration, such as changing sites without enabling `xrootd`, workflows can end up stuck in WQE.  
There should be `ACDC` in the `Request Name` of these workflows. To deal with them:
-   find the `prep-id` in the `Request Name` on the Stuck WQE Element page, go to Dima's site for that workflow.
-   then click on the WorkQuene button for the ACDC task with `ReqManager Status` that is not `completed`.
-   Check their input sites in the rightmost column of the WorkQueue page
-   go back to Dima's page, and check the `ReqMgr` page of the ACDC task 
-   use the following keyword to search for information under the `JSON` tab:
    -   `TrustSitelists` shows whether xrootd was enabled for primary input
    -   `SiteWhitelist` is the list of sites you select when submitting the ACDC
    -   `InitialTaskPath` the name of the task for which the ACDC was submitted
    -   `InputDataset` to check the input dataset

-   if `SiteWhitelist` is different from input sites, then `TrustSitelists` needs to be `true`
    -   If not, `abort` the ACDC on `ReqMgr`
    -   go to the console page of the original task
    -   look for name of ACDC task under `InitialTaskPath` on `ReqMgr`
    -   click on `Only this task`, and enable `xrootd`
    -   submit the ACDC

Note:
-   The reason that `xrootd` needs to be enabled is that: for a workflow to be processed at a site where the inputs are not stored locally,
    it needs to first reads the input from other sites over Internet. Without `xrootd` the reading cannot happen.
-   Secondary inputs are for pileups, so they are rather large and only stored at major sites. Therefore, `TrustPUSitelists` will usually be `true` on `ReqMgr` for workflows to read the inputs from the major sites where pileup (Neutrino) samples are stored



