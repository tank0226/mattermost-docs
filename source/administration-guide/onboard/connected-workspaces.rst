Connected workspaces
====================

.. include:: ../../_static/badges/ent-adv-cloud-selfhosted.rst
  :start-after: :nosearch:

Communicate across organizations, as well as external partners and vendors using Mattermost by synchronizing messages, emoji reactions, and file sharing in real-time through secured, connected Mattermost workspaces.

Connected workspaces in Mattermost behave like regular public and private channels and offer the same user experience and functionality. All members using secure connections, including local members and remote members, can :doc:`send and receive channel messages </end-user-guide/collaborate/send-messages>`, :doc:`use emojis </end-user-guide/collaborate/react-with-emojis-gifs>` to react to messages, :doc:`share files </end-user-guide/collaborate/share-files-in-messages>`, and :doc:`search message history </end-user-guide/collaborate/search-for-messages>`.  Content is synchronized across all participating Mattermost instances.

A channel's permissions and access continues to be governed by each server separately. :ref:`Advanced access control <administration-guide/manage/team-channel-members:advanced access controls>` permissions can be applied to a shared channel, and be in effect on the local Mattermost server while not being in effect on a remote Mattermost server.

Set up connected workspaces
---------------------------

The process of connecting Mattermost workspaces involves the following 5 steps:

1. Ensure that all Mattermost Enterprise servers are running v10.2 or later.

2. `Enable the connected workflows functionality <#enable-connected-workflows>`__ for each Mattermost Enterprise instance you want to connect.

3. System admins must `create a secure and trusted connection <#create-a-secure-connection>`__ with other Mattermost Enterprise instances using the System Console or slash commands. This process involves creating a password-protected, encrypted invitation, creating a strong decryption password, then sending the invitation and password to the system admin of a remote Mattermost instance.

4. When a remote system admin receives the invitation, they must `accept the invitation <#accept-a-secure-connection-invitation>`__ using the System Console or slash commands.

5. Once a trusted relationship is established between 2 Mattermost servers, system admins can `share specific public or private channels <#share-channels-with-secure-connections>`__ with secure connections.

.. note:: 

    - System admins can only create secure connections with other Mattermost Enterprise instances, and can only share channels with secured connections.
    - System admins must use Mattermost to generate a password-protected encrypted invitation code. However, sending secure connection invitations is not completed using Mattermost. System admins must have an independent way to extend the secure connection invitation, such as by email.
    - A channel shared by a host organization cannot be shared from the receiving organization to another organization. Organizations can't share a channel originating from another organization.

Enable connected workflows
---------------------------

System admins must enable connected workspaces functionality for their Mattermost instance. Ensure the following configuration settings are set to ``true`` in ``config.json``:

- ``ConnectedWorkspacesSettings.EnableRemoteClusterService = true``
- ``ConnectedWorkspacesSettings.EnableSharedChannels = true``

See the :ref:`Site Configuration Settings <administration-guide/configure/site-configuration-settings:enable connected workspaces>` documentation for details.

.. note::

    Following an upgrade to Mattermost v10.2 or later, existing configuration values for shared channels, including ``EnableSharedChannels`` and ``EnableRemoteClusterService`` are automatically converted to :ref:`connected workspace configuration settings <administration-guide/configure/site-configuration-settings:enable connected workspaces>` in the ``config.json`` file. The :ref:`deprecated shared channels experimental settings <administration-guide/configure/deprecated-configuration-settings:shared channels settings>` remain in the ``config.json`` file to support backwards compatibility.

Create a secure connection
---------------------------

.. tab:: System Console

    Only system admins can create workspace connections using the System Console.

    1. Go to **Site Configuration > Connected Workspaces**.
    2. Under **Connected Workspaces**, select **Add a connection**, and then select **Create a connection**.
    3. Specify the **Organization Name** for this connection. The remote system admin must specify this name when accepting a connection invitation.
    4. Select the **Destination Team** as the default team where shared channels will be added. This team will be used as the default location for organizing shared channels from this connected workspace.
    5. Select **Save**.

    An invitation consisting of a password-protected AES 256-bit encrypted code blob is generated. The connection is labeled as **Connection Pending** until the remote system admin accepts the invitation.

