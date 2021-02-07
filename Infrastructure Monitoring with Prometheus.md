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
- Alerting
- Visualization

### Whitebox versus blackbox monitoring

#### Blackbox Monitoring
- The application or host is observed from the outside.
- This approach can be fairly limited.
- Checks are made to assess whether the system under observation responds to probes in a known way.

##### Example probes
- Does the host respond to  (ICMP) ping?
- Is a given TCP port open?
- Does the application respond with the correct data and status code when it receives a specific HTTP request?
- Is the process for a specific application running in its host?



#### Whitebox Monitoring
- The system under observation exposes data about its internal state and the performance of critical sections. 
- Powerful form of introspection as it exposes the operating telemetry, and health of internal components

##### Telemetry data exposure
- **Exported through logging:** most common case how applications exposed their inner workings before instrumentation libraries were widespread. 
  - E.g: an HTTP server's access log can be processed to monitor request rates, latencies, and error percentages.
- **Emitted as structured events:** similar to logging but instead of being written to disk, the data is sent directly to processing systems for analysis and aggregation.
- **Maintained in memory as aggregates:** Data in this format can be hosted in an endpoint or read directly from command-line tools. **e.g.**:
  - `/metrics` with Prometheus metrics, 
  - HAProxy's stats page
  -  the `varnishstats` command-line tool.

### Metric Collection

#### Approaches
- Push Based
  - Metrics are sent either directly from the `producing application` or from a `local agent` to the `collecting service`
  - Frequency of metric/event generation is too high.
  - **Examples** :
    - Riemann
    - StatsD
    - Elasticsearch, Logstash and the Kibana (ELK) stack
    - Graphite, OpenTSDB
    - Telegraf, InfluxDB, Chronograph, and Kapacitor (TICK)
    - Nagios Service Check Acceptor (NSCA)

- Pull Based
  - collect metrics directly from applications or from proxy processes that make those metrics available to the system. 
  - **Examples**:
    - Nagios 
    - Nagios-style systems `(Icinga, Zabbix, Zenoss, and Sensu)`. 
    - Prometheus.

#### Metrics to observe
Methods for reducing noise and improving visibility on performance and general reliability concerns.

##### Google's four golden signals

- **Latency**: The time required to serve a request
- **Traffic**: The number of requests being made
- **Errors**: The rate of failing requests
- **Saturation**: The amount of work not being processed, which is usually queued

##### Brendan Gregg's USE method
machine focussed
- **Utilization**: Measured as the percentage of the resource that was busy
- **Saturation**: The amount of work the resource was not able to process, which is usually queued
- **Errors**: Amount of errors that occurred 

##### Tom Wilkie's RED method
focussed on service level approach, experience of external clients, 

- **Rate**: Translated as requests per second
- **Errors**: The amount of failing requests per second
- **Duration**: The time taken by those requests

