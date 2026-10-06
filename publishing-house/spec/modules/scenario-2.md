## Brief Overview

Scenario 2 is the lab's first hard-difficulty scenario. Students begin by running the Break Scenario job template to introduce the fault, then attempt to log in to the AAP Gateway UI and encounter an authentication failure. The root cause is an incorrect Gateway admin password stored in a Kubernetes Secret. Students use the OpenShift Console to inspect pod logs and service configurations, then fix the issue by exec-ing into the Gateway pod terminal and running the `aap-gateway-manage update_password` command. This scenario teaches how AAP Gateway credentials are managed within a Kubernetes-native deployment.

## Audience and Time

- **Target persona**: Platform engineers and administrators responsible for AAP on OpenShift authentication and identity management
- **Prerequisites**: Module 01 complete; understanding of Kubernetes Secrets and pod logs; comfort navigating OpenShift Console terminal features
- **Estimated duration**: 25 minutes

## Learning Objectives

- Run a Break Scenario job template in AAP Controller to introduce a controlled fault
- Correlate a login authentication failure with a misconfigured Kubernetes Secret using pod log inspection
- Reset the AAP Gateway admin password from within a running pod using `aap-gateway-manage update_password`
- Explain how AAP Gateway stores and reads admin credentials from Kubernetes Secrets

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Reproduce the Failure | 4 min |
| 2 | Inspect Gateway Pod Logs | 6 min |
| 3 | Locate the Admin Password Secret | 5 min |
| 4 | Reset the Password via Pod Terminal | 7 min |
| 5 | Validate the Resolution | 3 min |

## Detailed Steps

1. Open the Scenario 2 Showroom tab and navigate to AAP Controller
2. Run the Break Scenario job template; wait for it to complete successfully
3. Attempt to log in to the AAP Gateway URL using admin credentials — confirm the authentication failure
4. Open the OpenShift Console; navigate to Workloads → Pods; filter by the scenario namespace
5. Click the Gateway pod and open the Logs tab; identify authentication-related error messages
6. Navigate to Workloads → Secrets; search for the Gateway admin secret (e.g., `aap-gateway-admin-password`)
7. Decode the secret value and compare it with what the Gateway pod is using
8. Return to the Gateway pod; click the Terminal tab to open an interactive shell
9. Run: `aap-gateway-manage update_password --username admin --password <password-from-secret>`
10. Confirm the command exits successfully
11. Attempt to log in to the Gateway UI again and confirm success
12. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- AAP Gateway admin credentials are stored in a Kubernetes Secret; if the in-database value diverges from the Secret, login fails
- The `aap-gateway-manage update_password` command is the supported way to resynchronize the Gateway admin password without redeploying
- Pod terminal access via the OpenShift Console is a powerful diagnostic and remediation tool that does not require `oc` CLI access
- Pod logs are the first place to look when a web application returns an authentication error

## Infrastructure Notes

- The Break Scenario job template must be run before the fault is active; without running it, the Gateway login works normally
- The Gateway pod must be in Running state before the terminal tab becomes usable
- Solve/Validate buttons are present on this scenario page
