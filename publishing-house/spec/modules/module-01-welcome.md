## Brief Overview

This orientation module introduces students to the Demystifying AAP on OpenShift lab environment. Students learn how the lab is structured around a break-and-fix model: each namespace contains six pre-broken AAP instances, one per scenario. Showroom tabs provide direct browser access to each scenario's AAP UI and the shared OpenShift Console. No hands-on troubleshooting occurs in this module — it exists solely to orient students before they begin the scenarios.

## Audience and Time

- **Target persona**: Platform engineers, automation architects, and administrators who manage Ansible Automation Platform on OpenShift
- **Prerequisites**: Familiarity with AAP concepts (Hub, Controller, Gateway) and basic OpenShift navigation; no CLI access is required for any scenario
- **Estimated duration**: 10 minutes

## Learning Objectives

- Locate and use the Showroom tabs to access each scenario's AAP UI and the OpenShift Console
- Identify the lab credential sets for AAP (admin / {password}) and OpenShift ({user} / {password})
- Understand the break-and-fix workflow: each scenario has its own pre-broken AAP instance, a Break Scenario job template (where applicable), and Solve/Validate buttons

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1 | Lab Architecture Overview | 3 min |
| 2 | Showroom Tab Tour | 3 min |
| 3 | Credential Reference Tables | 4 min |

## Key Takeaways

- Each of the six scenarios runs in an isolated namespace with its own pre-broken AAP deployment
- Showroom tabs eliminate the need for copy-pasting URLs; all scenario UIs are one click away
- AAP credentials follow the pattern `admin / {password}`; OpenShift credentials are `{user} / {password}`
- No validation gate exists for this module — students proceed directly to Scenario 1
