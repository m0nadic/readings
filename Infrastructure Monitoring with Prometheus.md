# Infrastructure Monitoring with Prometheus

## Intro

### Organizational Context

|                         | Data Resolution | Data latency | Data Diversity |
| ----------------------- | --------------- | ------------ | -------------- |
| Infrastructure Alerting | High            | Low          | High           |
| Software release view   | High            | High         | High           |
| Capacity planning       | Low             | High         | High           |
| Product / business view | Low             | High         | Low            | 
  
  
### Monitoring Components

- Metrics
  - System resources, application events, business characeristics as specific point in time value
  - Information is obtained in aggregated form
- Logging
  - much more than metrics
  - manifests itself as an evend from an application or system
  - contains all information produces with that event
  - not aggregated and has full context 
- Tracing
  - 
- Alerting
- Visualization

