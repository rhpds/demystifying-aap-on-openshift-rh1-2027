## Brief Overview

Scenario 1 presents a broken Automation Hub where pods are stuck in the Pending state because the PersistentVolumeClaim was provisioned with the wrong storage access mode. Students use the OpenShift Console exclusively to inspect pod events and PVC status, diagnose the root cause, and apply a fix through the AAP Custom Resource. This scenario reinforces how Kubernetes storage access modes directly affect multi-pod workloads.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift day-2 operations
- **Prerequisites**: Module 01 complete; basic familiarity with the OpenShift Console (Workloads → Pods, Storage → PVCs)
- **Estimated duration**: 20 minutes

## Learning Objectives

- Identify pod scheduling failures caused by PVC access mode mismatches using OpenShift Console pod events
- Understand why Automation Hub requires ReadWriteMany (RWX) storage for multi-pod deployments
- Apply a storage class fix to the AAP Custom Resource to resolve the Hub PVC issue
- Force operator reconciliation after applying the fix to restore Hub pods to Running state

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Observe the Broken State | 4 min |
| 2 | Inspect Pod Events and PVC Status | 5 min |
| 3 | Diagnose the Root Cause | 3 min |
| 4 | Apply the Fix via AAP CR | 5 min |
| 5 | Validate the Resolution | 3 min |

## Detailed Steps

1. Open the Scenario 1 Showroom tab and confirm that Automation Hub pods are stuck in Pending state
2. Navigate to Workloads → Pods in the OpenShift Console; filter by the scenario namespace
3. Click on a Pending pod and open the Events tab; identify the scheduling failure reason
4. Navigate to Storage → PersistentVolumeClaims; inspect the access mode and confirm the diagnosis
5. Open the AAP CR (Operators → Installed Operators → AAP → AnsibleAutomationPlatform instance)
6. Edit the CR YAML to set the correct Hub file storage class
7. Delete the AutomationHub CR to force the operator to recreate it cleanly
8. Delete the stale PVC from the Storage → PVCs view
9. Delete the operator manager pods to force reconciliation
10. Monitor pod status until all Hub pods reach Running state
11. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- ReadWriteMany (RWX) storage is required for Automation Hub because multiple pods must mount the same volume simultaneously
- Pod Events in the OpenShift Console are the first diagnostic tool for scheduling failures
- Deleting stale PVCs and operator manager pods is the correct way to force the AAP operator to reconcile from a clean state

## Infrastructure Notes

- The scenario namespace contains a pre-configured AAP CR with the incorrect storage class already set
- Solve/Validate buttons are present on this scenario page
