Statistics
==========

.. include:: ../../_static/badges/allplans-cloud-selfhosted.rst
  :start-after: :nosearch:

Statistics on users, posts, and channels are tracked for each system and team. Enterprise Editions have access to advanced system statistics.

.. note::

  To maximize performance for large Enterprise deployments, statistics for total messages, total hashtag messages, total file messages, messages per day, and activated users with messages per day is configurable by changing the ``MaxUsersForStatistics`` value :ref:`in config.json <administration-guide/configure/reporting-configuration-settings:maximum users for statistics>`.

For advanced metrics for Enterprise deployments, :doc:`see performance monitoring documentation to learn more </administration-guide/scale/deploy-prometheus-grafana-for-performance-monitoring>`.

Site statistics
---------------

System statistics are viewable under **System Console > Reporting**. The data shown here is a cumulative sum across all teams on the system.

Total Users
    The total number of active accounts created on your system. Excludes deactivated accounts.

Total Teams
    The total number of teams created on your system.

Total Channels
    The total number of public channels and private channels created in all the teams on your system, including deleted channels. Doesn't include direct message channels.

Total Posts
    The total number of posts made in all the teams on your system, including deleted posts and posts made using automation.

Daily Active Users
  The total number of users who viewed the Mattermost site in the last 24 hours. Excludes bot users.

Monthly Active Users
  The total number of users who viewed the Mattermost site in the last 30 days. Excludes bot users.

Total Posts (graph)
    The total number of posts made on a certain day in all the teams on your system, including deleted posts and posts made using automation.

Total Posts from Bots (graph)
    The total number of posts made by a `bot account <https://developers.mattermost.com/integrate/admin-guide/admin-bot-accounts/>`_ on a certain day in all the teams on your system, including deleted posts and posts made using automation.

Active Users with Posts (graph)
    Users who made a post on a certain day in all the teams on your system, including system messages posted from the user's account.

Advanced system statistics
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. include:: ../../_static/badges/ent-selfhosted-only.rst
  :start-after: :nosearch:

Mattermost Enterprise includes additional system statistics.

Total Sessions
    The number of active user sessions connected to your system. Expired sessions are not counted.

Total Commands
    The number of active slash commands currently set up on your system. Slash commands that are created and then removed in the **Integrations** menu are not counted.

Incoming Webhooks
    The number of active incoming webhooks currently setup on your system. Incoming webhooks that are created and then removed in the **Integrations** menu are not counted.

Outgoing Webhooks
    The number of active outgoing webhooks currently set up on your system. Outgoing webhooks that are created and then removed in the **Integrations** menu are not counted.

WebSocket Conns
    The number of active WebSocket connections currently on your server.

Master DB Conns
    The number of active connections currently on your master database.

Replica DB Conns
    The number of active connections currently on one or more of :ref:`your read replica databases <administration-guide/scale/high-availability-cluster-based-deployment:database configuration>`.

Total Playbooks
    The total number of collaborative playbooks on this server.

Total Playbook Runs
    The total number of runs (active and complete) on this server.

Channel Types
    This chart displays the number of public channels and private channels in a visual format, including channels that might have been deleted.

Posts, Files and Hashtags
    This chart displays the number of posts containing files, hashtags, or only text. Posts containing both files and hashtags are counted in both categories, and deleted posts are included.

Team statistics
---------------

Team Statistics are viewable under **System Console > Team Statistics**. The data shown here is a cumulative sum across this team only, and excludes posts made in Direct Message channels, which are not tied to a team.

Total Users
    The total number of active accounts on this team. Excludes deactivated accounts.

Public Channels
    The number of public channels created in this team. Excludes deleted channels.

Private Channels
    The number of private channels created in this team. Excludes deleted channels.

Total Posts
    The total number of posts made in this team, including deleted posts and posts made using automation. Excludes posts made in Direct Message channels, which are not tied to a team.

Total Posts (graph)
    The total number of posts made on a certain day in this team, including deleted posts and posts made using automation.

Active Users with Posts (graph)
    Users who made a post on a certain day in this team, including system messages posted from the user's account.

Recent Active Users
    Twenty most recent users who have logged in and had recent browser activity in Mattermost.

Newly Created Users
    Most recent users who have joined the team.

Troubleshooting/FAQ
-------------------

I see an error: "Not enough data for a meaningful representation"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the statistics page is loading endlessly and you get an error message saying "Not enough data for a meaningful representation", check whether you're using an ad blocker. An ad blocker can prevent this page from loading data. To test this, temporarily disable your ad blocker, or view the page in a browser without an ad blocker installed.

Can team admins review their own team's statistics?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. include:: ../../_static/badges/ent-selfhosted-only.rst
  :start-after: :nosearch:

Yes. In Mattermost Enterprise, you can enable team admins to see their team's statistics by modifying available delegated granular administration system roles. See the :doc:`delegated granular administration </administration-guide/onboard/delegated-granular-administration>` documentation to learn more about these admin roles, including how to manage privileges and assign roles.

To enable team admins to access their team's statistics:

1. Go to **System Console > User Management > System Roles**, then edit the **Viewer** system admin role. 

.. image:: ../../images/edit-viewer-system-admin-role.png
   :alt: Enable team admins to access their team's statistics in the System Console by going to User Management > System Roles, and making changes to the Viewer role.

2. Under **Privileges**, expand the **Reporting** section, set **Team Statistics** to **Read only**, then set **Site Statistics** and **Server Logs** to **No access**.

3. Set all other privileges to **No access** to restrict all users with the **Viewer** role to access only the **Team Statistics** page in the System Console.   

.. image:: ../../images/restrict-role-access.png
   :alt: On the Viewer page, restrict user access to the Team Statistics page by expanding the Reporting section, setting Site Statistics and Server Logs to No Access, and setting all other privileges to No Access. 

4. Under **Assigned People**, select **Add People** to assign team admins to the **Viewer** role, and enable them to access their team's statistics.

.. image:: ../../images/assign-people-to-system-role.png
   :alt: On the Viewer page, specify which users can access the Team Statistics page by assigning specific users the Viewer admin role. 

5. Select **Save**.

.. note::

    System admins must manually add people to or remove people from the **Viewer** system admin role to address team admin changes, such as promotions or demotions.
