======================================
Customizing
======================================

Being a helm chart, :code:`intelligeni-core` comes with a highly customizable values.yaml. This finely controls the deployment, scaling and features needed for the cluster.

**Note** - *values.yaml shown above might be old, you may find some new fields and restructuring in the latest chart's values.yaml*.

.. literalinclude:: values.yaml
  :language: yaml
  :linenos:
  :caption: Values file for chart intelligeni-core (values.yaml)
  :emphasize-lines: 32, 51, 65, 78, 91, 105, 119, 123, 135, 151, 165, 166, 170-172, 174-178, 180, 182-183, 185-188, 190-192, 194-198, 209-210, 220, 239, 241-243, 260

The highlighted lines are mandatory to be filled, it provides the non-machine generated initial configuration that the cluster starts with.

Feel free to change any other parameter!

--------------------------------

-------------
Prerequisites
-------------

As you may have notices going through the *values.yaml* file that the chart needs a few things to be set up beforehand. Here is a list -

1. A **storageClass** by the name :code:`intelligeni-core`, or replace storageClass with other existing value.

2. A secret :code:`gcr-pull-secret`, which is used to pull images from the intelligeni repository

3. A :code:`pubsub-key` secret which is a valid Pub/Sub account(or google account) credentials file, having access to the configured Pub/Sub as well as Google Cloud Storage.

4. (Only if ingress is enabled) A certificate to validate tls :code:`intelligeni-ca-cert` - as a kubernetes secret.

5. (Only if ingress is enabled) A host (domain name) to replace all *dev.intelligeni.com* values in the values.yaml file.


----------
Next Steps
----------
