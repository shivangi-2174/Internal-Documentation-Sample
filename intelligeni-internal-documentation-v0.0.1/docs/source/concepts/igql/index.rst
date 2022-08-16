IgQL
============
***************
Overview
***************
IgQL is a query language that is used to operate on the data stored in
multiple data sources (currently two) in a consistent and simple format.
IgQL syntax is inspired by SQL and Cypher QL. IgQL also supports piping
of multiple queries that allows user to express highly complicated query
in a simple way.

***************
User Guide
***************

Terminology
-------------------

There are two main elements of an IgQL query. 

 - **REPOSITORY**
    Repositories
    are the data domains that you want to search. Basically, a repository is
    the type of data that one wants to query.

 - **ENTITY**
    Entities are the properties of a particular type of data. An
    entity is a field of a particular repository.

Here is a list of various REPOSITORIES which are currently supported by
IqQL. 

 - **SCENARIO**
 - **CI** 
 - **ALERTS** 
 - **SCENRIO_DETAIL** 
 - **IIS**

Clauses and Syntax
------------------------

The syntax for IgQL is very similar to that of SQL. There are three main
parts of a IgQL query. SELECT clause to specify what data we need, FROM
clause to specify from which repository to search and WHERE clause to
add constraints to the query. For example:
::

 SELECT scenarioID, priority
 FROM scenario
 WHERE location = “Singapore”

**Note:** Everything (**except entity names**) in IgQL is **case
insensitive** i.e. select, SELECT and sELeCt are all the same.


SELECT 
^^^^^^^^
SELECT clause is used to specify what to return. - A **" \* "**
is used to get all the fields/properties of a particular repository. For
example, to get all properties of all the CIs, you may query:
::

 SELECT * FROM ci


-  A query can return one or more entities of a from a repository by
   simply specifying comma separated entity names in the SELECT clause.
   For example, query to get scenarioID and priority of all scenarios: >
   **SELECT scenarioID, priority** FROM scenario

-  You may give variable names to repositories using the AS clause.
   You can then specify entities in the format
   {variable_name}.{entity_name}. > **SELECT a.id** FROM ci AS a

FROM
^^^^^^^^

FROM clause is used to specify the repositories which we want to query.
::

 SELECT * FROM ci

::

 SELECT scenarioID FROM scenario

::

 SELECT a.id FROM ci AS a

WHERE
^^^^^^^^

WHERE clause is used to specify conditions and constraints on a query.
 - A simple condition might be a simple expression like ``{left_operand}
   {operator} {right_operand}``. For example,
   ::

    SELECT scenarioID FROM scenario 
    WHERE location = “Singapore”

 - You might specify a set of conditions using logical operators such as
   AND, OR etc.
   ::

    SELECT scenarioID FROM scenario 
    WHERE location = “Singapore” AND priority = “P1”

Time Expression
"""""""""""""""""

You may also use a time expression as a constraint in *WHERE* clause. There are three types of time constraints that you can provide:
 - TIME SINCE
    To get results since the specified interval relative to current time.

   ::

    select * from scenario where TIME SINCE 24 hours and location = "Singapore"

 - TIME BEFORE
    To get results before the specified interval relative to current time.

   ::

    select * from scenario where TIME BEFORE 24 hours and location = "Singapore"

 - TIME BETWEEN
    To get results between the timestamps specified.

   ::

    select * from scenario where TIME BETWEEN "2020-03-15T17:43:01.247Z" TO "2020-03-17T19:43:01.247Z" and location = "Singapore"

Result Expression
"""""""""""""""""""

IgQL supports piping of multiple queries. You may also use previous query results in *WHERE* clause. This can be best understood by having a look at some examples:

 - To get thee impacted cis list from scenarios at location Singapore and find 
   their respective ci types.
   ::

    SELECT impactedCIList FROM scenario WHERE location = 'Singapore' 
    | SELECT citype FROM ci WHERE id in collect(result.impactedCIList)
   
   The **result** in above query refers to the data obtained as result of previous 
   query. 

 - You can also use ``_{n}.result`` to refer result of query number {n}. 
   ::

    SELECT p FROM (ci as c)<-[*0..]-(ci as c2) AS p WHERE c.id = 'SmartCenter::rmcops.microland.com' 
    | SELECT avg(value) AS avg_value FROM derived_metrics WHERE metric_name = 'health' AND TIME SINCE 36 hours GROUP BY parent_id AS cid LIMIT 10000 AT INTERVAL OF 12 hours 
    | TRAVERSE _1.result INSERT TO NODE (healthTimeSeries, _2.result) JOIN ON _2.result.cid
   
   The above query might seem complex because it uses `Data Aggregation and Correlation`_. 


IgQL functions
------------------

Data Aggregation and Correlation
------------------------------------   

Data Aggregation
^^^^^^^^^^^^^^^^^^^^^^^^

Interval Expression
"""""""""""""""""""""""

You can use interval expression to aggregate results over a time bucket.

To get number of alerts on each day since three days :

::

 SELECT count(alertUuid) FROM alerts WHERE TIME SINCE 3 days AT INTERVAL OF 1 day


Group By 
"""""""""""""""""""""""

You can use **group by** to group results over a particular entity.

To get number of alerts since three days grouped by ci on each day :

::

 SELECT count(alertUuid) FROM alerts WHERE TIME SINCE 3 days AT INTERVAL OF 1 day GROUP BY parent_id

IgQL Template Queries
^^^^^^^^^^^^^^^^^^^^^^^^^^

Query examples
-------------------

*******************
Technical Guide
*******************

Parser
-------------

Query Engine
-----------------