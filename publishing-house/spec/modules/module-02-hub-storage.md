## Brief Overview

Scenario 1 presents a broken Automation Hub where pods are stuck in the Pending state because the underlying PersistentVolumeClaim was provisioned with the wrong access mode (ReadWriteOnce instead of ReadWriteMany). Students use the OpenShift Console exclusively — no CLI — to inspect pod events and PVC status, diagnose the root cause, and apply the fix through the AAP Custom Resource. This scenario reinforces how Kubernetes storage access modes directly affect multi-pod workloads.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift day-2 operations
- **Prerequisites**: Module 01 complete; basic familiarity with the OpenShift Console (Workloads → Pods, Storage → PVCs)
- **Estimated duration**: 20 minutes

## Learning Objectives

- Identify pod scheduling failures caused by PVC access mode mismatches using OpenShift Console pod events
- Explain why Automation Hub requires ReadWriteMany (RWX) storage for multi-pod deployments
- Apply the correct storage class (`ocs-storagecluster-cephfs`) in the AAP Custom Resource to resolve the issue
- Trigger operator reconciliation by deleting the AutomationHub CR, stale PVC, and operator manager pods

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
3. Click on a Pending pod and open the Events tab; note the FailedAttachVolume or FailedMount message
4. Navigate to Storage → PersistentVolumeClaims; inspect the access mode column and confirm it shows RWO
5. Open the AAP CR (Operators → Installed Operators → AAP → AnsibleAutomationPlatform instance)
6. Edit the CR YAML: set `spec.hub.file_storage_storage_class: ocs-storagecluster-cephfs`
7. Save the CR, then delete the AutomationHub CR to force the operator to recreate it cleanly
8. Delete the stale PVC from the Storage → PVCs view
9. Delete the operator manager pods (prefixed `aap-operator-controller-manager`) to force reconciliation
10. Monitor pod status until all Hub pods reach Running state
11. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- ReadWriteMany (RWX) storage is required for Automation Hub because multiple pods must mount the same volume simultaneously
- `ocs-storagecluster-cephfs` (ODF CephFS) provides RWX access mode; `ocs-storagecluster-ceph-rbd` provides only RWO
- Deleting stale PVCs and operator manager pods is the correct way to force the AAP operator to reconcile from a clean state
- Pod Events in the OpenShift Console are the first diagnostic tool for scheduling failures

## Infrastructure Notes

- Storage class `ocs-storagecluster-cephfs` must be available in the cluster (provided by OpenShift Data Foundation)
- The scenario namespace contains a pre-configured AAP CR with the incorrect storage class already set
- Solve/Validate buttons are present on this scenario page
