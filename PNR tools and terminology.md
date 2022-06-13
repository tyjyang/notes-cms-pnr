A lot of the concepts here are similar, if not identical,
between the Data and MC jobs.
Therefore, we will discuss them in the context of MC for the sake of being succinct.

When someone wants to produce a set of MC samples,
they need to come up with a **configuration file** to be used by 
pieces of software such as the event generator (MadGraph, Pythia, Herwig. etc.)
and the detector simulator (GEANT etc.).
CMSSW will help setting up environments for accessing all necessary software.
With the config file ready, they need to submit a **request** to
pdmV (Physics Data and MC Validation) for review.  
Note that pdmV is a L2 team under PPD (Physics Performance and Dataset).  
After pdmV's approval of the request, it is then handled to Computing Operations (CompOps)
under O&C for production.  
PNR is a L3 team under CompOps.

Within PNR, we call very such request a **workflow**.  
Under each workflow, there can be many **tasks** corresponding to
different stages of the MC production (GENSIM, MiniAOD, NanoAOD, etc. also see CMS datatiers).
Multiple workflows can also be under the same **campaign**,
which is usually named by the production era (e.g. RunIISummer18).  
Workflows can be identified by their **prep-id**, which is the name given to the request.  
They can also be identified by the **workflow-id**, which is a longer name containing the prep-id given by Unified. See below for more details.

The lifecycle of a MC workflow has, briefly speaking, the following steps:
-   **Creation** of the workflow with config file and metadata
-   **Assignment** of the workflow to computing sites
-   Reading of **input** and storage of **output** files (e.g. reading minibias files for pileup, for reading the MiniAOD output for producing the subsequent NanoAOD.
-   **Manual intervention** for various issues; especially **ACDC** for re-submission of workflows with updated parameters to solve site issues.
-   **Announcement** that the workflow is done with and can be **archived**

We will briefly introduce here the main tools used by CompOps during the lifecycle of a workflow.  
There are in general 2 categories of tools: production tools and monitoring tools.
Some tools lie in both categories, but for convenience we categorize them here based on their main function.  
Note that even though PNR doesn't develop or maintain all of them,
having a basic understanding of their functions is still vital for our work.  
There will be more detailed documentation for each of them in the appendices of the PNR WorkBook.

# Production Tools

## WMCore

-   WM stands for **Workflow Management**. This is the core software for workload management.
-   It determines when (based on workflow **priority**) and where (which site) each workflow will be running.  
-   It divides workflows into blocks of **WorkQuene Elements (WQE)**.  
-   Based on the configuration for the workflows, the WQE are further divided into
    **jobs** to be run in **computing slots**, which are virtual chunks of computing resources
    with fixed CPU cores and memory allocation.

## Rucio/DBS

-   Rucio is the data management tool used by CMS for storing data and output of MC workflows
-   It also tells on which site the input files are stored

## Unified
-   a set of scripts for running the workflows
-   transfers the **status** of workflows, and output error messages

## wtc-console

-   Currently our main tool for manual intervention
-   also where we submit **ACDC** for workflows

# Monitoring/reporting Tools

## [ReqMgr](https://cmsweb.cern.ch/reqmgr2/)

-   A Web interface for managing requests
-   can check **workflow metadata** such as sitelist, prep-id, etc.

## grafana
-   a site with custom plots monitoring the performance of our computing systems (e.g. number of workflows failed at each site)

## jira

-   An issue tracker for workflow operations
-   powerful (or maybe not so much) tagging and search functions
-   notifications for people in other teams

## ggus
-   another ticket system we use for issue related to the computing sites