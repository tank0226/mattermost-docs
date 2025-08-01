Scaling for Enterprise 
======================

Mattermost is designed to scale from small teams hosted on a single server to large enterprises running in cluster-based, highly available deployment configurations.

- Mattermost supports any 64-bit x86 processor architecture
- **Databases supported**: PostgreSQL, Amazon RDS for PostgreSQL
- **Storage supported**: Amazon S3 or a local filestore

Server requirements vary based on usage and we highly recommend that you run a pilot before an enterprise-wide deployment in order to estimate full scale usage based on your specific organizational needs.

Backing storage
---------------

Review detailed :doc:`write and read storage benchmark results </administration-guide/scale/backing-storage-benchmarks>` for supported storage options including local file system (EBS, gp3), network file system (EFS), and object storage (S3) to make informed decisions based on your use case and infrastructure needs.

Enterprise search
-----------------

We highly recommend a dedicated server for large enterprise deployments to run highly efficient database searches in a cluster environment. 

For deployments with over 5 million posts, :doc:`Enterprise search </administration-guide/scale/enterprise-search>` using :ref:`Elasticsearch <administration-guide/scale/enterprise-search:elasticsearch>` or :ref:`AWS OpenSearch Service <administration-guide/scale/enterprise-search:aws opensearch service>` is required for optimized search performance, dedicated indexing and usage resourcing via cluster support without performance degradation and timeouts, resulting in faster, more predicable search results. 

High availability
-----------------

A :doc:`high availability cluster-based deployment </administration-guide/scale/high-availability-cluster-based-deployment>` enables a Mattermost system to maintain service during outages and hardware failures through the use of redundant infrastructure.

Redis
-----

:doc:`Redis </administration-guide/scale/redis>` is an in-memory data structure store that can be used as a database, cache, and message broker. Mattermost uses Redis as an external cache to improve performance at scale. When properly configured, Redis can help support Mattermost installations with more than 100,000 users by providing improved performance through efficient caching.

Available reference architectures
---------------------------------

.. toctree::
    :maxdepth: 1
    :hidden:
    :titlesonly:

    Backing storage benchmarks </administration-guide/scale/backing-storage-benchmarks>
    Enterprise search </administration-guide/scale/enterprise-search>
    High availability </administration-guide/scale/high-availability-cluster-based-deployment> 
    Redis </administration-guide/scale/redis>
    Scale up to 200 users </administration-guide/scale/scale-to-200-users>
    Scale up to 2000 users </administration-guide/scale/scale-to-2000-users>
    Scale up to 15000 users </administration-guide/scale/scale-to-15000-users>
    Scale up to 30000 users </administration-guide/scale/scale-to-30000-users>
    Scale up to 50000 users </administration-guide/scale/scale-to-50000-users>
    Scale up to 80000 users </administration-guide/scale/scale-to-80000-users>
    Scale up to 90000 users </administration-guide/scale/scale-to-90000-users>
    Scale up to 100000 users </administration-guide/scale/scale-to-100000-users>
    Scale up to 200000 users </administration-guide/scale/scale-to-200000-users>

The following reference architectures are available as recommended starting points for your self-hosted Mattermost deployment, where user counts refer to the number of concurrent users for a given deployment. The number of concurrent numbers is commonly lower than the total number of user accounts.

* :doc:`Scale up to 200 users </administration-guide/scale/scale-to-200-users>` - Learn how to scale Mattermost to up to 200 users.
* :doc:`Scale up to 2000 users </administration-guide/scale/scale-to-2000-users>` - Learn how to scale Mattermost to up to 2000 users.
* :doc:`Scale up to 15000 users </administration-guide/scale/scale-to-15000-users>` - Learn how to scale Mattermost to up to 15000 users.
* :doc:`Scale up to 30000 users </administration-guide/scale/scale-to-30000-users>` - Learn how to scale Mattermost to up to 30000 users.
* :doc:`Scale up to 50000 users </administration-guide/scale/scale-to-50000-users>` - Learn how to scale Mattermost to up to 50000 users.
* :doc:`Scale up to 80000 users </administration-guide/scale/scale-to-80000-users>` - Learn how to scale Mattermost to up to 80000 users.
* :doc:`Scale up to 90000 users </administration-guide/scale/scale-to-90000-users>` - Learn how to scale Mattermost to up to 90000 users.
* :doc:`Scale up to 100000 users </administration-guide/scale/scale-to-100000-users>` - Learn how to scale Mattermost to up to 100000 users.
* :doc:`Scale up to 200000 users </administration-guide/scale/scale-to-200000-users>` - Learn how to scale Mattermost to up to 200000 users.

