# Red Hat OpenShift 5: First Look

<!-- This file is the design document for your lab or demo. -->
<!-- Fill in each section below, or run /rhdp-publishing-house to have the intake skill help. -->
<!-- Sections marked with [brackets] are placeholders — replace with real content. -->
<!-- The validation gate checks for all required sections before submission. -->

## Overview

This is a two-hour hands-on lab for Solutions Architects, Technical Sales, and Field Engineers who need to understand and demonstrate the key new capabilities of Red Hat OpenShift 5. OCP 5 introduces AI-assisted operations (MCP server, agentic troubleshooting, AI-generated dashboards), a redesigned upgrade path, RHCOS 10, and expanded security capabilities including post-quantum cryptography. Participants will connect an AI client to the OpenShift MCP gateway, drive an agentic troubleshooting workflow on a broken workload, generate and refine monitoring dashboards via natural language, analyze AI-produced upgrade risk reports, walk through a disconnected OCP 5 install, configure post-quantum crypto on the API server and ingress, and mirror Operator catalog content using oc mirror v2's delta capabilities.

## Target Audience

- **Role:** Solutions Architects, Technical Sales, Field Engineers
- **Experience level:** Intermediate
- **What they already know:** OpenShift 4 administration concepts (deploying workloads, managing operators, oc CLI), familiarity with Kubernetes fundamentals
- **What they don't know:** OpenShift 5 architecture changes, the MCP server and agentic operations model, RHCOS 10 lifecycle, oc mirror v2, post-quantum cryptography in OCP

## Prerequisites

- OpenShift 4 operational experience (deploying and managing workloads, using oc CLI)
- Basic familiarity with Kubernetes concepts (pods, operators, nodes)
- No OCP 5 experience required
- Prerequisites are trust-based — no automated pre-check is planned for this classic lab

## Learning Objectives

1. Explore the OpenShift 5 MCP server by connecting an AI client and invoking live tool calls against a running cluster
2. Deploy an agentic troubleshooting workflow to diagnose a failing workload and apply AI-recommended remediation
3. Create an AI-generated Perses monitoring dashboard using natural language prompts and refine it iteratively
4. Analyze an AI-assisted upgrade risk report and Operator compatibility plan for an OCP 4→5 migration scenario
5. Verify a disconnected OCP 5 installation using pre-staged, air-gapped content
6. Configure post-quantum cryptography cipher suites on the OCP 5 API server and ingress controller
7. Demonstrate oc mirror v2 delta mirroring to selectively synchronize Operator catalog content

## Content Type

Lab (hands-on)

## Products & Technologies

- Red Hat OpenShift Container Platform 5 (primary — pre-GA, GA expected October 2026)
- Red Hat OpenShift AI (MaaS endpoint for agentic operations in Module 1)
- Red Hat Lightwell (Module 4.3 — placeholder, pending content confirmation)
- Perses (upstream monitoring framework, Module 1.3 — used via OCP 5 console integration)
- oc mirror v2 (OCP tooling, Module 4.1)

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Intelligent OpenShift | 35 min |
| 2 | Install & Upgrade | 30 min |
| 3 | RHCOS 10 | 15 min |
| 4 | Security, Sovereign & Disconnected | 20 min |
| 5 | Multi-Cluster *(placeholder)* | TBD |
| 6 | Other Changes in OpenShift 5 *(placeholder)* | TBD |
| — | **Total confirmed hands-on** | **~100 min** |
| — | Intro / orientation | ~10 min |
| — | **Total confirmed lab** | **~2 hours** |

> **Note:** Modules 5 and 6 (and sub-items 2.4, 2.5, 4.3) are reserved slots pending content confirmation with engineering. The confirmed lab runs ~2 hours; total duration will increase when placeholders are filled in.

## Difficulty Level

Intermediate

## Environment

**Learner view:** When the lab starts, participants have access to a pre-deployed OCP 5 cluster with:
- A sample workload in a broken state (CrashLoopBackOff + misconfigured resource limits) for Module 1.2
- The OpenShift MCP gateway configured and exposed, ready for AI client connection
- A MaaS endpoint pre-configured and accessible for agentic operations
- Pre-staged disconnected content mirrored to an internal registry for Module 2.3
- All required Operators installed

**Automation needed:** Yes — automation must provision the broken workload (Module 1.2), configure the MaaS endpoint, pre-mirror content for the air-gapped install walkthrough (Module 2.3), and stage any Module 2.4/2.5 placeholder resources when content is confirmed.

## Infrastructure Requirements

- **Cloud provider:** CNV
- **Cluster type:** Multinode
- **OCP version:** 5.0 *(confirm exact version string at GA)*
- **Topology:** Per-student
- **Sizing:** 3 control plane nodes (16 vCPU, 64GB RAM each); 3 worker nodes (16 vCPU, 64GB RAM, 200GB disk each)
- **Automation approach:** Ansible + GitOps (both)
- **AI/MaaS:** MaaS, open-source model
- **External services:** `registry.redhat.io`, `registry.access.redhat.com`, MaaS endpoint (TBD — confirm URL before submission)
- **AAP version:** N/A
- **Non-GA products:** Red Hat OpenShift Container Platform 5 (GA expected October 2026) — access via specific engineering build

## Assessment Strategy (Optional)

Trust-based — this is a classic Showroom lab with no automated solve/validate buttons. Completion is self-reported. Per-module verification is through visible UI results and terminal output that participants can observe and confirm during the lab.
