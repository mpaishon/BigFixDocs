# Best (and Bad) Practices Guidance FOR BIGFIX Console Operators

## Table of Contents
1. Document History
2. Document Objective
3. Document format
4. “Bad Practices | Things not to do”
5. “Best Practice | Things to do”
6. Appendages
    - Baseline Performance Appendage
    - ActionSite Appendage

---

## Document History
| Version | Author          | Revisions | Date       |
| ------- | --------------- | --------- | ---------- |
| 1.0     | Michael Paishon | Initial   | Oct/4/2024 |

---

## Document Objective

BigFix is a powerful platform and today is designed to scale to 250,000 end points. The platform is extremely performance oriented and can be leveraged to accomplish a multitude of IT operational, security and reporting tasks. All functionalities leverage a single platform and are administered through a single pane of glass. The capabilities and scalability of the platform has seen actual real world production environments, at scale. As with any IT tool, there are preferred approaches and less advantageous approaches to solve business challenges in alignment with IT tool efficiency. With BigFix, this is extremely important. At scale, inefficiency and poor technical approaches are magnified far more than other tools that do not scale similarly. 

Oftentimes, software vendors speak to best practice in alignment with IT tool efficiencies. This document follows that thought process. However, in addition to stating best practices, this document also provides a slightly different perspective. This document also focuses on “bad practices” or items BigFix users should steer away from.  

This document is based off common observations I have made over years. The observations and recommendations that follow are a result of experience at scale with many different customers that have pushed the limits of the BigFix platform for the good, and for the bad. This document is not meant to be a holistic guide or exhaustive list of items to do, or not to do. It is also not meant to provide full context to the statements made. This document provides general principles not given specific customer environment context. Keep in mind there are exceptions to any of the items below, use discretion when interpreting the statements made. 

---

## Document format

Below there are 2 sections defined, “Bad Practices | Things not to do” and “Best Practice | Things to do” each section is comprised of numerous categories which categorize the focus of the items that follow. Each item has a description highlighting a specific point as well as an reference ID which is associated to the item and category. Some of the items also are supported by sections specified within the appendages portion of this document. In these cases, the item and appendage identify the mutual relationship through the ID assigned to the item and the appendage associated to the item. Each item also has a single term which has been set to be bold. This is to provide the reader with a quick reference point as to what the item is relative to, allowing for quick recall while skimming the document later as a reference.

---

## “Bad Practices | Things not to do”

### Content Bad Practices

- **ID BCM1**  
  In general, avoid leveraging iterative or recursive client relevance inspectors. Specific bad examples other customers have tried and regretted include the following: 
  - Pull back all servers event logs that match criteria X from the beginning of time.
  - Pull the name and SHA1 of all files with given permissions across a large volume of files.
  - Iterate the entire file system for files by name or sha1 value.

- **ID BCM2**  
  In general avoid leveraging the following client relevance inspectors in custom content when other alternatives exist:
  - **WMI** – in general not efficient, can cause weird WMI-related resource spikes. See here for more information on WMI inspectors.
  - **DACLS inspectors** - especially when the inspector causes Windows to query the DC for the info (effective permissions).
  - **Descendants of Folders** – Do not use it for file scans.
  - **Event logs** – Good alternative here.
  - **Local users** – Note if the local users inspector is run on an active directory DC, it will iterate all AD users (not good).

- **ID BCM3**  
  Avoid duplicative actions upon the same content across the same machines. Consider duplicate actions that are taken as telling the client to do the same thing again… Duplicate actions translates to inefficiency and additional overhead.  

- **ID BCM4**  
  Unless the intended behavior is to allow an action to run forever, leave the action expiration date defined (default behavior). There have been instances on larger deployments where operators have gotten in a very bad habit of always unchecking the expiration of an action or worse, creating a custom action preset to remove action expiration. If this occurs, it is only a matter of time before it catches up with the architecture. 

- **ID BCM5**  
  Avoid leveraging manual groups where possible, a large amount of manual computer groups can severely slow down the BigFix console.

- **ID BCM6**  
  Do not base a computer group membership upon the non-membership of another. The reason for this is computer group evaluation order is never guaranteed.  
  **Example**: if computer group A includes a definition that a machine is not part of computer group B, there is no guarantee that computer group B will be evaluated first. Thus machines can potentially be temporary members of group A (at least until computer group B is evaluated). The potential of the issue can be dramatic (read as spectacularly disastrous) if operators target actions based upon Computer Group A in this example. 

- **ID BCM7**  
  Do not set analysis property analysis intervals to “Every Report”. Analysis property intervals should always be specified to the most time possible. Customers in the past have gotten in a bad habit of always changing the analysis evaluation interval to “Every Report” this greatly impacts the amount of information that is forwarded through the relay hierarchy. 

- **ID BCM8**  
  Do not bundle reboot actions with baselines; instead, manage reboot windows as separate pieces of content.

- **ID BCM9**  
  In general avoid leveraging baselines, unless no alternatives exist. From a client perspective baselines are the most inefficient type of content. In the context of client performance, think of Baselines as paying a $300 transaction fee on a $100 cash ATM withdrawal (not good).  
  In general Baselines should contain no more than 100 components.  
  If you are going to leverage baselines, assure there is a process to sync, upkeep, and decommission them.  
  Also look to the Baseline performance appendage within this document.

---

## Client Core Bad practices

- **ID BCC1**  
  In general, do not modify the clients CPU thresholds; throwing the client into a dead sprint or slow crawl generally does not serve customers very well.

