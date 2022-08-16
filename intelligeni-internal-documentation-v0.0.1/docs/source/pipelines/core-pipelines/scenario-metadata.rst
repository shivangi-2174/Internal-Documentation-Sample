Scenario Metadata
========================

Scenario metadata computation is that stage of the pipeline where we compute certain properties of scenario from the alerts that make the scenario in a stream process. As in when alert comes, these specific properties are recomputed and the update is sent to the scenario document. Following are sequence of steps that occur in this stage:

- **receive alert message from alerts_final eventhub topic**
	We subscribe to the alerts final topic, since it contains the enriched alert from the previous stage carrying the scenario context. The step also receives the encoded message from the topic and decodes it into the json format.

- **insert the data to redis** 
    For fast processing, we store the data in redis cache. This improves the performance of the overall processing. We store the metadata in redis keys ,named in the format of '<property_name>:<scenario_uuid>'. For example, if the metadata/property is 'zone_id',and the scenario id is #ra1233ddfrtffrt456446, then the key for it will be 'zone_id:ra1233ddfrtffrt456446'. The values are picked from the alert json received earlier and are then inserted into the redis based on the type of the key. 

- **retrieve data from redis**
	This step involves fetching all the data from the redis for the scenario.

- **compute the metadata**
	This step involves computing the metadata for the scenario based on the retrived data. Different metadata are computed differently thus the data store(redis data type for the key) also differs accordingly.

- **publish the data to scenario_master topic**
	This step takes all the updated values and then publishes the json with all of them and the scenario_id to eventhub.