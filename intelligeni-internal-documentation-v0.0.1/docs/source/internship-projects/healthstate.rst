Health State Computation Model
================================================

Problem Statement
########

**Intern Name:** Shivangi

**Mentor:** Prabodh

-	**Objective:** Enhance the current health state model to include reachability aspects, behavior model
-	**Approach:** Rule based algorithm (Pregel based); existing libraries/ technologies can be evaluated for introducing better technologies
-	**Outcome expected:** Backend implementation of Health State Computations Model

Technology Stack
########

- Scala with Spark
- GraphX using Pregel API

**GraphX Programming Guide** - `<https://spark.apache.org/docs/latest/graphx-programming-guide.html>`_

**Pregel API** - https://spark.apache.org/docs/latest/graphx-programming-guide.html#pregel-api


Solution
########

**Current Scenario**
*******

- In our existing model, we are calculating the health state individually for all CIs based on the alerts received. 
- We have pregel funtion for only the health score computation.

**Aim**
*******

- Our aim is to achieve the propagation of these health states genarated on individual CI till the last level.
- Using a rule based approach (Pregel based), we will be propagating the health states of the CIs till the service level.
- We will merge both the Health State and Health Score Computation Algorithms.

**Approach**
*******

We will be using a sample knowledge graph as a reference where we have listed down the CIs and relationship between them.


**Sample Knowledge Graph**
*******

.. figure:: sampleKnowledgeGraphwithStates.jpg
    :alt: Process
    :figclass: align-center

 
 
**Tabular Representation of above sample graph**
*******

.. figure:: TabularRepresentationBeforePregel.PNG
    :alt: Process
    :figclass: align-center

For sample date, we are assuming some of the CIs to be in active state which means that they have received some alerts which causes change in their health state. We are maintaining a Set datatype which well serve 2 purposes.

- to store all the health states propagated from different CIs.
- Set will store unique values.

To propagate the health states, we have maintained a rulebook.json file which stores data in the given format.

.. figure:: rulebook.png
    :alt: Process
    :figclass: align-center


**Algorithm** 
*******

Using this rulebook, we are using nested mapping, where our key will be the **source_state** and it will have multiple values which will again be a map having key-value pair of **relationship** and **destination_state**.

- **Map  ( srcState ->  Map  (rel -> dstState)  )**

Logic behind the algorithm is that for each CI in a particular health state, we are checking the value in the map for the src_state as well for the inner map with key as relationship and value as dst_state.

**Comparison for input to Pregel Algorithm** 
############

Before Merge
************
- **For Health State** - (parent_id, Set of States, status, parent_ci_type, timestamp,gradient)

- **For Health Score** - (newVal, inMap, status, parent_id, parent_id, parent_ci_type, oldVal)

After Merge
**********
- **For both Health State and Health Score** - (parent_id, Set of States, status, parent_ci_type, timestamp, gradient, newVal, oldVal, inMap, ci_id)


Result
######

- We have successfully propaged the health states of CIs till the service level.
- Use of external json Rulebook helps us to achieve flexibility and allow us to redefine our rules in future.
- We have merged both the Health State and Health Score Code.


.. figure:: Output.png
    :alt: Process
    :figclass: align-center



**Required Output After running Pregel**
*******

.. figure:: TabularRepresentationAfterPregel.PNG
    :alt: Process
    :figclass: align-center


**Output for Health State**   
*******
 
 
.. figure:: finalOutputState.PNG
    :alt: Process
    :figclass: align-center
    

**Output for Health Score**  
*******
 
.. figure:: finalOutput.PNG
    :alt: Process
    :figclass: align-center
       
