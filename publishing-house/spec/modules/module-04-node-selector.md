## Brief Overview

Scenario 3 focuses on Kubernetes node scheduling: a misconfigured `nodeSelector` in the AAP CR's database section prevents PostgreSQL pods from being scheduled onto any node, leaving them in the Pending state. Students run the Break Scenario job template, then investigate scheduler events through the OpenShift Console to identify the unmatched label. The fix involves correcting or removing the invalid `nodeSelector` from the AAP CR and forcing operator reconciliation. This scenario bridges Kubernetes scheduling fundamentals with AAP CR configuration.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift day-2 operations and capacity planning
- **Prerequisites**: Module 01 complete; basic understanding of Kubernetes node labels and pod scheduling; familiarity with the OpenShift Console Events view
- **Estimated duration**: 20 minutes

## Learning Objectives

- Run a Break Scenario job template to introduce a node scheduling fault
- Use OpenShift Console scheduler events to identify an unmatched `nodeSelector` preventing pod placement
- Correct the `nodeSelector` configuration (or remove it) in the database section of the AAP Custom Resource
- Force operator reconciliation by deleting the Gateway operator manager pod and the stale PostgreSQL pod

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Observe Pending Pods | 4 min |
| 2 | Read Scheduler Events | 5 min |
| 3 | Identify the Misconfigured nodeSelector | 4 min |
| 4 | Fix the AAP CR and Force Reconciliation | 5 min |
| 5 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 3 Showroom tab; navigate to AAP Controller and run the Break Scenario job template
2. Navigate to Workloads → Pods in the OpenShift Console; filter by the scenario namespace
3. Identify the PostgreSQL pod(s) in Pending state
4. Click a Pending pod and open the Events tab; look for a `FailedScheduling` event with a node selector mismatch message
5. Note the label key/value that the scheduler could not match (e.g., `node-role.kubernetes.io/invalid-role: "true"`)
6. Open Operators → Installed Operators → AAP → AnsibleAutomationPlatform; click the instance
7. Switch to the YAML view and locate `spec.database` (or equivalent CR section for PostgreSQL configuration)
8. Remove or correct the invalid `nodeSelector` entry; save the CR
9. Navigate to Workloads → Pods; delete the Gateway operator manager pod (`aap-operator-controller-manager-*`) to trigger reconciliation
10. Delete the stale PostgreSQL pod so the operator creates a new one with the correct scheduling configuration
11. Monitor pod status until PostgreSQL reaches Running state and the AAP stack recovers
12. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- A `nodeSelector` in a Kubernetes pod spec that matches no node labels causes the pod to remain Pending indefinitely — the scheduler will not override it
- The OpenShift Console Events tab on a Pending pod surfaces the exact `FailedScheduling` reason, including which label was unmatched
- AAP operator-managed workloads (like PostgreSQL) are controlled via the AAP CR; direct pod edits are overwritten on the next reconciliation
- Deleting the operator manager pod is the correct way to trigger immediate reconciliation after a CR change

## Infrastructure Notes

- The Break Scenario job template injects an invalid `nodeSelector` into the AAP CR; the operator then propagates it to the PostgreSQL pod spec
- The cluster must have at least one worker node without the injected label (true of any standard cluster — the label is fabricated)
- Solve/Validate buttons are present on this scenario page
