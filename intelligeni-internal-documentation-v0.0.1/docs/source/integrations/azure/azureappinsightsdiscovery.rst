Azure App Insights Resource Discovery
=====================================

What the API does
------------------
	| For a given Azure Application Insights resource, the API Endpoint dicovers the resources with which the corresponding azure application service interacts with, and sends the relationship data to the stream ``AzureResourceRelationships`` of Graylog.
	| The UI calls the API via the core. 

API Endpoint
------------
	| ``/api/azure/azureappinsights``

Payload
---------
	- ``app_id``: *Application ID* of the Azure Application Insights resource.
	- ``api_key``: *API Key* of the Azure Application Insights with Read telemetry access.
	- ``resource_id``: *Azure Resource ID* of the Azure Application Insights Resource.
	- ``zone_id``: *Zone ID*

Response
---------
	- ``message``: ``Request successfully received``

Interaction with Graylog
-------------------------
	| The API sends the relationship data to the ``AzureResourceRelationships`` stream of Graylog. The stream has the following rule:
 	| ``data_type = "azure_resource_relationship"``

	| The following message is sent to Graylog:

	- ``data_type``: ``azure_resource_relationship`` (Fixed value for the graylog stream ``AzureResourceRelationships``)
	- ``source_id``: Complete Azure Resource ID of the Application Insights Resource which is monitoring the Application Service.
	- ``dest_id`` : Fully Qualified Domain Name of the resource with which the Application Service interacts.
	- ``relationship_type``: ``CONNECTS_TO`` (Fixed for now. Can be changed later for more specific relationships).
	- ``message``: ``azure_app_insights_discovery`` (Fixed. Required field for Graylog).
	- ``zone_id``: Zone ID picked from the payload.
