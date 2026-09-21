# Module 3 — RHCOS 10

## Brief Overview

Red Hat CoreOS 10 (RHCOS 10) introduces an image-based lifecycle model powered by bootc, replacing the former rpm-ostree approach. This module gives participants hands-on inspection of node OS details on a post-install OCP 5 cluster, walks through the key changes in RHCOS 10, and demonstrates a day-2 OS update flow that does not require full node reprovisioning. Immutability guarantees and compliance implications are examined as part of the day-2 discussion.

## Audience and Time

- **Target personas:** Solutions Architects, Technical Sales, Field Engineers
- **Prerequisites for this module:** Completion of the orientation section; basic `oc` CLI familiarity; no prior RHCOS 10 or bootc experience required
- **Estimated duration:** 15 minutes

## Learning Objectives

- Inspect node OS details on an OCP 5 cluster using `oc debug node` and identify RHCOS 10 specifics
- Explain the key differences between RHCOS 10's image-based lifecycle and the prior rpm-ostree model, including bootc integration
- Describe the day-2 OS update flow in RHCOS 10 and why it does not require full node reprovisioning
- Articulate the immutability guarantees provided by RHCOS 10 and their relevance to compliance and security posture

## Lab Structure

Four sections, approximately 15 minutes total:

| # | Title | Duration |
|---|-------|----------|
| 1 | Inspecting Node OS Details | 5 min |
| 2 | Key RHCOS 10 Changes | 5 min |
| 3 | Day-2 OS Update Flow | 3 min |
| 4 | Immutability and Compliance | 2 min |

## Detailed Steps

### Section 3.1 — Inspecting Node OS Details

1. List all nodes in the cluster: `oc get nodes -o wide`. Note the OS image field.
2. Select one worker node for inspection (the lab guide specifies which node to use).
3. Open a debug shell on the node: `oc debug node/<node-name>`.
4. Inside the debug shell, run `chroot /host`.
5. Inspect the OS version: `rpm-ostree status` or the bootc equivalent as specified in the lab guide for RHCOS 10.
6. Review the output — identify the image reference, the version, and any pending updates.
7. Exit the debug shell.
8. Cross-reference the node OS image with what was listed in `oc get nodes -o wide`.

### Section 3.2 — Key RHCOS 10 Changes

1. Read the lab guide section summarizing the shift from rpm-ostree to the bootc image-based lifecycle model.
2. Note the key behavioral change: the node OS is now managed as a container image layer, not as individual RPM transactions.
3. Identify what bootc provides: atomic image pulls, rollback to a prior image, and a clear audit trail of what is running on each node.
4. Compare to RHCOS 9 behavior as described in the lab guide — note specifically what is no longer present (e.g., direct rpm-ostree layer customization).

### Section 3.3 — Day-2 OS Update Flow

1. Review the lab guide description of how a day-2 OS update is triggered in OCP 5 (via MachineConfig or the bootc image update mechanism, as specified in the lab guide).
2. Observe a simulated or narrated update event on the lab cluster as directed — a pre-staged update is demonstrated, not a full in-lab upgrade, given the 15-minute module window.
3. Confirm that the update process does not require full node drain-and-reprovision — note the difference from prior RHCOS versions.
4. Inspect the node status before and after the simulated update to see the image reference change.

### Section 3.4 — Immutability and Compliance

1. Read the lab guide note on RHCOS 10 immutability guarantees: the root filesystem is read-only; writable overlays are scoped and auditable.
2. Consider the compliance implications as described in the lab guide — immutability reduces the attack surface and simplifies auditability for standards such as CIS and STIG.
3. Note any compliance-relevant fields visible in the node debug output from Section 3.1 (e.g., image digest, signing metadata if present).

## Key Takeaways

- RHCOS 10 replaces rpm-ostree with a bootc image-based lifecycle — the node OS is managed as a versioned container image, making updates atomic and rollback straightforward
- Day-2 OS updates in OCP 5 do not require full node reprovisioning; the new image is staged and applied at the next boot cycle
- RHCOS 10's read-only root filesystem provides strong immutability guarantees that simplify compliance posture for CIS, STIG, and similar standards
- `oc debug node` remains the primary mechanism for live node inspection in OCP 5, and the output now reflects bootc image references rather than rpm-ostree layer history

## Infrastructure Notes

- The lab cluster must be running OCP 5 with RHCOS 10 nodes (worker and control plane) at lab start — this is the standard post-install state for the lab environment
- Section 3.3 uses a pre-staged or narrated update demonstration rather than a live cluster upgrade — the lab environment does not need to support a full node update within the module time window
- No additional tooling beyond `oc` is required for this module
