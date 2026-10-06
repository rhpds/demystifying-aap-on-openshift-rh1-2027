## Brief Overview

Scenario 3 focuses on Kubernetes node scheduling: a misconfigured `nodeSelector` in the AAP CR's database section prevents PostgreSQL pods from being scheduled onto any node, leaving them in the Pending state. Students run the Break Scenario job template, then investigate scheduler events through the OpenShift Console to identify the unmatched label and apply the fix. This scenario connects Kubernetes scheduling fundamentals with AAP CR configuration.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift day-2 operations and capacity planning
- **Prerequisites**: Module 01 complete; basic understanding of Kubernetes node labels and pod scheduling; familiarity with the OpenShift Console Events view
- **Estimated duration**: 20 minutes

## Learning Objectives

- Run a Break Scenario job template to introduce a node scheduling fault
- Use OpenShift Console scheduler events to identify a pod scheduling failure and its root cause
- Correct the database configuration in the AAP Custom Resource to resolve the scheduling issue
- Verify that AAP components recover after the fix is applied

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Observe Pending Pods | 4 min |
| 2 | Read Scheduler Events | 5 min |
| 3 | Identify the Root Cause | 4 min |
| 4 | Fix the AAP CR | 5 min |
| 5 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 3 Showroom tab; navigate to AAP Controller and run the Break Scenario job template
2. Navigate to Workloads → Pods in the OpenShift Console; filter by the scenario namespace
3. Identify the PostgreSQL pod(s) in Pending state
4. Click a Pending pod and open the Events tab; read the scheduler failure message
5. Open the AAP CR (Operators → Installed Operators → AAP → AnsibleAutomationPlatform instance)
6. Switch to the YAML view and locate the database configuration section
7. Correct the invalid configuration; save the CR
8. Delete the stale PostgreSQL pod so the operator creates a new one with the correct configuration
9. Monitor pod status until all AAP components return to Running state
10. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- A `nodeSelector` that matches no node labels causes the pod to remain Pending indefinitely
- The OpenShift Console Events tab on a Pending pod surfaces the exact scheduling failure reason
- AAP operator-managed workloads are controlled via the AAP CR; direct pod edits are overwritten on the next reconciliation

## Infrastructure Notes

- The Break Scenario job template injects an invalid `nodeSelector` into the AAP CR
- Solve/Validate buttons are present on this scenario page