.. important::

    Due to constraints in testing, the proxy instance specifications were fixed for all the scaling tests from which we derived these reference architectures. This was done to avoid a combinatorial explosion of variables in tests, but it resulted in minor gaps in our understandings of certain aspects of the reference architectures. In particular, the proxy instance is overspecified for the smaller user counts.

Testing methodology and updates
--------------------------------

All tests were executed with the custom load test tool built by the Mattermost development teams to determine supported users for each deployment size. Over time, this guide will be updated with new deployment sizes, deployment architectures, and newer versions of the Mattermost Server will be tested using an ESR. 

At a high level, each deployment size was fixed (Mattermost server node count/sizing, database reader/writer count/sizing), and unbounded tests were used to report the maximum numbers of concurrent users the deployment can support. Each test included populated PostgreSQL v14 databases and a post table history of 100 million posts, ~200000 users, 20 teams, and ~720000 channels to provide a test simulation of a production Mattermost deployment.

Tests were defined by configuration of the actions executed by each simulated user (and the frequency of these actions) where the coordinator metrics define a health system under load. Tests were performed using the Mattermost v9.5 Extended Support Release (ESR). Job servers weren't used. All tests with more than a single app node had an NGINX proxy running in front of them.

For the last test of 200K users, further infrastructure changes were made. Elasticsearch nodes were added. A Redis instance was added, and multiple NGINX proxies were used to distribute traffic evenly across all nodes in the cluster. More details can be found on the :doc:`scale to 200000 users </administration-guide/scale/scale-to-200000-users>` documentation page.

Full testing methodology, configuration, and setup is available, incluidng a `fixed database dump with 100 million posts <https://us-east-1.console.aws.amazon.com/backup/home?region=us-east-1#/resources/arn%3Aaws%3Ards%3Aus-east-1%3A729462591288%3Acluster%3Adb-pg-100m-posts-v9-5-5>`_. Visit the `Mattermost Community <https://community.mattermost.com/>`_ and join the `Developers: Performance channel <https://community.mattermost.com/core/channels/developers-performance>`_ for details.

The performance specifications and user capacities provided in these reference architectures are based on extensive testing conducted in AWS environments. 

For Azure deployments, the instance specifications have been extrapolated using equivalent configurations, drawing upon AWS performance data and industry expertise. While these configurations are designed to meet the stated requirements, we have not yet conducted formal testing in Azure environments to validate performance under load. Azure Blob Storage support is in active development, and not yet natively supported by Mattermost.

Mattermost load testing tools
-----------------------------

Mattermost provides a set of tools written in Go to help profiling Mattermost under heavy load, simulating real-world usage of a server installation at scale. The `Mattermost Load Test Tool <https://github.com/mattermost/mattermost-load-test-ng>`_ estimates the maximum number of concurrently active users the target system supports, and enables you to control the load to generate.

Visit the `Mattermost Load Test Tool <https://github.com/mattermost/mattermost-load-test-ng/tree/master/docs>`__ documentation on GitHub for details on getting started with the tools, and visit `the Go documentation <https://pkg.go.dev/github.com/mattermost/mattermost-load-test-ng>`_ for code-specific documentation details.

.. important::

    - The Mattermost Load Test Tool was designed by and is used by our performance engineers to compare and benchmark the performance of the service from month to month to prepare for new releases. It's also used extensively in developing our recommended hardware sizing. 
    - We recommend deploying :doc:`Prometheus and Grafana </administration-guide/scale/deploy-prometheus-grafana-for-performance-monitoring>` with our :ref:`dashboards <administration-guide/scale/deploy-prometheus-grafana-for-performance-monitoring:getting started>` for ongoing monitoring and scale guidance.
    - If you encounter performance concerns, we recommend :doc:`collecting performance metrics </administration-guide/scale/collect-performance-metrics>` and sharing them with us as a first troubleshooting step.