.. tab:: Slash Commands

    By default, only system admins can use slash commands to create workspace connections. You can grant the ability to **Manage Shared Channels** and **Managed Secure Connections** to Mattermost users by modifying permissions of the :ref:`system scheme <administration-guide/onboard/advanced-permissions:system scheme>` or :ref:`team override scheme <administration-guide/onboard/advanced-permissions:team override scheme>`.

    System admins can :doc:`run the following slash command </end-user-guide/collaborate/run-slash-commands>` to create a secure connection invitation:

    ``/secure-connection create --name <--displayname> --password``

    For example:

    ``/secure-connection create --name AcmeUS --displayname "AcmeUSA" --password examplepassword``

    This slash command creates an invitation consisting of a password-protected AES 256-bit encrypted code blob for a remote Mattermost entity known locally as ``AcmeUS`` with a password of ``examplepassword``. Within Mattermost, this shared connection displays to the local system admin based on the ``name`` and ``displayname`` provided.

Extend the invitation
~~~~~~~~~~~~~~~~~~~~~~
    
You must use a system, other than Mattermost, to share invitation codes and passwords. We strongly recommend sharing invitation codes separately from passwords to ensure that no one has all of the data necessary to take action if the message were compromised. Ensure the remote Mattermost instance can access your Mattermost workspace URL.

.. tab:: System Console

    Once you've created a connection in the System Console, you're prompted to share the invitation code and password with the system admin of the remote Mattermost server you want to connect with. Copy both the invitation code and password to a safe location, then select **Done**.

.. tab:: Slash Commands

    Copy the invitation code blob in the System message, then share the code blob and the decryption password to the remote Mattermost system admin you want to securely connect with.

Accept a connection invitation
-------------------------------

.. tab:: System Console

    1. Go to **Site Configuration > Connected Workspaces**.
    2. Under **Connected Workspaces**, select **Add a connection**, and then select **Accept an invitation**.
    3. Specify the **Organization Name** for this invitation. This must be the same name specified when creating the connection.
    4. Select the **Destination Team** where shared channels will be added. This team will serve as the default location for organizing shared channels from this connected workspace.
    5. Paste the encrypted invitation code and password you've been provided to connect with the remote workspace.
    6. Select **Accept**.

    The system admin who accepts the connection invitation is automatically added to all shared channels.

.. tab:: Slash Commands

    Run the following slash command to accept a secure connection invitation from a remote Mattermost instance:

    ``/secure-connection accept --name --displayname --password --invite [code blob]``

    For example:

    ``/secure-connection accept --name AcmeUS --displayname "AcmeUSA" --password examplepassword --invite [code-blob]``

    This slash command accepts a secure connection invitation from ``AcmeUS``.

Share channels with secure connections
--------------------------------------

Once a connection is established between two Mattermost servers, system admins can share channels across secured workspaces.

.. tab:: System Console

    1. Under **Shared Channels**, select **Add channels**.
    2. Specify the channels you want to share between Mattermost servers.

    Shared channels and members of those shared channels display a shared |shared| icon to distinguish them from channels and channel members of the local server.

.. tab:: Slash Commands

    Run the following slash command to specify the public or private channels to share:

    ``/share-channel invite --connectionID <--readonly>``

    You can extend an invitation that permits remote members to participate in the channel based on their channel and member permissions.

    Alternatively, you can extend a read-only invitation to a secure connection by appending the optional ``--readonly`` parameter to this command. Remote members can't post or reply to messages within shared read-only channels.

    .. tip:: 

        To convert a read-only shared channel to a participation channel, remove the original secured connection from the channel, then re-extend an invitation to that secure connection while omitting the optional ``--readonly`` parameter. For example:

        ``/share-channel invite --connectionID``
 
        This slash command invites the shared connection to the current channel based on its ``connectionID``.

        See `Reviewing Secure Connection Status <#review-secure-connection-status>`_ to find the ``connectionID`` for a shared connection.

Direct message delivery
-----------------------
  
From Mattermost v10.10, creating a direct or group message with remote users across connected workspaces is only available when the feature flag ``EnableSharedChannelsDMs`` is enabled. 

When ``EnableSharedChannelsDMs`` is disabled, the direct message option on a user's profile is disabled and unavailable to ensure users can't attempt direct messages with remote users when connected workspaces isn't enabled on the other Mattermost instance.

Plugin component interaction
-----------------------------

From Mattermost v10.10, plugin interactions such as slash commands, interactive buttons, and other plugin-generated components aren't displayed or accessible in shared channels by default to ensure a consistent experience across different Mattermost instances. 