- **ID BCC2**  
  Avoid locking the client to hold actions queued from executing.

- **ID BCC3**  
  Avoid using the “Send Refresh” right-click option on a large number of computers simultaneously. This will trigger the client to send a full report and has been known to negatively impact customer environments. If you must send the machine a refresh, try not to exceed more than 50 at a time.

- **ID BCC4**  
  Avoid taking actions upon large static lists of computers using the "Enter Device Names" option within the take action dialog. Attempt to limit the number of computers in a static list to no more than 100 computers where possible.

- **ID BCC5**  
  Avoid leveraging the right-click “Remove from database” on machines that are still valid within the deployment. This should only be used to remove a client that is known to have been removed or retired. Valid circumstances for using this option is only when a machine is known to have been removed from the network/retired from management.

- **ID BCC6**  
  Avoid leveraging the right-click “Revoke Certificate” on machines that are still valid within the deployment. This should only be used to refuse a client from using authenticated relay infrastructure. If you revoke a certificate on a computer, it will come back if it connects to a non-authenticating relay, but will be reset.

- **ID BCC7**  
  Avoid creating actions that contradict the successful completion of another. For example, setting a client value to X, and then another action to set it as X-1. This can create a toggling condition if the action is set to reapply whenever it becomes relevant.

- **ID BCC8**  
  Avoid sending WOL packet via the console right-click menu to a large number of offline machines. This is not necessarily an issue from the platform perspective, but may just be a bad “network common sense” decision.  
  **Examples**:
  - Turning on a large number of VDI machines simultaneously could overwhelm the shared host.
  - Turning on all machines at a remote site separated by a slow bandwidth/throughput link could saturate network communications.

---

## Master Operator Bad practices

- **ID BMO1**  
  Avoid global activation of analysis, unless the sum of local activations is enough to warrant a global activation.

- **ID BMO2**  
  Do not create test console operators, test roles, or test content sites as a regular occurrence. Once a console operator, site, or role is created, infrastructure will always carry some content to maintain it, even after deletion.

- **ID BMO3**  
  Avoid assigning sites by ad-hoc site subscription, as there is no automatic inverse action to remove the site once subscribed.

- **ID BM04**  
  Avoid creating content such as Fixlet, Analysis, Baselines, or Computer groups within Actionsite when possible.  
  **Reference**: See ActionSite Appendage.

---

## “Best Practice | Things to do”

### Console Best Practices

- **ID GC1**  
  Make a habit of deleting stopped and expired actions to speed up console performance.

- **ID GC2**  
  Use the “Show non-relevant content” button to filter non-relevant content, improving responsiveness.

- **ID GC3**  
  Manage console refresh intervals individually; set longer intervals for better console performance.

- **ID GC4**  
  Limit columns in the computer asset view to improve performance.

- **ID GC5**  
  Configure the “Mark offline after” setting to 3x the heartbeat interval.

- **ID GC6**  
  Align the console cache setting with BigFix administrator guidance for your environment.

- **ID GC7**  
  Only clear the console cache when troubleshooting specific console errors.

- **ID GC8**  
  Set cache expiration to "Moderate" for a balanced experience.

- **ID GC9**  
  Log out of the console if it will not be used for an extended period.

- **ID GC10**  
  Limit the number of open windows in the console to maintain performance.

---

## Content Maintenance / Awareness Best Practices

- **ID GCM1**  
  Establish a regular monthly rhythm for baseline decommission, maintenance, or updates.

- **ID GCM2**  
  Review recent content releases and updates routinely.

- **ID GCM3**  
  Store shared content within custom sites to make it accessible across operators.

- **ID GCM4**  
  Review existing content before creating new Fixlets, Tasks, or Analysis properties.

---

## BigFix Administrators Best Practices

- **ID GBA1**  
  Ensure SQL tasks complete daily, including index and backup jobs.

- **ID GBA2**  
  Evaluate IO subsystems on a monthly basis.

- **ID GBA3**  
  Check relay schemes routinely to identify inactive or non-reporting relays.

- **ID GBA4**  
  Run the audit trail cleaner regularly to maintain performance.

---

## Client Best Practices

- **ID GCB1**  
  Ensure Anti-Virus exceptions are in place.

- **ID GCB2**  
  Ensure the client and relay meet network prerequisites.

---

## Appendages 

### Baseline Performance Appendage

**Deeper Explanation**  
Baselines are dramatically inefficient from the client content evaluation perspective. Unfortunately, evaluating a baseline action is about 400% more inefficient than kicking off individual actions upon the same source content that comprised the baseline. From a performance standpoint, think of baselines as paying a $300 transaction fee to deposit $100.

Best Practices to Minimize Impact:
- Minimize the number of baselines and associated open actions.
- Evaluate baselines and open baseline actions regularly.
- Never create a baseline action with no expiration date.
- Do not exceed 100 components within a baseline.
- Sync and update baselines routinely.

**Referenced by BCM3**

---

### ActionSite Appendage

**Deeper Explanation**  
The Bigfix Actionsite is the foundation of the BigFix system. Inefficient usage of Actionsite can greatly impact performance. Actionsite must carry command and control parameters for the environment, but avoid adding unnecessary content.

Best Practices to Minimize Impact:
- Use Master Operator accounts when necessary.
- Minimize the number of operators, roles, and content sites.
- Avoid adding groups, Fixlets, or unnecessary items to Actionsite.

**Referenced By BM04**
