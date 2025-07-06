# Design Infra Monitoring 
![image](https://github.com/user-attachments/assets/9f8cc650-ad9c-450d-a943-13bdf169c37d)




|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Anitha Annem  | Jul 06  | v1.0|  Jul 06    | Pre-Reviewer   | Priyanshu            |
| Anitha Annem  |  |  |   | L0             | Khushi Malhothra    |
| Anitha Annem  |     |      |         | L1             | Mukul Joshi       |
| Anitha Annem  |     |      |         | L2             | piyush Upadhyay      |

# Table of Contents

- [Design Infra Monitoring](#design-infra-monitoring)
- [Introduction](#introduction)
- [Objectives](#objectives)
- [Key Performance Metrics](#key-performance-metrics)
- [Requirements for Infrastructure Monitoring](#requirements-for-infrastructure-monitoring)
- [Monitoring Tools](#monitoring-tools)
- [Tool Comparison Table: Infrastructure Monitoring](#tool-comparison-table-infrastructure-monitoring)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)


# Introduction
In this guide, we will discuss what metrics, monitoring, and requirements . We will talk about why they are important, what types of opportunities they provide, and the type of data you may wish to track. We will be introducing some key terminology along the way and will end with a short glossary of some other terms you might come across while exploring this space.
Information about the health and performance of your deployments not only helps your team react to issues, it also gives them the security to make changes with confidence. One of the best ways to gain this insight is with a robust monitoring system that gathers metrics, visualizes data, and alerts operators when things appear to be broken.


# Objectives
List the primary objectives of the infrastructure monitoring initiative. Examples:
- To clearly define critical infrastructure performance metrics.
- To set standard thresholds for these metrics.
- To specify requirements for continuous monitoring and alerting.
- To support proactive maintenance and rapid incident response.


# Key Performance Metrics

| **Metric**          | **Description**                                   | **Warning Threshold** | **Critical Threshold** |
|----------------------|--------------------------------------------------|----------------------|------------------------|
| **CPU Utilization** | % of CPU capacity used                         | > 70%               | > 90%                 |
| **Memory Usage**    | % of RAM used                                   | > 75%               | > 90%                 |
| **Disk Usage**      | % of disk space used                           | > 80%               | > 90%                 |
| **Disk I/O Wait** | Time spent waiting for I/O                    | > 10%              | > 20%                |
| **Network Usage** | Bandwidth utilization & errors              | > 70%              | > 90% or >1% packet loss |
| **Service Health** | Availability of critical services           | Partial downtime | Full service stop    |


# Requirements for Infrastructure Monitoring

| Requirement              | Description                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------|
| Real-time Monitoring     | Ability to monitor infrastructure components in real-time.                                        |
| Historical Data Analysis | Capability to store and analyze historical data for trend analysis and capacity planning.          |
| Customizable Dashboards  | Flexibility to create and customize dashboards to visualize key metrics.                           |
| Scalability              | Support for scaling monitoring infrastructure as the organization grows.                           |
| Integrations             | Compatibility with existing tools and systems for seamless integration.                            |
| Automation               | Support for automated monitoring setup, alerts, and responses.                                     |
| Multi-Platform Support   | Monitoring support for various platforms (e.g., cloud, on-premises, hybrid environments).          |

# Monitoring Tools

| Tool                | Description                                                                                                  | Role in Monitoring                                                                                             |
|---------------------|--------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **Prometheus + Grafana** | Prometheus is an open-source time-series database and monitoring system; Grafana is a visualization platform for dashboards. | Prometheus collects and stores metrics from infrastructure and applications; Grafana visualizes these metrics and provides real-time dashboards and alerting. |
| **Datadog**         | Cloud-based SaaS platform for monitoring infrastructure, applications, logs, and more.                       | Provides unified infrastructure and application monitoring, integrates logs and traces, supports AI-based anomaly detection, and enables fast issue resolution. |
| **New Relic**       | SaaS-based observability platform for application performance, infrastructure monitoring, and tracing.       | Offers deep application performance monitoring (APM), infrastructure health tracking, and end-to-end service visibility for identifying bottlenecks and improving performance. |
| **Nagios**          | Open-source (also available as enterprise) monitoring tool for systems and networks.                         | Performs active checks on services and hosts, sends threshold-based alerts, and ensures availability of critical systems, especially in traditional IT environments. |

# Tool Comparison Table: Infrastructure Monitoring

| **Tool**               | **Type**         | **Strengths**                                         | **Weaknesses**                       | **Best Use Cases**                         |
|-------------------------|------------------|-------------------------------------------------------|-------------------------------------|-------------------------------------------|
| **Datadog**          | SaaS (Full-stack) | Easy to set up, strong integrations, AI-powered alerts, excellent dashboards | High cost at scale              | Multi-cloud, hybrid, dynamic infra      |
| **New Relic**        | SaaS (Full-stack) | Deep APM capabilities, strong user experience monitoring, quick onboarding | Complex pricing, focus more on app-level | App-heavy, microservices, SaaS environments |
| **Grafana + Prometheus** | Open Source   | Highly customizable dashboards, strong cloud-native support, no license cost | Requires manual setup, ongoing maintenance | Kubernetes, container-based, cloud-native infra |
| **Nagios**          | Open Source     | Strong for basic infra/service checks, highly extensible plugins, fully self-managed | Older UI, lacks modern cloud integrations | Traditional on-prem servers, smaller static workloads |


# Conclusion

While all tools effectively support monitoring core metrics (CPU, memory, disk, network), **Grafana with Prometheus** is chosen for its flexibility, strong community support, cost-effectiveness, and superior support for modern cloud-native and container environments. It provides full data ownership and powerful, customizable dashboards tailored to operational and business needs.


# Contact Information 
| Name       | Email Address                |
|------------|------------------------------|
| Anitha     |anitha.annem.snaatak@mygurukulam.co|

# References
| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [Grafana Documentation](https://grafana.com/docs/)| Documentation on Grafana     |
| [Prometheus Documentation](https://prometheus.io/docs/introduction/overview/) | Documentation on Prometheus |
| [Flow](https://medium.com/@guptagoutam2021/how-to-design-a-metrics-monitoring-and-alerting-system-87c02e990dd1) | For Flows |

