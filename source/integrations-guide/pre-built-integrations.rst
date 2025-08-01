Pre-built integrations
=======================

.. include:: ../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

.. toctree::
  :maxdepth: 1
  :hidden:
  :titlesonly:

    GitHub </integrations-guide/github>
    GitLab </integrations-guide/gitlab>
    Jira </integrations-guide/jira>
    Microsoft Calendar </integrations-guide/microsoft-calendar>
    Microsoft Teams </integrations-guide/microsoft-teams-sync>
    Microsoft Teams Meetings </integrations-guide/microsoft-teams-meetings>
    ServiceNow </integrations-guide/servicenow>
    Zoom </integrations-guide/zoom>

Accelerate operational and technical workflows by connecting Mattermost operational and DevOps integrations to your mission-critical tools.

Your Mattermost deployment comes with the following plugins pre-packaged that you can configure and enable. Pre-packaged plugins can be enabled with just a few clicks from the System Console without requiring manual plugin upload, though they are not enabled by default.

* :doc:`GitHub </integrations-guide/github>` - Connect GitHub to Mattermost to subscribe to repositories, stay current with reviews, assignments, and more.
* :doc:`GitLab </integrations-guide/gitlab>` - Connect GitLab to Mattermost to subscribe to repositories, use GitLab events as Mattermost action triggers, and more.
* :doc:`Jira </integrations-guide/jira>` - Connect Jira to Mattermost to create Jira tickets from messages in Mattermost, and get Mattermost notifications for Jira updates.
* :doc:`Mattermost Agents </administration-guide/configure/agents-admin-guide>` - Deploy and manage AI agents for workflow automation and task completion within Mattermost.
* :doc:`Mattermost Boards </end-user-guide/project-task-management>` - Create and manage project boards, tasks, and collaborative workflows directly in Mattermost.
* :doc:`Mattermost Calls </administration-guide/configure/calls-deployment>` - Enable voice and video calls with screen sharing capabilities within Mattermost channels and direct messages.
* :doc:`Mattermost Channel Export </administration-guide/comply/export-mattermost-channel-data>` - Export channel history and data for compliance and archival purposes.
* :doc:`Mattermost Metrics </administration-guide/scale/collect-performance-metrics>` - Collect and analyze performance metrics and usage statistics for your Mattermost deployment.
* :doc:`Mattermost Playbooks </end-user-guide/workflow-automation>` - Create and run structured workflows for incident response, project management, and other repeatable processes.
* :doc:`Mattermost User Survey </administration-guide/configure/manage-user-surveys>` - Collect user feedback and satisfaction data to improve your Mattermost deployment.
* :doc:`Microsoft Calendar </integrations-guide/microsoft-calendar>` - Sync Microsoft Calendar events and receive meeting reminders directly in Mattermost.
* :doc:`Microsoft Teams </integrations-guide/microsoft-teams-sync>` - Synchronize users and data between Microsoft Teams and Mattermost.
* :doc:`Microsoft Teams Meetings </integrations-guide/microsoft-teams-meetings>` - Start and join Microsoft Teams meetings directly from Mattermost.
* `Pexip * <https://mattermost.com/marketplace/pexip-video-connect/>`_ - Connect Pexip to Mattermost to start video conferencing calls in Mattermost with a single click.
* :doc:`ServiceNow </integrations-guide/servicenow>` - Connect ServiceNow to Mattermost to subscribe to record changes in ServiceNow and manage them in Mattermost.
* `Splunk * <https://mattermost.com/marketplace/splunk-2/>`_ - Connect Splunk to Mattermost to receive alerts and notifications from Splunk directly in Mattermost channels.
* :doc:`Zoom </integrations-guide/zoom>` - Connect Zoom to Mattermost to start audio and video conferencing calls in Mattermost with a single click.

Integrations marked with an asterisk (``*``) are available for download via the Mattermost Marketplace. The binary file you download can then be uploaded to your Mattermost Server using the System Console.

Mattermost Marketplace integrations
-----------------------------------

Additional integrations are available on the `Mattermost Marketplace <https://mattermost.com/marketplace/>`_. You can install these integrations directly from the Marketplace, by uploading them in the System Console, or by using the REST API.

.. important::

  Installed plugins are persisted to the configured file store and unpacked on server startup. It's imperative that your file store be accessible to the server immediately on startup. If using a shared filesystem, ensure the mount completes successfully before starting the server. We also strongly recommend testing integration updates in a staging environment before deploying to production, and regularly backing up integrations.