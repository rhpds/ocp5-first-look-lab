# Module 4 — Security, Sovereign & Disconnected

## Brief Overview

OpenShift 5 extends its security and disconnected-operations story in three areas: oc mirror v2's delta mirroring capability for efficient Operator catalog synchronization, post-quantum cryptography (PQC) cipher suite support on the API server and ingress controller, and Red Hat Lightwell (reserved placeholder). This module gives participants hands-on experience with delta mirroring and PQC configuration, which together address two of the most common field concerns — keeping disconnected environments current and hardening TLS for post-quantum threats. The Lightwell section is a placeholder pending content confirmation.

## Audience and Time

- **Target personas:** Solutions Architects, Technical Sales, Field Engineers
- **Prerequisites for this module:** Completion of the orientation section; familiarity with OCP TLS configuration concepts and Operator catalogs; no prior oc mirror v2 or PQC experience required
- **Estimated duration:** 20 minutes (confirmed sections 4.1–4.2 only; 4.3 is a placeholder and will add time when filled in)

## Learning Objectives

- Use oc mirror v2 delta mirroring to selectively synchronize Operator catalog content by channel and architecture
- Apply a new CatalogSource to the cluster using mirrored content
- Enable post-quantum cryptography cipher suites on the OCP 5 API server and ingress controller
- Inspect certificates to confirm PQC cipher suites are active
- Describe key rotation behavior under PQC configuration

## Lab Structure

Two confirmed sections plus one reserved slot, approximately 20 minutes for confirmed content:

| # | Title | Duration |
|---|-------|----------|
| 1 | oc mirror v2 Improvements | 10 min |
| 2 | Post-Quantum Cryptography | 10 min |
| 3 | Lightwell (placeholder) | TBD |

## Detailed Steps

### Section 4.1 — oc mirror v2 Improvements

1. Review the lab guide introduction to oc mirror v2 delta mirroring — understand how it differs from a full mirror by only transferring changed layers since the last mirror run.
2. Inspect the pre-staged mirror state on the internal registry (mirror metadata file location is specified in the lab guide).
3. Construct a mirror configuration file (ImageSetConfiguration) that filters by a specific channel and architecture — the lab guide provides a template; complete the channel and architecture fields as directed.
4. Run oc mirror v2 in delta mode: `oc mirror --config=<imageset-config.yaml> docker://<internal-registry> --dry-run` to preview what would be transferred.
5. Review the dry-run output — confirm that only delta content (new layers since the last mirror) is listed, not a full catalog.
6. Run the actual mirror command (without `--dry-run`) as directed by the lab guide.
7. Once the mirror run completes, apply the generated CatalogSource YAML to the cluster: `oc apply -f catalogsource.yaml`.
8. Verify the CatalogSource is healthy: `oc get catalogsource -n openshift-marketplace` and confirm the new source appears and reaches a READY state.
9. Optionally, browse the Operator Hub in the console to confirm the mirrored catalog content is visible.

### Section 4.2 — Post-Quantum Cryptography

1. Read the lab guide overview of post-quantum cryptography and the specific cipher suites supported in OCP 5 (e.g., KYBER or ML-KEM variants, as documented in the lab guide).
2. Open the APIServer cluster resource for editing: `oc edit apiserver cluster`.
3. Add the PQC cipher suite configuration to the TLS security profile as directed by the lab guide (the exact field and value are provided).
4. Save the change and monitor the API server rollout: `oc get co kube-apiserver -w`.
5. Wait for the API server operator to report Available=True, Progressing=False, Degraded=False.
6. Repeat the cipher suite configuration step for the IngressController: `oc edit ingresscontroller default -n openshift-ingress-operator` (using the field and value specified in the lab guide).
7. Monitor the ingress controller rollout: `oc get co ingress -w`.
8. Inspect the active cipher suites on the API server endpoint using `openssl s_client` or the equivalent command provided in the lab guide.
9. Confirm a PQC cipher suite appears in the negotiated cipher output.
10. Review the lab guide note on key rotation behavior — understand how certificate rotation interacts with PQC cipher configuration in OCP 5.

### Section 4.3 — Lightwell *(placeholder)*

> **PLACEHOLDER — Content pending confirmation with engineering.**
> This section will cover Red Hat Lightwell as it relates to OCP 5 security and sovereign cloud capabilities. Steps, duration, and infrastructure requirements are TBD. Do not include this section in the published lab until content is confirmed.

## Key Takeaways

- oc mirror v2 delta mirroring significantly reduces the bandwidth and time required to keep disconnected Operator catalogs current by transferring only changed content
- Filtering by channel and architecture in the ImageSetConfiguration further reduces mirror size and complexity, making catalog management practical in constrained environments
- OCP 5 supports post-quantum cryptography cipher suites on both the API server and ingress controller, providing a path to TLS hardening ahead of standardized PQC adoption
- Certificate rotation in OCP 5 is compatible with PQC cipher configuration — key rotation behavior is automatic and does not require manual intervention
- Section 4.3 (Lightwell) is a reserved slot — content will be added when confirmed with engineering

## Infrastructure Notes

- Section 4.1 requires a pre-staged internal registry with an initial mirror state (metadata file) so that delta mirroring behavior can be demonstrated — this is provisioned by lab automation
- Section 4.2 requires the lab cluster to be running OCP 5 with a version that supports PQC cipher suite configuration (confirm minimum version with engineering before publishing)
- The `openssl` or equivalent inspection tool must be available in the lab terminal environment for Section 4.2 step 8
- Section 4.3 infrastructure requirements are TBD pending content confirmation
