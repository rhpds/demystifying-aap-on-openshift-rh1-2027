## Brief Overview

Scenario 3 focuses on Kubernetes node scheduling: a misconfigured configuration in the AAP CR prevents PostgreSQL pods from being scheduled, leaving them in the Pending state. Students run the Break Scenario job template, then investigate failing pod events through the OpenShift Console to identify the root cause and apply the fix. This scenario connects Kubernetes scheduling fundamentals with AAP CR configuration.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift day-2 operations
- **Prerequisites**: Module 01 complete; basic familiarity with the OpenShift Console Events view
- **Estimated duration**: 20 minutes

## Learning Objectives

- Run a Break Scenario job template and observe pods stuck in a non-Running state
- Investigate failing pod events in the OpenShift Console to identify the scheduling root cause
- Apply a fix to the AAP Custom Resource to resolve the scheduling issue
- Verify that AAP components return to a Running state after the fix is applied

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Observe Failing Pods | 4 min |
| 2 | Investigate Pod Events | 5 min |
| 3 | Identify the Root Cause | 4 min |
| 4 | Apply the Fix | 5 min |
| 5 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 3 Showroom tab; log into AAP and run the Break Scenario job template
2. Open the OpenShift Console; navigate to Workloads → Pods; filter by the scenario namespace
3. Identify pods that are not reaching a Running state; click a failing pod and open the Events tab
4. Read the scheduler event messages to understand why pods cannot be placed
5. Once the root cause is identified, apply the fix through the OpenShift Console
6. Refer to the solution page if guidance is needed: xref:module-04-node-selector-solution.adoc[Solution: Automation Controller]
7. Confirm all pods transition to Running state and log into the AAP UI successfully
8. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- Scheduler event messages on Pending pods surface the exact reason placement failed
- AAP operator-managed workloads are controlled via the AAP CR; changes to the CR trigger operator reconciliation
- The OpenShift Console Events view is the primary diagnostic tool for pod scheduling failures

## Infrastructure Notes

- The Break Scenario job template introduces the fault; the environment is healthy before it runs
- Solve/Validate buttons are present on this scenario page
