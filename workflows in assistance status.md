When workflows **finish running** but produce **less statistics**
(number of events or number of lumisections depending on how it's calculated) than expected,
their [[unified status]] will be transitioned to `assistance-*`,
and they will show up on the [unified page](https://cms-unified.web.cern.ch/cms-unified//assistance.html#assistance-manual).  
It is then the responsibility of PNR operators to solve the issues and make sure that the workflows move on.

There are many categories of the `assistance-*` workflows. They can be understood by their category name:
-   `manual` in the category name means the workflow is finished with statistics below a certain **threshold**. This threshold is determined by the nature of the workflow. For example:
    -   100% for data workflows (e.g. `ReReco` in workflow name), we don't want to lose anything from data
    -   99% for NanoAOD MC
    -   95% for other MC
-   `filemismatch` means the workflow doesn't find the input file it needs to run
-   workflows stay in the `agentfilemismatch` status for 2 days after a `filemismatch` error is first reported. If the issue isn't resolved for 2 days, then the workflow moves to status `filemismatch`.
    This is to rule out network issues.
        -   The workflows in `[agent]filemismatch` will move on once the issue is resolved on either DBS or Rucio.
-   `noRecoveryDoc` means the workflow finishes with incomplete statistics, but the system hasn't seen any failures so it can't give any recovery information.
-   `biglumi` means the number of events in the LumiSections for the workflow falls outside of the recommended bound (too many or too few events in one lumi).
-   `recovering` means there is an ongoing first round of [[wtc-console|ACDC]]
-   `recovered` means the first round of [[wtc-console|ACDC]] has finished
    -   `recovered-recovering` means the second or more rounds of [[wtc-console|ACDC]] is ongoing
-   



3 in status assistance-agentfilemismatch
3 in status assistance-agentfilemismatch-manual
1 in status assistance-biglumi-noRecoveryDoc
14 in status assistance-filemismatch
9 in status assistance-filemismatch-manual
2 in status assistance-filemismatch-manual-recovered
3 in status assistance-filemismatch-noRecoveryDoc
3 in status assistance-filemismatch-recovered
69 in status assistance-manual
88 in status assistance-manual-recovered
116 in status assistance-noRecoveryDoc
6 in status assistance-recovered-recovering
20 in status assistance-recovering

new -> assignment-approved: pdmv's responsibility
assignment-transitionO
staging: from tape to disk
acquired: wf waiting in the queue
