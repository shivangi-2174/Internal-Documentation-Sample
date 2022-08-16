Health Computation Using Spark
==============================

Health state and health score are two metrics that are computed exclusively in intelligeni. We call them derived metrics in intelligeni. Health metrics in a way relates more to the quality of service for the whole system. This stage involves computation on graph using pregel algorithm. Below are the steps that happen inside the spark app:

- **Receive messages from alerts_final topic**
	This is not true streaming, rather its micro batch streaming. The messages are pulled at an interval of 5mins from alerts_final topic. 

- **compute health score**
	Health score computation can further be broken into below steps:

	- read old health scores for all CI from elasticsearch derived-metrics index
	- get the topology from neo4j db and construct the whole graph in graphx 
	- compute the health score for all CIs, for which there were alerts in the 5min window, individually
	- pass the graph, new scores to the pregel algorithm to compute the scores for all CIs in the topology. The pregel algorithm then computes the scores ,using the relationship and corresponding formula , for all connected CIs.
	- publish the score in the form of health metrics to derived metrics topic

- **compute health state**
	Health state are the qualitative representation of the CIs health. This is computed using the QoS tags assigned to the alerts in the scenario-creation stage. We have a mapping between health states and the QoS tag/values. Follwing steps are followed in the state computation:
	
	- Aggregation of QoS tags for each CI, that has alert in that 5 min window
	- using state map, compute the state from tags data
	- keep a track of CI entering CI by caching the info into redis
	- create a state-change alert and publish to alerts_input topic when CI enters to the state for the first time
	- using cache decide the state exit case for the CI in each window
	- publish the state metrics data to derived metrics topic 

