## Brief Overview

Scenario 4 is the lab's second hard-difficulty scenario and covers ALIA, the Ansible Lightspeed AI assistant integrated into the AAP UI. After running the Break Scenario job template, the ALIA chat icon remains visible but the assistant does not respond to messages. Students inspect Lightspeed pod logs through the OpenShift Console, identify the misconfiguration in the chatbot secret, and apply a fix to restore ALIA functionality. This scenario practices log-based diagnosis and Kubernetes Secret inspection in the context of an AI integration.

## Audience and Time

- **Target persona**: Platform engineers and AAP administrators responsible for configuring and maintaining ALIA / Ansible Lightspeed integrations
- **Prerequisites**: Module 01 complete; familiarity with Kubernetes Secrets; understanding of how LLM endpoints work
- **Estimated duration**: 30 minutes

## Learning Objectives

- Run a Break Scenario job template and observe the ALIA chat icon present but unresponsive
- Inspect Lightspeed pod logs to identify a configuration error preventing the assistant from responding
- Locate and inspect the chatbot configuration Kubernetes Secret to find the misconfigured value
- Apply a fix to the Secret and restore ALIA functionality

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Confirm ALIA is Unresponsive | 4 min |
| 2 | Inspect Lightspeed Pod Logs | 7 min |
| 3 | Inspect the Configuration Secret | 6 min |
| 4 | Apply the Fix | 10 min |
| 5 | Validate the Resolution | 3 min |

## Detailed Steps

1. Open the Scenario 4 Showroom tab; navigate to AAP Controller and run the Break Scenario job template
2. Open the AAP Gateway UI; confirm the ALIA chat icon is visible but that messages receive no response
3. Switch to the OpenShift Console; navigate to Workloads → Pods; filter by the scenario namespace
4. Find the Lightspeed API pod; open the Logs tab and look for errors related to the LLM endpoint
5. Navigate to Workloads → Secrets; find the chatbot configuration secret
6. Inspect the secret to identify the misconfigured value
7. Edit the secret to set the correct value; save
8. Delete the relevant Lightspeed pods to force them to restart and pick up the updated secret
9. Wait for all pods to return to Running state
10. Reload the AAP Gateway UI and confirm ALIA is now responding
11. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- ALIA reads its LLM configuration from a Kubernetes Secret at startup; a wrong value causes the external endpoint to reject requests
- The symptom (chat icon present but unresponsive) is a UI-level indicator of a backend LLM connectivity failure
- Pod logs are essential for tracing which component first encounters the error
- Pods must be restarted after a Secret is updated because they read configuration only at startup

## Infrastructure Notes

- The external LLM is served via a MaaS (Model-as-a-Service) API at `{alia_url}`; the token `{alia_token}` is pre-provisioned in the lab environment
- The authorized model identifier is `{alia_model}`
- Solve/Validate buttons are present on this scenario page
