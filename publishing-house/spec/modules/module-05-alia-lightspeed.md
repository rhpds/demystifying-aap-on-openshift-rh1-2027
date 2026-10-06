## Brief Overview

Scenario 4 is the lab's second hard-difficulty scenario and covers ALIA — the Ansible Lightspeed AI assistant integrated into the AAP UI. After running the Break Scenario job template, the ALIA chat icon disappears from the AAP Gateway UI because the Lightspeed component has been configured with an unauthorized model (`codellama-7b-instruct`) that the external MaaS LLM endpoint rejects. Students inspect Lightspeed API pod logs and chatbot pod logs through the OpenShift Console, decode the `chatbot-configuration-secret`, identify the incorrect model value, patch the secret, and force reconciliation. This scenario teaches how AAP Lightspeed reads its LLM configuration from Kubernetes Secrets and how to recover from a misconfigured AI backend.

## Audience and Time

- **Target persona**: Platform engineers and AAP administrators responsible for configuring and maintaining ALIA / Ansible Lightspeed integrations
- **Prerequisites**: Module 01 complete; familiarity with Kubernetes Secrets (base64 encoding/decoding); understanding of how LLM endpoints and model identifiers work
- **Estimated duration**: 30 minutes

## Learning Objectives

- Run a Break Scenario job template and observe a missing ALIA chat icon in the AAP Gateway UI
- Inspect Lightspeed API and chatbot pod logs to identify an unauthorized model error from the MaaS endpoint
- Decode and inspect the `chatbot-configuration-secret` Kubernetes Secret to find the misconfigured model key
- Edit the Secret to set the correct model value (`{alia_model}`) and force Lightspeed reconciliation by deleting the relevant pods
- Explain how AAP Lightspeed reads LLM endpoint configuration (URL, model, token) from Kubernetes Secrets

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Run Break Scenario and Confirm ALIA Disappears | 4 min |
| 2 | Inspect Lightspeed API Pod Logs | 7 min |
| 3 | Inspect Chatbot Pod Logs | 5 min |
| 4 | Decode and Inspect the Configuration Secret | 6 min |
| 5 | Patch the Secret and Force Reconciliation | 6 min |
| 6 | Validate the Resolution | 2 min |

## Detailed Steps

1. Open the Scenario 4 Showroom tab; navigate to AAP Controller and run the Break Scenario job template
2. Open the AAP Gateway UI and confirm the ALIA chat icon is no longer visible in the navigation bar
3. Switch to the OpenShift Console; navigate to Workloads → Pods; filter by the scenario namespace
4. Find the `lightspeed-api-*` pod; open the Logs tab and look for errors related to the LLM endpoint (unauthorized model or 403 responses from the MaaS API)
5. Find the `lightspeed-chatbot-api-*` pod; open the Logs tab and note any model validation or connection errors
6. Navigate to Workloads → Secrets; search for `chatbot-configuration-secret`
7. Open the secret; decode the `chatbot_model` key value and confirm it is set to `codellama-7b-instruct` (the unauthorized model)
8. Also note the `chatbot_url` and `chatbot_token` keys — these reference the MaaS API endpoint (`{alia_url}`) and auth token (`{alia_token}`)
9. Click Edit (or switch to YAML view); update the `chatbot_model` key to the correct value (`{alia_model}`); save the secret
10. Delete the `ansible-lightspeed-operator-controller-manager-*` pod to trigger operator reconciliation
11. Delete the `lightspeed-chatbot-api-*` pod so it restarts and reads the updated secret
12. Delete the `lightspeed-api-*` pod so it restarts with the correct model configuration
13. Wait for all three pods to return to Running state
14. Reload the AAP Gateway UI and confirm the ALIA chat icon reappears
15. Click the Validate button to confirm the scenario is resolved

## Key Takeaways

- ALIA (Ansible Lightspeed AI Assistant) reads its LLM endpoint configuration — URL, model identifier, and auth token — from a Kubernetes Secret at startup; a wrong model value causes the external MaaS API to reject all requests
- The symptom (missing chat icon) is a UI-level indicator of a backend LLM connectivity failure, not a networking issue
- Pod logs are essential for tracing which component (chatbot API vs. Lightspeed API) first encounters the error
- Three pods must be recycled after the Secret is updated because none of them watch for Secret changes at runtime — they read configuration only at startup
- Kubernetes Secrets are the canonical configuration store for Lightspeed; do not edit the operator CR directly for model configuration changes

## Infrastructure Notes

- The external LLM is served via a MaaS (Model-as-a-Service) API at `{alia_url}`; the token `{alia_token}` is pre-provisioned in the lab environment
- The authorized model identifier is `{alia_model}`; the Break Scenario job replaces this with `codellama-7b-instruct`
- All three pods (`ansible-lightspeed-operator-controller-manager`, `lightspeed-chatbot-api`, `lightspeed-api`) must be deleted in this order for a clean reconciliation
- Solve/Validate buttons are present on this scenario page
