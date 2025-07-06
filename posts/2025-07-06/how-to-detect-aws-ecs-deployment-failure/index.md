<!--
.. title: how to detect aws ecs deployment failure
.. slug: how-to-detect-aws-ecs-deployment-failure
.. date: 2025-07-06 22:21:46 UTC+02:00
.. tags: aws,ecs,cloudwatch,prometheus
.. category: aws
.. link: 
.. description: 
.. type: text
-->
# Deployment Issue and Alert Configuration

Recently we had an interesting issue.  
Due to an incorrect cloud config entry, one of our services fell into an endless loop of failed deployments.  

So we had to add some kind of alerts to detect this in the future.

We are deploying our services on AWS ECS.  
As we are using [yet-another-cloudwatch-exporter](https://github.com/prometheus-community/yet-another-cloudwatch-exporter) to export CloudWatch metrics into Prometheus, 
the natural way was to expose new metrics and add a Prometheus alert based on it.

We exported one new metric from `ECS/ContainerInsights`:

```yaml
apiVersion: v1alpha1
# [...]
discovery:
  jobs:
    - type: ECS/ContainerInsights
      regions:
        - eu-west-1
      dimensionNameRequirements: [ClusterName, ServiceName]
      statistics: [Average]
      nilToZero: true
      addCloudwatchTimestamp: false
      metrics:
        - name: DeploymentCount
# [...]
```

and added new config for Prometheus alerts to `config/apps-alerts.yml`:

```yaml
---
groups:
  - name: apps-alerts
    rules:
      - alert: DeploymentAlert
        expr: sum by (dimension_ServiceName) (aws_ecs_containerinsights_deployment_count_average) >= 2
        for: 15m
        labels:
          severity: page
        annotations:
          summary: "Probable deployment problem: `{{ $labels.dimension_ServiceName }}`"
```

and finally added new config to Prometheus env-based configs (`prometheus-XXX-config.yml`):

```yaml
# [...]
rule_files:
  # [...]
- apps-alerts.yml
# [...]
```

Now if there are some problems with failed deployments, we are seeing notifications in our slack.