Enterprise roll out checklist
==============================

.. include:: ../../_static/badges/ent-selfhosted.rst
  :start-after: :nosearch:

This checklist is intended to serve as a guide to Enterprises who are rolling out Mattermost to thousands of users. 

Checklist overview
-------------------

Prepare for the roll out
~~~~~~~~~~~~~~~~~~~~~~~~

- `1. Define the roll out project`_ 
- `2. Validate essential security and compliance requirements`_ 
- `3. Create development, staging, and production environments`_ 
- `4. Configure and customize your Mattermost site`_  
- `5. Test production performance and redundancy`_ 

Roll out Mattermost
~~~~~~~~~~~~~~~~~~~~

- `1. Define your team and channel strategy`_ 
- `2. Enable key integrations`_ 
- `3. Prepare for user onboarding`_ 
- `4. Deploy client apps`_  
- `5. Roll out to groups of users`_ 
- `6. Drive adoption`_ 

Review the roll out
~~~~~~~~~~~~~~~~~~~

- `1. Review project charter success metrics`_ 
- `2. Review and analyze usage`_ 
- `3. Analyze system performance`_ 
- `4. Harden security`_  
- `5. Perform maintenance tasks`_ 

Checklist details
-------------------

Prepare for the roll out
~~~~~~~~~~~~~~~~~~~~~~~~

Much of the preparation work is focused on ensuring the environment is deployed and secured prior to onboarding users. 

1. Define the roll out project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Define key stakeholders and project team members

 - Example project team members: Project Manager, Network Administrator, Database Administrator, Corporate Directory Administrator, Security & Compliance Administrator(s), User Support, User Champions, User Trainers
  
- Define use cases and requirements for teams, their workflows and their integrations

 - Resource: https://mattermost.com/blog/27-things-enterprises-can-learn-startups-increase-productivity/
 
- Define success criteria, goals and metrics to measure success
  
- Create a Project Charter to document goals, tasks, deliverables, and decisions 

 - Get buy-in from project team members and key stakeholders on the project charter 

2. Validate essential security and compliance requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Review Mattermost security features

 - Resource: https://docs.mattermost.com/about/security.html
 
- Determine monitoring requirements

 - Database, network, storage, log integrity
 - Identify fields for log management tools (e.g. Splunk Enterprise event data)

- Determine environment access policies

 - Network access, physical access, group controlled access

- Determine encryption policies

 - Resource: https://docs.mattermost.com/deployment-guide/encryption-options.html
 - Resource: https://docs.mattermost.com/install/transport-encryption.html

- Determine system administration access policies

 - Identify the list of users or groups who need administrative access for Mattermost System Console, Command Line Tools, and API privileges

- Define and configure authentication policies

 - Resource: https://docs.mattermost.com/about/corporate-directory-integration.html

- Determine requirements for multi-factor authentication

 - Resource: https://docs.mattermost.com/administration-guide/onboard/multi-factor-authentication.html

- Configure and test SSO or Corporate Directory integration (SAML or AD/LDAP)

 - Resource: https://docs.mattermost.com/administration-guide/onboard/sso-saml.html
 - Resource: https://docs.mattermost.com/administration-guide/onboard/ad-ldap.html

- Define your mobile usage policy

 - Resource: https://docs.mattermost.com/deployment-guide/mobile-overview.html

- Evaluate external network access requirements
 
 - The `Mattermost Marketplace <https://mattermost.com/marketplace>`_ is a service hosted by Mattermost that functions as a central place to store the current versions of available Mattermost integrations.  See :ref:`Enable Remote Marketplace <administration-guide/configure/plugins-configuration-settings:enable remote marketplace>` documentation for details about required external network access.
 - Mattermost supports external GIF providers. See :ref:`GIF Commands <administration-guide/configure/integrations-configuration-settings:enable gif picker>` configuration documentation for details about required external network access.

3. Create development, staging, and production environments
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Finalize production environment design basing hardware on expected usage and requirements for high availability

 - Resource: https://docs.mattermost.com/deployment-guide/application-architecture.html
 - Resource: https://docs.mattermost.com/deployment-guide/index.html 
 - Resource: https://docs.mattermost.com/administration-guide/scale/scaling-for-enterprise.html 
 - Resource: https://docs.mattermost.com/administration-guide/scale/high-availability-cluster-based-deployment.html

- Create development and staging environments

 - Recommend using to test early configurations for database, authentication, file storage, Elasticsearch, prior to setting up high availability and load balancing 
 - Recommend configuring staging to be an identical replication of your production environment

