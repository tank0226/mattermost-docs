Manage custom groups
====================

.. include:: ../../_static/badges/ent-pro-cloud-selfhosted.rst
  :start-after: :nosearch:

Custom groups reduce noise and improve focus by notifying the right people in a channel at the right time, while maintaining transparency for all members in that channel. Custom user groups let you notify up to 256 users at a time rather than notifying users individually.

For example, perhaps you want to @mention a cross-functional team about a bug fixes needed for an upcoming feature release, without notifying everyone else in the channel. Using a custom group notifies the cross-functional team immediately, while keeping important stakeholders in the loop on the status of the feature release.

Or perhaps you want to add a group of users to a team and a channel. When you @mention a custom group in a channel, Mattermost prompts you to add anyone from that custom group who isn't already a channel and team member. See the :doc:`invite people to your workspace </end-user-guide/collaborate/invite-people>` documentation for details.

Once a custom user group has been created, you can mention that group the same way you @mention another Mattermost member. See the :doc:`mention people in messages </end-user-guide/collaborate/mention-people>` documentation for details.

.. note::

  - System admins need to enable this feature. See our :ref:`Mattermost Configuration Settings <administration-guide/configure/site-configuration-settings:enable custom user groups>` documentation for details.
  - From Mattermost v7.2, system admins can limit who can manage custom user groups through the Custom Group Manager system admin role. See the :doc:`delegated granular administration </administration-guide/onboard/delegated-granular-administration>` documentation for details.
  - The ability to create custom user groups on mobile will be available in a future release. @mentions for custom user groups on mobile work the same as :ref:`LDAP-synced groups <end-user-guide/collaborate/mention-people:@groupname>`.

Create a custom group
---------------------

1. Using Mattermost in a web browser or the desktop app, select |plus| at the top of the channel sidebar, then select **Create New User Group**.

2. Specify a name and mention. The mention is the handle you use to @mention a notification to the group. Group names must be unique across the Mattermost :doc:`workspace </end-user-guide/end-user-guide-index>`. If a name is in use as a channel name, display name, or another custom group's name, it won't be available.

3. Search for and select members to add to the custom user group, then select **Create Group**.

Review group members
--------------------

From Mattermost v7.8, using Mattermost in a web browser or the desktop app, select a group mention in a thread to display a list of group members.

Manage custom user groups
-------------------------

You can review and filter the list of custom groups, add people to an existing group, edit the group name or mention, leave the group, or archive the group.

To manage a custom user group in a web browser or the desktop app, select **User Groups** from the Products menu, then select the group you want to modify.

.. image:: ../../images/access-user-groups.png
  :alt: Access tools to manage user groups.

Review available groups
~~~~~~~~~~~~~~~~~~~~~~~

Review a list of all available custom user groups, search for specific groups by name.

.. tip::

  You can filter the list of groups to display only groups you're a member of, or only archived groups.

Change name or mention
~~~~~~~~~~~~~~~~~~~~~~

1. From the **More Actions** |more-icon-vertical| icon to the right of any custom group, select **View Group**.

  .. image:: ../../images/manage-user-groups.png
    :alt: Access tools to manage your custom user groups.

2. From the **More Actions** |more-icon-vertical| icon, select **Edit Details**.

  .. image:: ../../images/edit-custom-group.png
    :alt: Edit details of a custom user group.

3. Update the **Name** or **Mention**, then select **Save Details**.

Add people
~~~~~~~~~~

1. Select **Add People**.
2. Search for and select people to add to the group, then select **Add People**.

Remove people
~~~~~~~~~~~~~

Hover over a member, then select the **Trash** icon to remove them from the group.

Join a group
~~~~~~~~~~~~

While viewing the members of a group, from the **More Actions** |more-icon-vertical| icon, select **Join Group**.

Leave a group
~~~~~~~~~~~~~

From the **More Actions** |more-icon-vertical| icon, select **Leave Group**.

Archive group
~~~~~~~~~~~~~

From the **More Actions** |more-icon-vertical| icon, select **Archive Group**. When you archive a custom user group, you won’t be able to mention the group’s handle or view its members. However, the group isn't deleted from the list, and all members remain in the group unless they're manually removed.

Unarchive group
~~~~~~~~~~~~~~~

From Mattermost v9.1, you can restore an archived group. From the **More Actions** |more-icon-vertical| icon, filter the list of groups to show only archived groups. Select an archived group view details about the group, if preferreed, and then select **Restore Group**.
