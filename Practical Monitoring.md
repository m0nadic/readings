# Practical Monitoring

## Antipatterns

- Tool obsession does not give you better monitoring.
- Monitoring is everyones job, not a single role in the team.
- Great monitoring is more than `Check-Box Monitoring`
- Monitoring doesn't fix broken things
- Lack of automation is an indicator that something important has been missed.

## Components of a monitoring service

- Data collection
- Data storage
- Visualization
- Analytics and Reporting
- Alerting

## Desing Patterns

- composable monitoring is more useful than monoliths (e.g: Nagios)
- monitoring from user's perspective first yields more effective visibility.
- prefer buying monitoring tools (Saas solutions) over building your own monitoring tools.
- improve continuously

## Data collection models
- Push based model (syslog, collectd) - easier to scale in a distributed architecture like cloud environemnts
- Pull based model (SNMP, nagios, health endpoints with consul or etcd) - difficult to scale due to central poller

## Types of data to be collected
- logs
  - structured (JSON)
  - unstructured (nginx, apache)
- metrics
  - counter - ever increasing metric
  - gauge - point in time value - store in TSDB - plot as graph for visualization


## Metric storage
- Metrices are time series data
- generally stored in TSDB (Round Robin Database, Graphite Whisper, InfluxDB, Prometheus)
- Many TSDBs support roll-up or age-out of data - multiple datapoints are summarised into a single datapoint
  - averaging
  - summing datapoints
  
## Log storage
- flat files (rsyslog, syslog)
- search engines (Elastic Search)

## Log storage is expensive
- Compression
- Retention Policies

## Visualization 
Dashboard products:
- Grafana
- Smashing

## Nyquist–Shannon sampling theorem
> To measure outage of 2 minutes, you must be collecting data in minuite-long intervals. Thus to measure availability down too one second, you must be collecting data in sub-second intervals, That is why reporting SLA better than 99% is so difficult.


---

## Alerting

### Meaning
- Wake Up Alerts
- FYI Alerts

### Practices that are key to building great alerts
- Stop using emails for alerts
- Write **runbooks**.
- Arbitrary static thresholds are not the only way.
- Delete and tune alerts
- Use maintenance periods
- Attempt self healing first.

### Alert Use Cases and Response

- **Action required immediately**
  - Send SMS, Pager Duty `Actual Alert as per definition`
- **Awareness needed, but immediate action not required**
  - Send to chat rooms, channels (or emails - less preferred)
- **Record for historical / diagnostic purpose**
  - Send information to log files.

### Runbooks
- great way to quickly orient yourself when an alert fires.
- greate way to spread knowledge withing the team.
- Runbooks are written for a service.
- Every **alert** should have a link to the **runbook** for that service.

### A good runbook should answer the following questions
- What is this service and what does it do?
- Who is responsible for this service?
- What dependencies does the service have?
- What does the infrastructure for the service look like?
- What **metrics** and **logs** does the service emit and what do they mean?
- What alerts are setup for the servive and why?

### Tuning Alerts

- Do all your alerts require someone to work?
- Look at a month's worth history for your alerts. What are they? what were the actions? What were the impact of each one? Are there alerts that can simply be deleted? What about modifying the thresholds? Could you redesign the underlying check to be more accurate?
- What automation can you build to make the alert obsolete entirely?

## On-Call

### Duties of on-call person
- compile the list every alert that got fired for the previous day.
- go through them and ask yourself, how the alerts signal can be improved or the alert can be deleted entirely.
- Do this everyday you are on-call.

### Strategies for handling legitimate alerts
- Make it the duty of the on-call person to work on system resiliency and stability during their on-call shifts when they are not fighting fires.
- Explicitly plan for system resiliency and stability work during the following weeks sprint planning based on the information collected from the previous on-call week.

### On-call rotation

- Rotation schedule (weekly assignment)
- on-call hand off on a weekday 
  - whats in-flight that needs attention
  - any patterns noticed during the week.

- **FTS rotation** (Follow-the-sun): split rotation across time-zones.
- backup on-call person: not a good idea (burn-out)
- Proper escalation paths
- Put software engineers into on-call shifts so that they empathise the strugles that arise during on-call and try to improve resiliency.
- Augment your on-calls with tools such as PagerDuty, VictorOps, OpsGenie etc.

### On-call compensation
- Give a PTO day immediately following the end of on-call shift.
- Pay your team extra for their on-call shifts.

### Incident management
Formal way of handling issues that arise on production.

- Frameworks - ITIL

#### Simplified process for Incident Management

- Incident Identification `(monitoring identifies a problem)`
- Incident Logging `(monitoring automatically opens a ticket for the incident)`
- Incident diagnosis, categorization, resolution and closure. `(on-call troubleshoots, fixes the problem, resolves the ticket with comments and additional data)`
- Communications throughout the incident as necessary.
- After the incident is resolved, come up with remediation plans for building in more resiliency)

> **Reference:** [PagerDuty's Incident Response documentation](https://response.pagerduty.com/)
> 

## Statistics

### Mean an Average
- useful for determining, what a dataset generally looks like without examining every single entry in the set

### Moving Average
- Rather than taking the entire dataset and calculating the avrage, it calculates the average when a new datapoint arrives.
- smoothes out a spiky graph
- used in TSDBs for storing rolled-up data
- used in time-series graphing tool when viewing a large set of metrics.

### Median
- Essentially the "Middle" of a dataset.
- For datasets that are **highly skewed** in one direction, median can be more representative of the dataset than a mean.
- Sort the dataset in ascending order and then calculate the middle using the formula `(n+1)/2`. For even number of entries it is average of the two middle entries.

### Seasonality
- When datapoints adopt a repeating pattern.
- Not all workloads have seasonality

### Quantiles
- Statistical way of describing a specific point in a dataset.
- 50th quantile -> median
- Percentile - way of describing a point in a dataset in terms of percentage.
- percentiles found in
  - metered bandwidth billing
  - latency reporting
- Percentile calculation
  - Sort dataset in ascending order
  - remove top n percent of the values
  - the next largest number is the Nth percentile
- you can't average perceentiles as you are missing some data.
- Percentiles are helpful for understanding what the bulk of your data looks like, but be careful, they inherently ignore the extreme datapoints.

### Standard Deviation
- measure of how close or far are values from the mean.
- can only be appled for a normally distributed dataset.

---

## Business KPIs

### Common metrics used by business owners

- **Monthly Recurring Revenue**
- **Revenue per customer**
- **Number of paying customers**
- **Net promoter score**
- **Customer lifetime value**
- **Cost per customer**
- **Customer acquisition cost**
- **Customer churn**
- **Active users**
- **Burn Rate**
- **Run Rate**
- **Total Addreseable market**
- **Gross profit margin**

**Reference**
- [16 startup metrics](https://a16z.com/2015/08/21/16-metrics/)
- [16 more startup metrics](https://a16z.com/2015/09/23/16-more-metrics/)




### Yelp KPIs

### Reddit KPIs

### Corelating Business KPIs to Technical Metrics


