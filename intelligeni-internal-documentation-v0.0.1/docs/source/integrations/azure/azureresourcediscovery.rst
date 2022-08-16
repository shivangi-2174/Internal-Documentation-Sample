Azure Resource Discovery
==========================

What the API does
------------------
	| For a given Azure subcription, the API Endpoint discovers the resources in Azure and sends their data to the stream ``AzureResourceDiscovery`` of Graylog. It also loops recursively through the resources to collect connected resources and sends the topology data to the stream ``AzureResourceRelationships`` of Graylog.
	| The UI calls the API via the core.

API Endpoint
------------
	| ``/api/azure/azureResources``

Payload
---------
	- ``tenantId``: *Tenant ID* of the Azure subscription.
	- ``subscription_id``: *Subscription ID* of the Azure subscription.
	- ``clientSecret``: *Cleint Secret* of the application
	- ``clientId``: *Client ID* of the application
	- ``zone_id``: *Zone ID* 

Response
---------
	- ``message``: ``Request successfully received``

Interaction with Graylog
-------------------------

	| The API sends the resource discovery data to ``AzureResourceDiscovery`` stream of Graylog. The stream has the following rule:
	| ``data_type = "azure_resource_discovery"``
	| The properties of the resource are sent as a key-value pair along with the following properties to Graylog:

	- ``data_type``: ``azure_resource_discovery`` (Fixed value for the graylog stream ``AzureResourceDiscovery``)
	- ``message``: ``azure_resource_discovery`` (Fixed. Required field for Graylog).
	- ``zone_id``: Zone ID picked from the payload.	
	- ``subscription_id``: Subscription ID picked from the payload.

	| The API sends the relationship data to the ``AzureResourceRelationships`` stream of Graylog. The stream has the following rule:
 	| ``data_type = "azure_resource_relationship"``
	| The following message is sent to Graylog:

	- ``data_type``: ``azure_resource_relationship`` (Fixed value for the graylog stream ``AzureResourceRelationships``
	- ``source_id``: Complete Azure Resource ID of the Application Insights Resource which is monitoring the Application Service.
	- ``dest_id`` : Fully Qualified Domain Name of the resource with which the Application Service interacts.
	- ``relationship_type``: ``CONNECTS_TO`` (Fixed for now. Can be changed later for more specific relationships).
	- ``message``: ``azure_app_insights_discovery`` (Fixed. Required field for Graylog).
	- ``zone_id``: Zone ID picked from the payload.
