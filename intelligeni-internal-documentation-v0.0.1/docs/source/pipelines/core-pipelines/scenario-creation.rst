Scenario Creation
====================

Scenario creation is the 1st stage of the core pipeline. Following steps are performed in this stage:

- *receiving alert from alerts_input topic*
	In a stream process, 1st step is reading from the stream. In the same we do first read the message from the alerts_input stream and decode the message to standard json format. Decoding is needed since in azure eventhub messages are stored in the encoded format. Once the message is available in json readable format, we pass it on to the next step.
- *check if the alert message has valid or required fields*
	We check for the validity or correction of the message by looking for presence of required fields in the alert json
- *tagging alert with various metadata*
	Once we receive the alert in the standard json format, we look to enrich the alert further by tagging it with the properties of the CI the alert belongs to. We make a igql call to get the properties of the CI and augment the alert json with those values. We further enrich the alert by assigning QoS tags to it. We fetch the QoS tags for the alert by calling ML service endpoint with the alert title as the main input. Received tags are assigned to the alert and then sent for further processing.

- *aggregating alerts to create scenarios*
	This is the most important step of this stage and here we decide whether the alert will create a new scenario or will get merged to one or more existing scenarios. This step also performed in a flow as shown below:

	.. figure:: scenario-flow.png
		:alt: Scenario flow diagram

- *publish messages to scenario chat window*
	Once the scenario is created or an alert added to a scenario, we need to pass on this message to the chatops window dedicated for the scenario. Incase when a new scenario is created, this step also created the chatops window for the scenario. To create or publish message to the chatops we call the core API with the necessary details and text to be shown in the chat window. Two kind of messages are published and they are as follows.
	
		- scenario created
		- alert added

- *publish enriched alert to alerts_final topic*
	Once we have decided the scenario for the alert, its time to stamp that to the alert and then publish the updated alert to the alert_final stream/topic for further processing by a diffrent stage.
	
- *publish created scenario to scenario_master topic*
	We also publish the scenario json to a upstream topic called scenario_master for further processing in other stages of pipeline.

