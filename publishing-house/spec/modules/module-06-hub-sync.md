## Brief Overview

Scenario 5 is the lab's most operationally rich easy-difficulty scenario, presenting two compounding Automation Hub failures that must both be resolved before the Community repository sync succeeds. Hub Content worker pods are OOMKilled because their memory limits are too low, and the Hub's PersistentVolumeClaim is only 25Mi — far too small for a real sync operation. Students learn to increase both memory limits and storage capacity through the AAP CR, and to perform a live PVC expansion directly in the OpenShift Console — a capability enabled by ODF CephFS's support for online volume expansion.

## Audience and Time

- **Target persona**: Platform engineers and AAP administrators responsible for Automation Hub capacity planning and resource tuning
- **Prerequisites**: Module 01 complete; familiarity with AAP Hub repository sync operations; basic understanding of Kubernetes resource requests/limits and PVC capacity
- **Estimated duration**: 25 minutes

## Learning Objectives

- Identify OOMKilled pods and diagnose memory limit misconfiguration using the OpenShift Console
- Update Automation Hub Content worker memory limits in the AAP Custom Resource
- Recognize a storage capacity failure in Hub sync job output and trace it to PVC size
- Expand a PersistentVolumeClaim live (without downtime) using the OpenShift Console YAML editor
- Update `spec.hub.file_storage_size` in the AAP CR to align the operator's desired state with the expanded PVC

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Trigger a Community Sync | 4 min |
| 2 | Diagnose OOMKilled Content Worker Pods | 6 min |
| 3 | Fix Memory Limits via AAP CR | 4 min |
| 4 | Diagnose PVC Storage Capacity Failure | 5 min |
| 5 | Expand the PVC and Update the AAP CR | 4 min |
| 6 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 5 Showroom tab; navigate to AAP Controller and run the Break Scenario job template
2. Open Automation Hub; navigate to Collections → Repository Management and trigger a sync of the Community repository
3. Observe the sync failure; switch to the OpenShift Console
4. Navigate to Workloads → Pods; filter by the scenario namespace and identify Content worker pods with `OOMKilled` status in the Last State column
5. Open the AAP CR (Operators → Installed Operators → AAP → AnsibleAutomationPlatform instance); switch to YAML view
6. Locate `spec.hub.content.resource_requirements`; set the memory limit to `8Gi` and save
7. Delete the OOMKilled Content worker pods to allow the operator to recreate them with the new limits
8. Re-trigger the Community sync in Hub; note the new failure — this time a storage capacity error
9. Navigate to Storage → PersistentVolumeClaims in the OpenShift Console; find the Hub file storage PVC (currently 25Mi)
10. Click the PVC; switch to YAML view; change `spec.resources.requests.storage` to `10Gi`; save — ODF CephFS performs the expansion online
11. Return to the AAP CR; update `spec.hub.file_storage_size: 10Gi` to reconcile the operator's desired state
12. Re-trigger the Community sync and confirm it completes successfully
13. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- Two independent issues can compound to prevent a sync from completing; fixing only one reveals the second
- OOMKilled pods signal that the container memory limit is below the workload's actual peak usage; AAP Hub content workers are memory-intensive during large syncs
- ODF CephFS supports online PVC expansion — storage can be increased without pod restarts or data migration
- Both the PVC itself and the AAP CR's `file_storage_size` field must be updated; the CR governs the operator's desired state going forward

## Infrastructure Notes

- Storage class `ocs-storagecluster-cephfs` must be configured to allow volume expansion (`allowVolumeExpansion: true` in the StorageClass spec)
- The initial 25Mi PVC size is deliberately set too small to simulate the failure; 10Gi is a realistic minimum for a Community sync workload
- Memory limit of 8Gi for Hub Content workers is the tested minimum for this lab's sync payload; production values will vary
- Solve/Validate buttons are present on this scenario page
