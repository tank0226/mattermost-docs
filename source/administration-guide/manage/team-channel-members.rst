Manage team and channel members
================================

.. include:: ../../_static/badges/allplans-cloud-selfhosted.rst
  :start-after: :nosearch:

System admins can manage channel configuration in the System Console, including:

- **Management:** Manage synchronization, moderation, and membership settings.
- **Promoting/demoting Team and Channel Admins:** Team admins and channel admins can be demoted via the System Console.
- **View channel members:** System admins can view members of a channel without having to join the channel.
- **Member count:** Total member count in a team or channel.
- **Archive channels:** System admins can archive channels without having to join the channel.

Teams
------

To view and manage team information, navigate to **System Console > Teams**. Teams can be managed by **Group Sync** or **Invite Only**. The type of management in use may affect the management options available for that team.

Select a team to view its configuration options.

Team profile
~~~~~~~~~~~~

The name and description of the team. 

System admins can archive or unarchive the team from within **User Management > Teams > Team Management**. Archiving a team makes its contents inaccessible for all users. All related information is archived, including posts from the database. Before archiving a team, we recommend that you perform a database backup.

Archive a team
~~~~~~~~~~~~~~~

Select **Archive Team**, then select **Save**. Select **Archive** when prompted to confirm the team archive.

Alternatively, system admins can use the mmctl ``mmctl team archive`` to archive teams. See the :ref:`mmctl product documentation <administration-guide/manage/mmctl-command-line-tool:mmctl team archive>` for details.

Unarchive a team
~~~~~~~~~~~~~~~~~~

Select **Unarchive Team**, then select **Save**.

Alternatively, system admins can use the mmctl ``mmctl team restore`` to archive teams. See the :ref:`mmctl product documentation <administration-guide/manage/mmctl-command-line-tool:mmctl team restore>` for details.

Team management
~~~~~~~~~~~~~~~

.. include:: ../../_static/badges/ent-only.rst
  :start-after: :nosearch:

- When **Sync Group Members** is enabled, the **Synced Groups** list is visible and additional groups can be added.
- When **Sync Group Members** is not enabled, invitation limitations can be selected.

Groups
~~~~~~~

.. include:: ../../_static/badges/ent-only.rst
  :start-after: :nosearch:

You can add and remove groups, as well as promote or demote group members to team admin/member roles.

Members
~~~~~~~~

A list of all members in a channel is visible to system admins. Members can be added and removed from the team members and be promoted or demoted to a team admin/member role. Use the role **Filter** to refine your search results. You can use one filter, or combine filters to search by multiple roles:

- Guest
- Member
- Team admin
- System admin

Channels
---------

To view and manage channel information, navigate to **System Console > Channels**. Channels can be managed by **Group Sync** or **Manual Invites**. The type of management in use may affect the management options available for that channel. Use the **Filter** to refine your search results. You can use one filter, or combine filters to search by channel and management type:

- Public channels
- Private channels
- Archived channels
- Channels that are synchronized to an AD/LDAP Group
- Channels with users who were invited manually (not synced from LDAP)

Select a channel to view its configuration options.

Profile
~~~~~~~

The name and description of the channel. To archive the channel, select **Archive Channel > Save**. The channel is still searchable in the **Channels** list. To unarchive the channel, select **Unarchive Channel** and **Save**.

Channel management
~~~~~~~~~~~~~~~~~~

.. include:: ../../_static/badges/ent-only.rst
  :start-after: :nosearch:

- When **Sync Group Members** is enabled, the **Synced Groups** list is visible and additional groups can be added.
- When **Sync Group Members** is not enabled, you can select whether the channel is **Private** or **Public**.

Advanced access controls
~~~~~~~~~~~~~~~~~~~~~~~~

.. include:: ../../_static/badges/ent-pro-only.rst
  :start-after: :nosearch:

Advanced access control settings enable system admins to restrict actions within specific channels. These actions include:

- **Make channel read-only:** :ref:`Read-only channels <administration-guide/onboard/advanced-permissions:read-only channels>` enable system admins to turn off posting in specified channels.
- **Restrict reactions:** Turn off the ability for members and guests to post reactions.
- **Restrict channel mentions:** Turn off the ability for users to post channel wide mentions (@all/@channel/@here) in specified channels.
- **Channel member management:** Only admins have the ability to add and remove channel members in the specified channels.
- **Bookmarks management:** Only channel and system admins can :doc:`manage channel bookmarks </end-user-guide/collaborate/manage-channel-bookmarks>`.

These settings are modified in **System Console > User Management > Channels**.

.. note:: 
  These settings are applicable only to Guests and Members. System, team, and channel admins aren't affected. If you wish to grant posting ability to a specific member, you must first promote that member to channel admin.

The availability of advanced access control settings can also be affected by existing system and team permissions configurations. If there are existing configurations that override the channel settings you would like to apply, it will be indicated in the user interface. These settings can then be adjusted in the relevant panel in the **Permissions** section of the System Console.

**Configure a channel so that members can post/reply/react but guests can only read and react.**

1. Navigate to **System Console > User Management > Channels**.
2. Select **Edit** next to the name of the channel you want to configure.
3. In the **Create Posts** panel, uncheck **Guests**.
4. In the **Post Reactions** panel, uncheck **Guests** if required.
5. Select **Save**.

The channel is available for all members and guests to access, but guests can only read messages and react to them.

**Create an Announcement Channel where only Channel Admins are able to post (read-only).**

1. Create a new channel (either Public or Private).
2. Navigate to **System Console > User Management > Channels**.
3. Select **Edit** next to the name of the channel you just created (you may need to search for it).
4. In the **Create Posts** panel, uncheck **Guests** and **Members**.
5. In the **Post Reactions** panel, uncheck **Guests** and **Members**.
6. Select **Save**.

The channel is available for all members and guests to access but only Admins can post to the channel.

Groups
~~~~~~

.. include:: ../../_static/badges/ent-only.rst
  :start-after: :nosearch:

You can add and remove groups, as well as promote or demote group members to team admin/member roles.

Members
~~~~~~~

A list of all members in a channel is visible to system admins. Members can be added and removed from the team members and be promoted or demoted to a team admin/member role. Use the role **Filter** to refine your search results. You can use one filter, or combine filters to search by multiple roles:

- Guest
- Member
- Channel admin
- System admin