- Create a production environment

 - Install Mattermost

  - Install the number of nodes based on your high availability requirements outlined in your production environment design
  - Recommendation: Use Kubernetes and the Mattermost Operator, with external supported external database and file storage solutions. This will also provide blue/green deployment, rolling upgrades, and canary builds

   - Resource: https://docs.mattermost.com/deployment-guide/server/deploy-kubernetes.html

 - Install and configure database

  - Install the number of read and search replicas based on your high availability requirements outlined in your production environment design

   - Resource: https://docs.mattermost.com/deployment-guide/server/server-architecture.html#database-with-virtual-ips

  - (Optional) Set up configuration management via the database instead of a config file for high available environments

   - Resource: https://docs.mattermost.com/administration-guide/configure/configuration-in-your-database.html

 - Install and configure File Storage

  - Resource: https://docs.mattermost.com/deployment-guide/server/preparations.html#file-storage-preparation

 - Install and configure proxy or load balancers

  - Note: If running Kubernetes and the Mattermost Operator, proxies will be created automatically. 
  - Add SSL Cert

   - Resource: https://docs.mattermost.com/administration-guide/onboard/ssl-client-certificate.html
   - Resource: https://docs.mattermost.com/administration-guide/scale/high-availability-cluster-based-deployment.html#proxy-server-configuration

  - (Optional) Set up certificate-based authentication (CBA) for user or device-based authentication with a digital certificate

   - Resource: https://docs.mattermost.com/administration-guide/onboard/certificate-based-authentication.html

 - Configure SMTP for email notifications

  - Resource: https://docs.mattermost.com/install/smtp-email-setup.html

 - Set up Elasticsearch (highly recommended if your organization anticipates over two million posts)

  - Resource: https://docs.mattermost.com/administration-guide/scale/elasticsearch.html

- Document network configuration

 - Example: https://docs.mattermost.com/getting-started/architecture-overview.html#reference-architectures 

4. Configure and customize your Mattermost site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Login to Mattermost and access the System Console to connect your environment to Mattermost

 - Resource: https://docs.mattermost.com/administration-guide/configure/configuration-settings.html#environment-variables
 - Upload your valid Enterprise License under Edition and License
 - Ensure site URL is set appropriately for your production, dev and staging environments
 - Add your database configuration to **System Console > Environment > Database**
 - Add your Elasticsearch or AWS OpenSearch configuration to **System Console > Environment > Elasticsearch**
 - Add your file storage system configuration to **System Console > Environment > File Storage** 
 - Add your proxy configuration to **System Console > Environment > Image Proxy** 
 - Add your SMTP configuration to **System Console > Environment > SMTP**
 - Enable Push Notifications by adding your server to **System Console > Environment > Push Notification Server**
 - Add your cluster configuration to **System Console > Environment > High Availability**

- Configure your site within the System Console

 - Resource: https://docs.mattermost.com/administration-guide/configure/configuration-settings.html#site-configuration

- Set site access policies including permissions for roles and guest access

 - Permissions Resource: https://docs.mattermost.com/administration-guide/onboard/advanced-permissions.html
 - Guest Access Resource: https://docs.mattermost.com/administration-guide/onboard/guest-accounts.html

5. Test production performance and redundancy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Define and test disaster recovery policy and processes

 - Resource: https://docs.mattermost.com/install/install-kubernetes.html#using-mattermost-operator-functionality
 - Resource: https://docs.mattermost.com/administration-guide/scale/high-availability-cluster-based-deployment.html#upgrade-guide 

- Performance test production environment

 - Load test production environment to verify that it will handle anticipated user load

  - Resource: https://github.com/mattermost/mattermost-load-test

 - Set up Prometheus and Grafana to monitor performance

  - Resource: https://docs.mattermost.com/administration-guide/scale/deploy-prometheus-grafana-for-performance-monitoring.html 

 - Set up alerts in Grafana

  - Resource: https://docs.mattermost.com/administration-guide/scale/deploy-prometheus-grafana-for-performance-monitoring.html

Roll out Mattermost
~~~~~~~~~~~~~~~~~~~
Now that you have an environment in place, we recommend working through the following items in an iterative process. You may need to cycle through each of these topics multiple times to make adjustments to suit your organization as you onboard groups of users. 

1. Define your team and channel strategy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Determine and create a team structure for your environment

 - Recommendation: Start with fewer teams in your early roll out
 - Resource: https://docs.mattermost.com/messaging/organizing-channels.html 

- Determine and create key channels to support your users. A default Town Square channel is created automatically and available on every team.

 - Recommendation: Add a “Support” channel for your users to escalate questions 

- (Optional) Migrate messages and channels from legacy systems

 - Resource: https://docs.mattermost.com/administration-guide/onboard/migrating-to-mattermost.html

2. Enable key integrations
^^^^^^^^^^^^^^^^^^^^^^^^^^

