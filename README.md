
# Infra Monitoring with Alert Manager

|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Anitha Annem  | Jul 07  | v1.0|  Jul 07    | Pre-Reviewer   | Priyanshu            |
| Anitha Annem  |  |  |   | L0             | Khushi Malhothra    |
| Anitha Annem  |     |      |         | L1             | Mukul Joshi       |
| Anitha Annem  |     |      |         | L2             | piyush Upadhyay      |




# Introduction
This document provides a detailed guide on setting up and using Grafana with alerting rules and the Alert Manager for infrastructure monitoring. It covers alerting rules, configuration, and best practices to ensure timely issue detection and resolution.


# Monitoring Tools
- **Prometheus**: Collects and stores metrics.
- **Grafana**: Visualizes metrics and sets up dashboards.
- **Alert Manager**: Manages alerts, routes them to the appropriate channels, and sends notifications.

# Alerting Rules
Alerting rules are critical for detecting issues in your infrastructure. These rules define the conditions under which an alert should be triggered. 
Alerting rules are written in Prometheus, where you specify conditions based on metrics. When these conditions are met for a specified duration, an alert is triggered and sent to Alert Manager.

## Steps to Create Alerting Rules
1. **Identify Key Metrics**: Determine which metrics are crucial for your infrastructure’s health (e.g., CPU usage, memory usage, disk space, network latency, etc.).
2. **Define Thresholds**: Set thresholds for these metrics that will trigger alerts. For example, disk space usage above 80% might trigger a warning, while above 90% could trigger a critical alert.
3. **Create Alerting Rules in Prometheus**: Write alerting rules using Prometheus’s PromQL. Example:
    ```yaml
    groups:
    - name: CPU-alerts
      rules:
      - alert: HighCPUUsage
        expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage detected on instance {{ $labels.instance }}"
          description: "CPU usage has exceeded 80% for more than 5 minutes."
      - alert: HighMemoryUsage
        expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 80
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High Memory usage detected on instance {{ $labels.instance }}"
          description: "Memory usage has exceeded 80% for more than 5 minutes."
      - alert: DiskSpaceUsage
        expr: node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"} * 100 < 20
        for: 10m
        labels:
          severity: lowd-disk-warning
        annotations:
          summary: "Low disk space on instance {{ $labels.instance }}"
          description: "Disk space is less than 20% available for more than 10 minutes."
      - alert: HighNetworkLatency
        expr: rate(node_network_receive_errs_total[5m]) > 1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High network latency detected on instance {{ $labels.instance }}"
          description: "Network receive errors have exceeded 1 per second for more than 5 minutes."
      - alert: ServiceDown
        expr: up{job="my-service"} == 0
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Service {{ $labels.job }} is down on instance {{ $labels.instance }}"
          description: "The service has been down for more than 2 minutes."
    ```
    This rule triggers if CPU usage exceeds 80% for more than 5 minutes.
4. **Test Alerting Rules**: Test the rules in a staging environment to ensure they trigger correctly.
5. **Deploy Rules**: Once tested, deploy the alerting rules in your production environment.
6. **Document Thresholds**: Clearly document the thresholds and the rationale behind them to ensure they are understood and maintained correctly.

# Steps to Configure Alert Manager
Begin by setting up the basic configuration in the `alertmanager.yml` file. This includes defining routes, receivers, and notification channels. 
    ```yaml
    global:
      resolve_timeout: 5m

    route:
      group_by: ['alertname']
      group_wait: 30s
      group_interval: 5m
      repeat_interval: 3h
      receiver: 'email_notifications'

    receivers:
    - name: 'email_notifications'
      email_configs:
      - to: 'rahul.kumar@opstree.com'
        from: 'alertmanager@opstree.com'
        smarthost: 'smtp.opstree.com:587'
        auth_username: 'alertmanager@opstree.com'
        auth_password: 'password@123'

    inhibit_rules:
    - source_match:
        severity: 'critical'
      target_match:
        severity: 'warning'
      equal: ['alertname', 'instance']
    ```
3. **Routing Alerts**: Set up routing rules to determine which alerts go to which receivers. For example, you can route critical alerts to both email and Slack, while lower severity alerts might only go to email.
4. **Notification Channels**: Configure various notification channels such as email, Slack, or PagerDuty. This ensures alerts reach the appropriate teams quickly.
5. **Grouping and Inhibition**: Group similar alerts to avoid alert fatigue. Use inhibition rules to suppress alerts that are less important when a related, more critical alert is active.
6. **Test Configuration**: Test the entire setup by triggering sample alerts. Verify that the alerts are received correctly and routed to the right channels.

# Step-by-Step Usage
1. **Monitoring**: Alert Manager continuously monitors incoming alerts from Prometheus. It checks the alert labels and routes them according to your configuration.
2. **Alert Deduplication**: When multiple alerts of the same type are triggered, Alert Manager deduplicates them, ensuring you don't receive multiple notifications for the same issue.
3. **Alert Grouping**: Alerts are grouped based on your configuration (e.g., by alert name, instance, or severity). This helps in managing alerts more effectively.
4. **Notification**: Once an alert is grouped and processed, Alert Manager sends notifications via the configured channels.
5. **Silencing Alerts**: If you’re performing maintenance or are aware of an issue that’s already being handled, you can silence alerts temporarily. This prevents unnecessary notifications.
6. **Inhibition**: Alerts can be suppressed based on inhibition rules. For instance, if a critical alert is active, related warning alerts might be inhibited to reduce noise.

## Overview
Grafana is a powerful visualization tool for displaying metrics collected by Prometheus. It can also be used to set up dashboards and alerting rules to monitor infrastructure. Alert Manager manages alerts sent by Prometheus, handling deduplication, grouping, and routing to the correct receivers. This guide walks through the steps needed to configure and effectively use Grafana and Alert Manager in your infrastructure monitoring setup.

### Best Practices for Using Alert Manager

| Best Practice                    | Description                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------|
| **Clear Thresholds**             | Set and document clear thresholds for triggering alerts. This helps prevent alert fatigue and ensures that alerts are meaningful. |
| **Regular Configuration Review** | Regularly review and update Alert Manager's configuration to align with changes in your infrastructure or application. |
| **Document Everything**          | Maintain thorough documentation of your alerting rules, thresholds, and Alert Manager configuration. This is crucial for troubleshooting and onboarding. |
| **Training**                     | Ensure your team is trained in using Alert Manager, understanding alerts, and taking appropriate action. |
| **Testing**                      | Regularly test your alerting and notification setup to ensure everything works as expected.       |

## Conclusion
Alert Manager is a key component in an effective infrastructure monitoring setup. By properly configuring and using it, you can ensure that your team is notified of critical issues in a timely manner, enabling quick response and resolution.

## Contact Information
| Name          | Email                    |
|---------------|--------------------------|
| Rahul Sharma  | rahul.kumar@opstree.com  |

## References

| Reference                               | Description                                      |
|-----------------------------------------|--------------------------------------------------|
| [Prometheus Documentation](https://prometheus.io/docs/) | Official documentation for Prometheus.            |
| [Grafana Documentation](https://grafana.com/docs/)     | Official documentation for Grafana.               |
| [Alert Manager Documentation](https://prometheus.io/docs/alerting/latest/alertmanager/) | Official documentation for Alert Manager.         |
