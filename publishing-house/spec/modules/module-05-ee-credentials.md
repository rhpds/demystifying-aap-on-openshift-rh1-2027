## Brief Overview

Scenario 4 demonstrates one of the most common AAP day-2 failures: a job template referencing a private container registry for its Execution Environment image, but with no Container Registry Credential configured. Students run the Break Scenario job template, then launch the available job templates to identify which one fails. They trace the failure from AAP job output through OpenShift pod logs to confirm an image pull error from a private Quay registry. The fix is straightforward — switching the failing job template's EE to the Default Execution Environment — and reinforces how AAP manages private registry authentication.

## Audience and Time

- **Target persona**: Automation architects and AAP administrators responsible for Execution Environment management and credential configuration
- **Prerequisites**: Module 01 complete; familiarity with AAP job templates, Execution Environments, and the concept of Container Registry Credentials
- **Estimated duration**: 20 minutes

## Learning Objectives

- Launch multiple job templates and identify which one fails due to an image pull error
- Trace an EE image pull failure from AAP job output to OpenShift pod logs
- Explain the relationship between Container Registry Credentials and private EE image access in AAP
- Fix the misconfigured job template by switching its EE to the Default Execution Environment

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario | 2 min |
| 2 | Launch Job Templates and Identify the Failure | 5 min |
| 3 | Inspect Job Output and Pod Logs | 6 min |
| 4 | Fix the Job Template EE Configuration | 5 min |
| 5 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 4 Showroom tab; navigate to AAP Controller
2. Run the Break Scenario job template and wait for it to complete
3. Navigate to Resources → Templates; identify all available job templates
4. Launch each job template and observe which one fails (the SUMMIT-T4 job template)
5. Open the failed job's output in AAP; look for the image pull error message referencing a private Quay registry
6. Switch to the OpenShift Console; navigate to Workloads → Pods and find the failed runner pod
7. Open the pod's Events or Logs tab to confirm the `ImagePullBackOff` or `ErrImagePull` error and the private registry URL
8. Return to AAP Controller; navigate to Resources → Templates → SUMMIT-T4
9. Click Edit; locate the Execution Environment field
10. Change the EE from the private registry image to "Default Execution Environment"
11. Save the template; re-launch SUMMIT-T4 and confirm it completes successfully
12. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- AAP pulls Execution Environment images from container registries at job runtime; if the registry requires authentication and no Container Registry Credential is attached, the pull fails
- The Default Execution Environment image is hosted on a publicly accessible registry and does not require credentials
- Job output in AAP Controller is the first diagnostic surface; OpenShift pod events provide the lower-level container runtime error
- Container Registry Credentials in AAP are the correct long-term fix; switching to a public EE is an immediate workaround

## Infrastructure Notes

- The private Quay registry referenced by SUMMIT-T4 is intentionally inaccessible from the lab cluster (no credential exists to provide)
- The Default Execution Environment is available and accessible from all lab namespaces
- Solve/Validate buttons are present on this scenario page