- Build the list of key integrations and tools used by your teams

 - Resource: https://developers.mattermost.com/integrate/getting-started/

- Define use cases and requirements for plugins, bots, webhooks, slash commands 

 - Resource: https://developers.mattermost.com/integrate/other-integrations/

- Set up key integrations (or migrate from POC environments)

 - Resource: https://mattermost.com/marketplace/

- Understand Mattermost API capabilities

 - Resource: https://api.mattermost.com/

3. Prepare for user onboarding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Onboard champion users 

- Onboard trainers and support team
- Create a training plan

 - Resource: https://academy.mattermost.com/

- Define user escalation and support processes

 - Ensure you have set the site’s support URL to your own support team under **System Console > Site Configuration > Customization**

- Notify users in advance of roll out

 - Sample email: https://docs.mattermost.com/getting-started/welcome-email-to-end-users.html

4. Deploy client apps
^^^^^^^^^^^^^^^^^^^^^

- Roll out desktop app 

 - Resource: https://docs.mattermost.com/end-user-guide/collaborate/install-desktop-app.html
 - Resource: https://docs.mattermost.com/deployment-guide/desktop-app.html
 - (Optional) Use the MSI installer to install on Windows machines

  - Resource: https://docs.mattermost.com/deployment-guide/desktop/desktop-msi-installer-and-group-policy-install.html

- Roll out mobile app

 - Resource: https://docs.mattermost.com/deployment-guide/mobile-overview.html
 - (Optional) Use an EMM provider

  - Resource: https://docs.mattermost.com/deployment-guide/mobile/deploy-mobile-apps-using-emm-provider.html 

5. Roll out to groups of users
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Provision user accounts

 - Resource: https://docs.mattermost.com/administration-guide/onboard/user-provisioning-workflows.html 

- (Optional) Bulk Load users

 - Resource: https://docs.mattermost.com/administration-guide/onboard/bulk-loading-data.html 

- Onboard users to teams and channels

 - Recommendation: Use LDAP Group Sync to automate this process

  - Resource: https://docs.mattermost.com/administration-guide/onboard/ad-ldap-groups-synchronization.html

- Implement your training plan to end users on how to use Mattermost

 - Train on using Mattermost
 - Train on how to use integrations

6. Drive adoption
^^^^^^^^^^^^^^^^^

- Incrementally roll out to additional user groups

 - See “Roll Out to Groups of Users”

- Manage support requests and product requests from your end users

 - Resource: https://mattermost.com/support/ 
 - See the process below for escalating to Mattermost

- Enable additional integrations and plugins to support user workflows

 - Resource: https://mattermost.com/marketplace/

- Understand management tools available to support users

 - mmctl Command Line Tool Resource: https://docs.mattermost.com/administration-guide/manage/mmctl-command-line-tool.html
 - Command Line Tools Resource: https://docs.mattermost.c../administration-guide/manage/command-line-tools.html

Review the roll out 
~~~~~~~~~~~~~~~~~~~

We recommend that you review your rollout on a cadence that matches your iterative approach to rolling out to users. Below are some areas to consider.  

1. Review project charter success metrics
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Perform end-user surveys and measure satisfaction

 - Optional resource within Mattermost: https://mattermost.com/marketplace/matterpoll/

- Verify use case fulfillment from original requirements gathering

- Measure your response time and resolution rate for user support issues

- Identify usage gaps and address or create a plan for addressing

2. Review and analyze usage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Review Project Charter success metrics - identify usage gaps and address or create a plan for addressing

- Monitor site and team statistics

 - Resource: https://docs.mattermost.com/administration-guide/manage/statistics.html 
 - Review: Total posts, total teams, total channels, total group chats, total direct chats, top channels, top teams

- Analyze usage by lines of business and peak usage times

3. Analyze system performance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Monitor trends in CPU/memory usage

- Review trends in database connections

- Review trends in Go routines 

- Review trends in concurrent sessions 

4. Harden security
^^^^^^^^^^^^^^^^^^

- Harden security controls around the web, desktop, and mobile security

- Harden configuration management 

- Harden network security

 - Identify additional tests and scans
 - (Optional) Enable Compliance Reporting

  - Resource: https://docs.mattermost.com/administration-guide/comply/compliance-export.html

5. Perform maintenance tasks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- Monitor for security updates (or sign up for email updates)

 - Resource: https://mattermost.com/security-updates/

- Perform the first upgrade

 - Resource: https://docs.mattermost.c../administration-guide/upgrade/upgrading-mattermost-server.html

- Determine upgrade schedule based on Mattermost release schedules and life cycle

 - Resource: https://docs.mattermost.com/about/releases-lifecycle.html

- Run System checks and either address or set address-by date	
