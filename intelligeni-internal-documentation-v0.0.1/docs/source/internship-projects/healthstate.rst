Health State Computation Model
================================================

**************
Sample Knowledge Graph
**************

.. figure:: Picture1.JPG
    :alt: Process
    :figclass: align-center

Heatmap
########

**Purpose**

The observability feature is introduced with an aim to help the user to get a visibility into the monitoring state of their environment and helping them to improve it. Currently, only metrics are supported, but the plan is to bring in logs and traces to provide complete observability.


**Terminologies**

- **Metric**: A metric is a value that is measured by the system.
- **IG metrics**: Metrics under CI types, that are considered to be important by Intelligeni
- **Observable metrics**: Metrics that are available under a CI
- **Actively monitored metrics**: Metrics that are actively monitored (Some alert rule or anomaly detection is set on them)
- **QOS impact type**: The Quality of Service tag that is assigned to a metric.

**Intelligeni Metrics**

- These are metrics that are considered to be important by Intelligeni
- These metrics would be under an Intelligeni CI type (host, application, etc.)
- These would be pre-classified into QoS types, so additional lookup at metric discovery for the same is not needed.
- These would be the metrics for which Intelligeni would attempt to compute observability score i.e. important metrics
- Any other metric not mapped to these would be classified as a generic metric and not used for observability calculation, this is so that in Intelligeni’s opinion there will be a few set of metrics that it would want the customer to monitor. Anything extra is always welcome but not scored.

**Heatmap computation process**

- Master metric mapping (For integrations supported by us) and IG metric list will be maintained by us.
- A user will only be able to map metrics that have not been mapped by us. This is because all the mappings from Intelligeni are Intelligeni opinions. Any changes to our mapping can be obtained as feedback. (Upcoming feature)
- Metrics discovery occurs via edge. When it comes to the core, before indexing in cimetrics, a lookup will be done from the metric mapping (that will consist of both IG mapping and user-defined mapping) and corresponding IG metric would be added in the document, by default it will be genericMetric.
- Observability will fetch the IG metrics list for a citype and compare from cimetrics as to what metrics are available, not available, and actively monitored and compute the observability score.

**Flowcharts**



.. figure:: end-to-end-flow.JPG
    :alt: Process
    :figclass: align-center

    Overall process behind observability computation

.. figure:: heatmap-compute.JPG
    :alt: alternate text
    :figclass: align-center

    Observability heatmap computation

