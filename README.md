# Introduction

Validating Admission Policy (VAP) is a new feature in Kubernetes 1.30 and is available in OpenShift starting with 4.17. This
feature enables operators to enforce policies using CEL and an admission controller that is built into the
Kubernetes API server. This avoids some of the challenges of traditional policy engines that rely on
admission webhooks which can be brittle and cause cluster reliability issues if not properly tuned.

The intent of this repository is to provide a library of policies that are useful in an OpenShift environment.

# VAP Limitations

VAP is not a complete framework in of itself, it lacks some capabilities that can make operationalizing it
somewhat challenging. For example, it provides no mechanism for testing a new or changed policy beyond placing
it a `Warn` mode and manually testing resources. It only provides logging to the audit log which is not
particularly useful from a reporting perspective.

Hopefully over time OSS projects will fill in some of these gaps.

# Enabling VAP in OpenShift

In OpenShift 4.17 VAP is included out of the box. In OpenShift 4.16 and potentially earlier (only tested in 4.16)
it needs to be enabled with a FeatureGate.

*NOTE*: Adding a FeatureGate makes your cluster permanently non-upgradeable, *never* do this on a real cluster that is use. This
should only be done on ephemeral lab type environment.

To enable the FeatureGate, add the following to the existing FeatureGate `cluster` CR:

```
apiVersion: config.openshift.io/v1
kind: FeatureGate
metadata:
  name: cluster
spec:
  customNoUpgrade:
    enabled:
    - ValidatingAdmissionPolicy
  featureSet: CustomNoUpgrade
```
