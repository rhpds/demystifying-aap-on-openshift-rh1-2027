# Demystifying Ansible Automation Platform on OpenShift Container Platform

## Overview

This lab gives AAP administrators hands-on experience diagnosing and recovering
a production-style Ansible Automation Platform deployment running on OpenShift
Container Platform. All four scenarios are drawn from real AAP failure patterns
observed by Red Hat Support and Services in customer environments. While some
fixes involve OpenShift primitives — pod logs, storage configuration, Kubernetes
Secrets — those are simply the diagnostic layer for an AAP problem; the subject
of every scenario is the AAP deployment itself. Participants use the OpenShift
Console and AAP UI to inspect pod events, logs, and Custom Resource definitions,
then apply targeted fixes and validate recovery.

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

1. Troubleshoot Automation Hub pod scheduling failures by identifying an
   incompatible StorageClass and correcting the PVC access mode in the AAP
   Custom Resource.
2. Restore Automation Gateway authentication by diagnosing pod log output to
   identify credential mismatches and resetting the admin password using
   in-cluster management tooling.
3. Diagnose and resolve Kubernetes pod scheduling failures caused by an invalid
   node selector in the AAP Custom Resource database configuration.
4. Troubleshoot Ansible Lightspeed (ALIA) service failures by inspecting pod
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
| 5 | Scenario 4: ALIA — Ansible Lightspeed Issues | 30 min |
| 6 | Conclusion | 5 min |
| — | **Total** | **~110 min (~1.75 hours)** |

Each scenario module (2–5) has a companion solution reference sub-page; these
are not separately timed in the map.

### Optional / Bonus Modules (nice to have)

Two additional scenarios are already fully developed and could be included as
optional content for attendees who finish the four core scenarios early:

| Module | Title | Duration |
|--------|-------|----------|
| — | Scenario 5: Execution Environment Credentials | ~20 min |
| — | Scenario 6: Automation Hub Sync Issues | ~25 min |

These were removed from the required path to keep the core lab within ~90
minutes, which matches observed completion times. If included, they would be
clearly marked as optional in the Showroom UI and would not be required for
the Validate completion gate.

## Difficulty Level

Intermediate

## Environment

**Learner view:** When the lab starts, participants land in a pre-provisioned
shared OpenShift cluster with a dedicated namespace already created for them.
The namespace contains four independently deployed AAP instances — one per
scenario — each pre-broken by the provisioning automation. The Showroom sidebar
provides direct links to the OpenShift Console and to each scenario's AAP UI.
Scenario modules include a Run Break Scenario button (Scenarios 2–4) that
triggers the break job for that instance, followed by investigation and fix
steps, and a Validate button to confirm recovery. No cluster-admin access is
required; all work is scoped to the student namespace and performed through the
OpenShift Console GUI and AAP UI, with a single exception (a pod terminal
command in Scenario 2).

**Automation needed:** Yes

The provisioning automation must:
- Create one namespace per student on the shared OCP cluster
- Deploy four AAP Operator instances in each namespace (one per scenario),
  including Automation Hub, Automation Gateway, Automation Controller, and
  Lightspeed Operator
- Provision ODF-backed CephFS PVCs for Automation Hub instances
- Pre-configure each AAP instance for its scenario (valid baseline state)
- Expose each scenario's AAP UI route and configure Showroom tab links
- Configure the external MaaS LLM endpoint and ALIA chatbot credentials for
  Scenario 4
- Provide Break Scenario and Validate job templates within each AAP instance

## Infrastructure Requirements

- **Cloud provider:** CNV
- **Cluster type:** Multinode OCP (shared cluster)
- **OCP version:** 4.20 minimum
- **Topology:** Shared-cluster, 60 max concurrent users
- **Sizing:** TBD — infra reviewer to size for 60 simultaneous students, each with 6 AAP
  Operator deployments (Hub, Gateway, Controller, Lightspeed) in their own namespace plus
  ODF-backed CephFS PVCs
- **Automation approach:** Both (Ansible + GitOps)
- **AI/MaaS:** MaaS, open-source model (external LLM endpoint for ALIA/Lightspeed in Scenario 6)
- **External services:** MaaS LLM endpoint (provisioned and used during student session for Scenario 6)
- **AAP version:** 2.7
- **Non-GA products:** None (all products are GA)

## Assessment Strategy

This is a Zero-Touch lab. Each of the four scenario modules (Scenarios 1–4)
includes automated solve and validate buttons:

- **Solve** applies the correct fix to the broken AAP instance for participants
  who are stuck or want to compare their approach against the reference solution.
- **Validate** runs an automated check confirming the environment has been
  restored to a working state and returns a pass/fail result in the Showroom UI.

Successful validation on all four scenario modules constitutes completion of the
lab. The Welcome and Conclusion modules do not have validation gates.
