============
Installation
============

Installation of the chart requires-
-----------------------------------

- helm cli (`install <https://helm.sh/docs/intro/install>`_)

To Install
----------

1. Add the Chart repository

.. code-block:: bash

   $ helm repo add <repo_name> https://harbor.intelligeni.com/chartrepo/intelligeni --username <username> --password <password>

2. Install the intelligeni chart

.. code-block:: bash

    $ helm install <release_name> <repo_name>/intelligeni-core

The cluster should be ready in a few minutes ! Happy Helming :)
