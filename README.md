# kyverno-kuttl
Sample test written through Kyverno's fork of Kuttl for a Sample Mutating Policy

## Description

This test is designed to test the working of the policy mentioned below.
```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: lfx-add-labels
spec:
  rules:
  - name: lfx-mentorship
    match:
      any:
      - resources:
          kinds:
          - ConfigMap
          namespaces:
            - "team-*"
    mutate:
      patchStrategicMerge:
        metadata:
          labels:
            +(lfx-mentorship): kyverno
```

## Expected Behavior

The tests will create a configmap, and a policy, then apply the policy. A ConfigMap resource `resource-mutated.yaml` is expected, and another one `resource-fail.yaml` is not expected.

## Testcases

* To check if the mutation is taking place if the label already exists
    * Expected Result: It should skip the mutation
* To check if the mutation is taking place if the label does not exist
    * Expected Result: It should create another ConfigMap(StrategicMergePatch) with the mentioned label
* To check if the mutation is taking place in a namespace other than `team-*`
    * Expected Result: It should not find any matching resource to apply the mutation

## GitHub Action

Wrote an action based from the Kyverno's existing action for conformance tests with a few modifications. It prepares the image from the kyverno repository, and checks with the test given here.
