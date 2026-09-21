# Module 1 — Intelligent OpenShift

## Brief Overview

OpenShift 5 ships with an MCP (Model Context Protocol) server and gateway that expose live cluster data as AI-callable tools. This module connects an AI client to that gateway, exercises agentic troubleshooting against a pre-broken workload, generates and refines a Perses monitoring dashboard via natural language prompts, and reviews an AI-produced upgrade risk report. Together these four sections demonstrate how AI-assisted operations are a first-class feature of OCP 5, not an add-on.

## Audience and Time

- **Target personas:** Solutions Architects, Technical Sales, Field Engineers
- **Prerequisites for this module:** Basic OCP 4 operational experience; AI client tooling (e.g., a local MCP-compatible client) available in the lab environment; MaaS endpoint pre-configured
- **Estimated duration:** 35 minutes

## Learning Objectives

- Connect an AI client to the OpenShift 5 MCP gateway and invoke live tool calls against a running cluster
- Interpret gateway routing behavior and understand which tools the MCP server exposes
- Deploy and drive an agentic troubleshooting workflow that detects a failing workload, gathers diagnostic data, and proposes a remediation
- Apply an AI-recommended fix and verify workload recovery
- Generate a Perses monitoring dashboard scoped to a specific workload using natural language prompts
- Apply the AI-generated dashboard YAML to the cluster and inspect live metrics
- Refine the dashboard through iterative prompting
- Interpret an AI-assisted upgrade risk report including blocking conditions, warnings, and recommended actions

## Lab Structure

| Section | Title | Duration |
|---------|-------|----------|
| 1.1 | MCP Server and Gateway | ~8 min |
| 1.2 | Agentic Troubleshooting | ~12 min |
| 1.3 | On-Demand Perses Dashboards | ~10 min |
| 1.4 | Update Risk and Status Analysis | ~5 min |

## Detailed Steps

### Section 1.1 — MCP Server and Gateway

1. Open the AI client tool provided in the lab environment.
2. Configure the client to point at the pre-exposed OpenShift MCP gateway URL (provided in the lab credentials panel).
3. Authenticate the client using the lab-provided credentials.
4. Execute a live tool call: list all nodes in the cluster. Observe the structured response returned by the MCP server.
5. Execute a second tool call: describe the pre-broken pod (provided in the lab workload namespace). Note how the gateway routes the request to the correct cluster resource.
6. Execute a third tool call: query recent events for the broken pod's namespace.
7. Observe that the AI client receives structured, contextual cluster data without needing direct `oc` access for these queries.
8. Review the gateway routing explanation in the lab guide — understand which tools are exposed and how requests are dispatched.

### Section 1.2 — Agentic Troubleshooting

1. Confirm the pre-broken workload is running in the lab namespace (the lab environment provisions a deployment with CrashLoopBackOff and misconfigured resource limits).
2. Use the AI client to invoke the agentic troubleshooting workflow against the broken workload.
3. Observe the agent's detection step: it identifies the CrashLoopBackOff condition and flags the misconfigured resource limits.
4. Watch the agent gather diagnostic data: logs, events, and resource metrics for the affected pod.
5. Review the agent's proposed remediation — corrected resource limit values — presented in the AI client output.
6. Apply the recommended fix using `oc` or via the console (as directed by the lab guide).
7. Monitor the workload recovery: watch the pod restart and reach a Running state.
8. Confirm recovery by running a final agent status check against the workload.

### Section 1.3 — On-Demand Perses Dashboards

1. In the AI client, submit a natural language prompt requesting a Perses dashboard scoped to the lab workload namespace (example prompt is provided in the lab guide).
2. Review the AI-generated dashboard YAML returned by the agent.
3. Inspect the dashboard definition: note the data sources, panel types, and label selectors used.
4. Apply the dashboard YAML to the cluster using `oc apply`.
5. Open the OpenShift console and navigate to the Observe section to locate the new dashboard.
6. Inspect the live metrics being displayed for the lab workload.
7. Submit a follow-up refinement prompt to the AI client (e.g., requesting an additional panel or adjusted time range).
8. Apply the refined YAML and verify the updated dashboard reflects the requested changes.

### Section 1.4 — Update Risk and Status Analysis

1. Use the AI client to invoke the upgrade risk scan tool against the pre-deployed cluster.
2. Wait for the AI-generated upgrade risk report to be returned (the cluster is pre-configured with a mix of Operators for this exercise).
3. Review the report structure: blocking conditions, warnings, and recommended actions.
4. Identify any blocking conditions listed in the report and note what remediation the report recommends.
5. Confirm the report reflects the cluster's actual Operator state by cross-referencing with `oc get clusteroperators`.

## Key Takeaways

- The OCP 5 MCP server exposes live cluster data as structured, AI-callable tools, enabling AI clients to query and act on cluster state without raw API access
- Agentic troubleshooting workflows automate the detection-diagnosis-remediation loop, reducing mean time to resolution for common failure patterns
- Perses dashboard generation via natural language prompts eliminates the need to hand-author dashboard YAML for routine workload monitoring
- AI-assisted upgrade risk reports surface blocking conditions and sequenced remediation steps before a customer begins an upgrade, reducing risk

## Infrastructure Notes

- The MCP gateway must be pre-exposed and reachable from the learner's AI client session at lab start
- The MaaS (Model-as-a-Service) endpoint must be pre-configured and accessible; the lab environment provides this via the Red Hat OpenShift AI integration
- The broken workload (CrashLoopBackOff + misconfigured resource limits) must be provisioned by lab automation before Module 1.2 begins — participants do not create it themselves
- The Perses integration must be enabled in the OCP 5 console at lab start
- All Operator content required for the risk report (Section 1.4) must be installed on the cluster before the lab begins
