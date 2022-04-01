# HLT and xrootd

-   For a given failed task, if there is a necessary site which is drain (marked as red and bold letters in the console):
    -   Pre-select primary xrootd (don't touch the secondary xrootd at any step)
    -   If either `T2_CH_CERN` or `T2_CH_CERN_HLT` is among the pre-selected sites, unselect them
    -   It would be good to display a small message if console does this procedure in order to make it clear for new operators etc. It could be something like `There is a necessary site in drain, therefore enabling xrootd. Since xrootd is enabled, unselecting CERN and HLT since HLT cannot perform remote reads. We unselect CERN too, since jobs sent to CERN can be redirected to HLT internally`

# filter workflows for group ACDC

Sometimes the same type of error could affect multiple workflows, and the steps for manual ACDC are identical.  
Under this circumstance, it would be great if we can filter out these workflows by some criteria,
and apply ACDC with the same configuration for all of them.

The filtering criteria could be:
-   error code
-   which task the errors are in
-   site where the error occurred
-   time when the error occurred
-   campaign to which the workflow belongs
-   percentage of completion
-   size of the workflow

for example, we could say:
-   run ACDC without xrootd and splitting for all workflows in assistance-manual that failed at CERN sites with error 91009
-   kill and clone for all workflows failed at TIFR that has < 90% statistics and < 1M events

# Automatic site reassignment for draining sites
When sites go into drain, we have to manually reassign workflows to other sites. The rules for which sites to reassign the workflows to could be automated:

-   for each site, designate 2-3 backup sites in order of priority, based on geological proximity, network connection, site capacity, etc.
-   when one site goes into drain, cycle through the backup sites in order, ACDC to the working site with highest priority 
-   if all of the backup sites are in drain, cycle through their backup sites, and pick the highest-priority working sites for ACDC
-   repeat this process until one finds a working site to submit the ACDC