System admins can enable the ``EnableSharedChannelsPlugins`` feature flag to enable these plugin interactions in shared channels. When plugin components are enabled in shared channels, we recommend ensuring that all connected Mattermost instances have the same plugins installed and configured to avoid inconsistent user experiences. Plugin behaviors can vary between instances if different plugin versions or configurations are used.

Remote user discovery
----------------------

From Mattermost v10.10, remote users across connected workspaces can be discovered for direct or group messages when the feature flag ``EnableSyncAllUsersForRemoteCluster`` is enabled. This feature implements global user synchronization between connected Mattermost instances, making remote users discoverable without requiring them to post in a shared channel first. When enabled, Mattermost uses cursor-based synchronization to efficiently sync user information between connected instances. The system:

- Synchronizes users in configurable batch sizes to prevent timeouts and reduce memory usage
- Tracks synchronization progress using timestamps to enable efficient resumption
- Filters out users from their original cluster to prevent syncing users back to their home instance
- Only syncs users that have been updated since the last synchronization

The feature includes configuration options for :ref:`automatically syncing users when connections are established <administration-guide/configure/site-configuration-settings:sync users on connection open>` and :ref:`controlling batch processing sizes <administration-guide/configure/site-configuration-settings:global user sync batch size>` for optimal performance.

When ``EnableSyncAllUsersForRemoteCluster`` is disabled, remote users are only discoverable in the DM/GM creation modal after they have participated in a shared channel.

.. note::

  Enabling these features can increase the load on your Mattermost server’s CPU, memory, and database due to frequent updates, database queries, and API communication. Excessive sync frequency and retries can overwhelm system resources, potentially causing performance degradation or instability. Monitor your system carefully when enabling these features.

Channel membership synchronization
----------------------------------

From Mattermost v10.10, channel membership synchronization between connected workspaces is controlled by the feature flag ``EnableSharedChannelsMemberSync``. When this feature flag is enabled, channel membership changes are automatically synchronized across all connected workspaces that share the same channel.

When ``EnableSharedChannelsMemberSync`` is enabled:

- Users added to a shared channel on one workspace are automatically added to the corresponding shared channel on all connected workspaces
- Users removed from a shared channel on one workspace are automatically removed from the corresponding shared channel on all connected workspaces
- Membership changes are processed in configurable batch sizes to optimize performance and prevent timeouts
- The system uses cursor-based synchronization to efficiently track and sync membership changes

The feature includes a configuration option for :ref:`controlling batch processing sizes for member synchronization <administration-guide/configure/site-configuration-settings:member sync batch size>` to ensure optimal performance during large membership changes.

When ``EnableSharedChannelsMemberSync`` is disabled, channel membership changes are not synchronized between connected workspaces, and users must be manually added or removed from shared channels on each workspace.

.. note::

  Enabling these features can increase the load on your Mattermost server’s CPU, memory, and database due to frequent updates, database queries, and API communication. Excessive sync frequency and retries can overwhelm system resources, potentially causing performance degradation or instability. Monitor your system carefully when enabling these features.

Manage connections and invitations
----------------------------------

System admins can `edit <#edit-a-connected-workspace>`__ or `delete <#delete-a-connected-workspace>`__ a connected workspace, and `review connection status <#review-connection-status>`__, and `regenerate invitation codes and passwords <#regenerate-invitation-codes-for-pending-connections>`__ for pending connections.

Edit a connected workspace
~~~~~~~~~~~~~~~~~~~~~~~~~~

In the System Console, system admins can change the **Organization Name**, the **Destination Team** (which serves as the default location for organizing shared channels from this connected workspace), or channels shared with a remote Mattermost instance as well as channels shared with your local Mattermost instance.
    
1. Under **Connected Workspaces**, identify the connected workspace you want to change.
2. Select the **More** |more-icon| icon to the right of the connected workspace, and then select **Edit**.

Remove all connections from the current channel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the following slash command to remove all secure connections from the current channel:

``/share-channel unshare``

This slash command removes all secure connections from the current channel. A System message notifies you that the channel is no longer shared. Secure connections may continue to be invited to other shared channels.

Delete a connected workspace
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Deleting a connected server severs the trust relationship between the local Mattermost server and the remote Mattermost server.

From Mattermost v10.10, removing a shared channel from a connected workspace removes the channel from all connected workspaces. The channel is deleted from both the local and remote Mattermost servers. Prior to Mattermost v10.10, removing a shared channel from a connected workspace stops synchronizing the channel with the remote Mattermost server; however, the channel continues to function for local users.

