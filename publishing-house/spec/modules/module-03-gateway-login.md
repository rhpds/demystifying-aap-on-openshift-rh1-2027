## Brief Overview

Scenario 2 is the lab's first hard-difficulty scenario. Students begin by running the Break Scenario job template to introduce the fault, then attempt to log in to the AAP Gateway UI and encounter an authentication failure. Students use the OpenShift Console to inspect pod logs and identify the root cause, then apply the fix to restore access. This scenario practices using pod logs and OpenShift Console tooling to diagnose and resolve authentication failures in AAP.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift authentication and identity management
- **Prerequisites**: Module 01 complete; understanding of Kubernetes Secrets and pod logs; comfort navigating OpenShift Console
- **Estimated duration**: 25 minutes

## Learning Objectives

- Run a Break Scenario job template in AAP Controller to introduce a controlled fault
- Diagnose an AAP Gateway authentication failure using pod logs and the OpenShift Console
- Apply a fix to restore AAP Gateway authentication and verify successful login

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Reproduce the Failure | 4 min |
| 2 | Inspect Gateway Pod Logs | 6 min |
| 3 | Identify the Root Cause | 5 min |
| 4 | Apply the Fix | 7 min |
| 5 | Validate the Resolution | 3 min |

## Detailed Steps

1. Open the Scenario 2 Showroom tab and navigate to AAP Controller
2. Run the Break Scenario job template; wait for it to complete successfully
3. Attempt to log in to the AAP Gateway URL using admin credentials; confirm the authentication failure
4. Open the OpenShift Console; navigate to Workloads → Pods; filter by the scenario namespace
5. Click the Gateway pod and open the Logs tab; identify authentication-related error messages
6. Use the OpenShift Console to investigate the root cause of the authentication failure
7. Apply the appropriate fix to restore the correct admin credentials
8. Attempt to log in to the Gateway UI again and confirm success
9. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- Pod logs are the first place to look when a web application returns an authentication error
- Pod terminal access via the OpenShift Console is a powerful diagnostic and remediation tool that does not require `oc` CLI access
- AAP Gateway admin credentials can diverge from expected values; restoring them requires in-pod tooling

## Infrastructure Notes

- The Break Scenario job template must be run before the fault is active; without running it, the Gateway login works normally
- The Gateway pod must be in Running state before the terminal tab becomes usable
- Solve/Validate buttons are present on this scenario page
