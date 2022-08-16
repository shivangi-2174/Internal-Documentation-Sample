Introduction
======================================

Intelligeni core is packaged as a helm repository, and is available for download via helm cli and tools.

The components this chart include:
  *Dependencies*

  - Mongo `mongo chart <https://github.com/bitnami/charts/tree/master/bitnami/mongodb>`_
  - Elasticsearch `elastic chart <https://github.com/bitnami/charts/tree/master/bitnami/elasticsearch>`_
  - Neo4j `neo4j chart <https://github.com/equinor/helm-charts/tree/master/charts/neo4j-community>`_
  - Redis `redis chart <https://github.com/bitnami/charts/tree/master/bitnami/redis>`_
  - Keycloak `keycloak chart <https://github.com/codecentric/helm-charts/tree/master/charts/keycloak>`_
  - Mysql `mysql chart <https://github.com/bitnami/charts/tree/master/bitnami/mysql>`_
  - Spark `spark operator chart <https://github.com/GoogleCloudPlatform/spark-on-k8s-operator/tree/master/charts/spark-operator-chart>`_
  - Ingress Controller `ingress controller chart <https://github.com/kubernetes/ingress-nginx/tree/master/charts/ingress-nginx>`_

  *Templates*

  - Chatops Handler App
  - Core API Layer
  - Event Discovery App
  - Machine Learning App
  - Scenario Creation App
  - Scenario Metadata App
  - Topology Discovery App
  - Discovery Nodes App
  - Spark Jobs
  - Ingress Definitions
  - Cronjobs for various batch processing

The Helm version required is Helm 3+, chart api version 2.

The helm repository is hosted at `Harbor <https://harbor.intelligeni.com/chartrepo/intelligeni>`_.
