# Demystifying Ansible Automation Platform on OpenShift Container Platform

## Overview

This lab gives AAP administrators hands-on experience diagnosing and recovering
a production-style Ansible Automation Platform deployment running on OpenShift
Container Platform. Six pre-broken AAP instances — one per scenario — present
realistic failure conditions drawn from common operator misconfiguration and
resource constraint patterns. Participants use the OpenShift Console and AAP UI
to inspect pod events, logs, and Custom Resource definitions, then apply
targeted fixes and validate recovery.

## Target Audience

- **Role:** AAP administrators and platform engineers responsible for deploying
  or operating Ansible Automation Platform on OpenShift
- **Experience level:** Intermediate
- **What they already know:** Basic navigation of the OpenShift Console,
  familiarity with Kubernetes primitives (Pods, PersistentVolumeClaims, Events,
  Secrets), and day-to-day AAP operation outside of OpenShift
- **What they don't know:** How the AAP Operator manages component lifecycles,
  how AAP Custom Resource fields affect scheduling and storage, how Gateway
  credentials and Lightspeed configuration are stored and reset in an
  OpenShift-native deployment

## Prerequisites

- Ability to navigate the OpenShift Console (Workloads, Storage, Events views)
- Awareness of Kubernetes pod scheduling concepts (node selectors, resource
  requests, PVC access modes)
- Basic familiarity with Ansible Automation Platform components (Hub,
  Controller, Gateway) — no OCP-specific AAP experience required
- Cannot be validated automatically; the Welcome module sets expectations but
  does not gate entry

## Learning Objectives

1. Troubleshoot Automation Hub pod scheduling and resource failures by
   correcting the StorageClass, PVC access mode, memory limits, and PVC
   capacity in the AAP Custom Resource.
2. Restore Automation Gateway authentication by diagnosing pod log output to
   identify credential mismatches and resetting the admin password using
   in-cluster management tooling.
3. Diagnose and resolve Kubernetes pod scheduling failures caused by an invalid
   node selector in the AAP Custom Resource database configuration.
4. Verify Execution Environment job template configurations and resolve private
   registry authentication failures by updating the Execution Environment
   assignment on failing templates.
5. Troubleshoot Ansible Lightspeed (ALIA) service failures by inspecting pod
   logs and Kubernetes Secrets to identify unauthorized LLM model
   configurations and restore connectivity to an external LLM endpoint.

## Content Type

Lab (hands-on)

## Products & Technologies

- Red Hat Ansible Automation Platform (AAP Operator, Automation Hub,
  Automation Gateway, Automation Controller)
- Ansible Lightspeed Intelligent Assistant (ALIA) / Lightspeed Operator
- Red Hat OpenShift Container Platform
- Red Hat OpenShift Data Foundation (CephFS RWX storage)
- PostgreSQL 15 (managed by AAP Operator)
- Model-as-a-Service (MaaS) — external LLM endpoint for ALIA integration

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Welcome and Lab Credentials | 10 min |
| 2 | Scenario 1: Automation Hub Storage Issue | 20 min |
| 3 | Scenario 2: Gateway and Controller Login Issues | 25 min |
| 4 | Scenario 3: Node Selector and Resource Issues | 20 min |
| 5 | Scenario 4: Execution Environment Credentials | 20 min |
| 6 | Scenario 5: Automation Hub Sync Issues | 25 min |
| 7 | Scenario 6: ALIA Lightspeed Issues | 30 min |
| 8 | Conclusion | 5 min |
| — | **Total hands-on** | **~155 min (~2.5 hours)** |

Each scenario module (2–7) has a companion solution reference sub-page; these
are not separately timed in the map.

## Difficulty Level

Intermediate

## Environment

**Learner view:** When the lab starts, participants land in a pre-provisioned
shared OpenShift cluster with a dedicated namespace already created for them.
The namespace contains six independently deployed AAP instances — one per
scenario — each pre-broken by the provisioning automation. The Showroom sidebar
provides direct links to the OpenShift Console and to each scenario's AAP UI.
Scenario modules include a Run Break Scenario button (Scenarios 2–6) that
triggers the break job for that instance, followed by investigation and fix
steps, and a Validate button to confirm recovery. No cluster-admin access is
required; all work is scoped to the student namespace and performed through the
OpenShift Console GUI and AAP UI, with a single exception (a pod terminal
command in Scenario 2).

**Automation needed:** Yes

The provisioning automation must:
- Create one namespace per student on the shared OCP cluster
- Deploy six AAP Operator instances in each namespace (one per scenario),
  including Automation Hub, Automation Gateway, Automation Controller, and
  Lightspeed Operator
- Provision ODF-backed CephFS PVCs for Automation Hub instances
- Pre-configure each AAP instance for its scenario (valid baseline state)
- Expose each scenario's AAP UI route and configure Showroom tab links
- Configure the external MaaS LLM endpoint and ALIA chatbot credentials for
  Scenario 6
- Provide Break Scenario and Validate job templates within each AAP instance

## Infrastructure Requirements

- **Cloud provider:** TBD — confirmed in infrastructure phase
- **Cluster type:** TBD — confirmed in infrastructure phase
- **OCP version:** TBD — confirmed in infrastructure phase
- **Topology:** TBD — confirmed in infrastructure phase
- **Sizing:** TBD — confirmed in infrastructure phase
- **Automation approach:** TBD — confirmed in infrastructure phase
- **AI/MaaS:** TBD — confirmed in infrastructure phase
- **External services:** TBD — confirmed in infrastructure phase
- **AAP version:** TBD — confirmed in infrastructure phase
- **Non-GA products:** TBD — confirmed in infrastructure phase

## Assessment Strategy

This is a Zero-Touch lab. Each of the six scenario modules (Scenarios 1–6)
includes automated solve and validate buttons:

- **Solve** applies the correct fix to the broken AAP instance for participants
  who are stuck or want to compare their approach against the reference solution.
- **Validate** runs an automated check confirming the environment has been
  restored to a working state and returns a pass/fail result in the Showroom UI.

Successful validation on all six scenario modules constitutes completion of the
lab. The Welcome and Conclusion modules do not have validation gates.