.. tab:: System Console

    1. Under **Connected Workspaces**, identify the connected workspace you want to remove.
    2. Select the **More** |more-icon| icon to the right of the connected workspace, and then select **Delete**.

.. tab:: Slash Commands

    Using slash commands, you can uninvite or delete a secure connection from your Mattermost instance.
    
    Run the following slash command to uninvite a secure connection:

    ``/share-channel uninvite --connectionID``

    This slash command removes a secure connection from the current channel based on its ``connectionID``. The secure connection may continue to be invited to other shared channels.
    
    Run the following slash command to delete a secure connection:

    ``/secure-connection remove --connectionID``

    For example:

    ``/secure-connection remove --connectionID``

    This slash command severs the trust relationship between the local Mattermost server and a remote Mattermost server based on its ``connectionID`` and removes the secure connection from all shared Mattermost channels.

Review connection status
~~~~~~~~~~~~~~~~~~~~~~~~

.. tab:: System Console

    Under **Connected Workspaces**, you can review all connected workspaces and their current status as one of: **Connected**, **Offline**, or **Connection Pending**.

.. tab:: Slash Commands

    Run the following slash command to review the current status of all secure connections established for your Mattermost instance:

    ``/secure-connection status``

    Status details include:

    - Connection ID
    - Connection URL
    - Description
    - Invite accepted (Yes/No)
    - Online (Yes/No)
    - Last ping timestamp (UTC)
    - Deleted

Regenerate invitation codes for pending connections
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When using the System Console to manage connected workspaces, system admins can re-generate invitation codes and passwords for pending connections. 

1. Under **Connected Workspaces**, identify the pending connection whose invitation and password you want to regenerate.
2. Select the **More** |more-icon| icon to the right of the connected workspace, and then select **Regenerate invitation code**.

.. note::

    Regenerating doesn't invalidate the existing password, and the existing password can continue to be used in addition to the newly-generated password. Once a connection invitation is accepted and the workspace displays a status of **Connected**, invitation codes and passwords can't be regenerated.

Frequently Asked Questions
---------------------------

Are special characters supported in secure connection names?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No. When using slash commands, ``--name`` can include periods, hyphens, and/or underscores. You must surround ``--name`` using quotation marks (") when the value contains spaces.

What happens if two Mattermost instances contain different emojis?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In cases where one Mattermost instance has different emojis than another instance, emoji text displays in place of a missing emoji image.

Is a Display Name required for all secure connections?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No. When using slash commands, ``--displayname`` is optional. When omitted, ``--name`` is displayed and used instead.

What information is synchronized between connected workspaces?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, member status and availability for all members of shared channels is synchronized between connected workspaces.

When a user is added to a shared channel, member status is synchronized within a few seconds of the member's status changing. Status updates aren't immediate and don't necessarily display in real-time.

When using Mattermost in a web browser, Mattermost polls the server every minute. Refreshing the browser page triggers immediate synchronization.

By default, a maximum of 50 messages are synchronized at a time, and :ref:`this value is configurable <administration-guide/configure/site-configuration-settings:default maximum posts per sync>`. 

Channel as well as member status and availability synchronization :ref:`can be disabled <administration-guide/configure/site-configuration-settings:disable shared channel status sync>`.

From Mattermost v10.10, channel membership can be synchronized between connected workspaces when the feature flag ``EnableSharedChannelsMemberSync`` is enabled. When a user is added to or removed from a shared channel on one workspace, that membership change is automatically applied to the corresponding shared channel on all connected workspaces. This ensures consistent channel membership across all participating Mattermost instances. Additionally, connected workspaces also synchronize message priority, message acknowledgements, and persistent notifications between connected servers. This ensures that important message indicators and user interactions are consistently reflected across all connected workspace instances.

Do connection interruptions affect message synchronization?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes. A System message is posted in the channel visible to all channel members when message synchronization is interrupted for more than 5 minutes. Once connectivity is restored, a full synchronization will happen for all missed messages, including direct messages and channel links.

What happens if two secure connections share the same usernames?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In cases where members share the same usernames across Mattermost secure connections, usernames on the local server instance are appended with the secure connection name of the remote server.

For example, if multiple members named John Smith exist after two Mattermost instances establish a secure connection with one another, all remote John Smith members include their Secure Connection ID following their username to help differentiate members across multiple Mattermost instances.
