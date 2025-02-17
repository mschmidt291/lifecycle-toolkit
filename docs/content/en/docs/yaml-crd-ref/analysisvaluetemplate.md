---
title: AnalysisValueTemplate
description: Define the data source and query for each SLI
weight: 8
---

An `AnalysisValueTemplate` resource
defines a Service Level Indicator (SLI),
which identifies the data to be analyzed
by data source to use and the query to issue.
One Analysis can use data from multiple instances
of multiple types of data provider.

## Synopsis

```yaml
apiVersion: metrics.keptn.sh/v1alpha3
kind: AnalysisValueTemplate
metadata:
  name: response-time-p95
  namespace: keptn-lifecycle-toolkit-system
spec:
  provider:
    name: prometheus | dynatrace | dql | datadog
  query: <query>
```

## Fields

* **apiVersion** -- API version being used
* **kind** -- Resource type.
  Must be set to `AnalysisValueTemplate`
* **metadata**
  * **labels** -- The Analysis feature uses the
    `name` and `part-of` labels that are discussed in
    [Basic annotations](../implementing/integrate/#basic-annotations)
    plus the following:
    * **app.kubernetes.io/instance** analysis-sample
    * **app.kuberentes.io/managed-by** -- Tool used to manage
      the operation of the application.
      Valid values are `helm` and `kustomize`.
    * **app.kubernetes.io/created-by** metrics-operator

      TODO: Need to clarify how to use these annotations
  * **name** -- Unique name of this template.
    Names must comply with the
    [Kubernetes Object Names and IDs](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-subdomain-names)
    specification.
  * **namespace** -- Namespace where this template lives
* **spec**
  * **provider**
    * **name** -- The `spec.name` value of the
      [KeptnMetricsProvider](metricsprovider.md) resource to use.
      Note that each `AnalysisValueTemplate` resource
      can use only one data source.
      However, an `Analysis` resource
      can use multiple `AnalysisValueTemplate` resources,
      each of which uses a different data source.
  * **query** -- query to be made.
    This is done in the data provider's query language.
    It can include variables that use the go templating syntax
    to insert a placeholder in the query.
    For example, the query might include `{{.nodename}}'}`;
    the value to substitute for that variable for this Analysis
    is defined in the `spec.args` section of the `AnalysisTemplate` resource,
    which might be set to `nodename: test`.

## Usage

You must define a
[KeptnMetricsProvider](metricsprovider.md)
for each instance of each data provider you are using.
The `AnalysisValueTemplate` refers to that provider and queries it.

One `Analysis` can use data from multiple instances
of multiple types of data provider;
you must define a
[KeptnMetricsProvider](metricsprovider.md)
resource for each instance of each data provider you are using.
The template refers to that provider and queries it.

## Example

{{< embed path="/metrics-operator/config/samples/metrics_v1alpha3_analysisvaluetemplate.yaml" >}}

For a full example of how the `AnalysisValueTemplate` is used
to implement the Keptn Analysis feature, see the
[Analysis](../implementing/slo)
guide page.

## Files

API reference:
[AnalysisValueTemplate](../crd-ref/metrics/v1alpha3/#analysisvaluetemplate)

## Differences between versions

Keptn v0.8.3 includes a preliminary release of the Keptn Analysis feature
but is hidden behind a feature flag.
To preview these features, set the environment `ENABLE_ANALYSIS` to `true`
in the `metrics-operator` deployment.

## See also

* [Analysis](analysis.md)
* [AnalysisDefinition](analysisdefinition.md)
* [Analysis](../implementing/slo) guide
