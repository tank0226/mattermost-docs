.. _guest-accounts:

Guest accounts
==============

.. include:: ../../_static/badges/ent-pro-cloud-selfhosted.rst
  :start-after: :nosearch:

Guest accounts in Mattermost are a way to collaborate with individuals, such as vendors and contractors, outside of your organization by controlling their access to channels and team members. For example, guest accounts can be used to collaborate with customers on a support issue or work on a website project with resources from an external design firm.

.. important::
  
  - A system admin must :ref:`enable guest access <administration-guide/configure/authentication-configuration-settings:guest access>` before guests can be invited.
  - Mattermost Enterprise and Professional customers can :doc:`control who can invite guests </administration-guide/onboard/advanced-permissions>` in their organization. By default, only system admins can invite guests.
  - Guest accounts count as a paid user in your Mattermost :doc:`workspace </end-user-guide/end-user-guide-index>`. However, guests aren't automatically added to the default **Town-square** channel when they log in. You must :doc:`invite guests </end-user-guide/collaborate/invite-people>` to individual teams and channels manually. Deactivating a guest account reduces your licensed seat count.
  - You'll identify guest users in Mattermost based on their **GUEST** badge next to their name and profile picture. Channels that contain guests also display the message ***This channel has guests** in the channel header.

Guests account limits
---------------------

Guests can:

- Pin messages to channels
- Use slash commands (excluding restricted commands such as invite members, rename channels, change headers, etc)
- Favorite channels
- Mute channels
- Update their profile
- Use different authentication methods than other users
- Leave channels to which they were added (including Town Square). 

Guests cannot:

- Discover public channels
- Join open teams
- Create direct messages or group messages with members who aren’t within the same channel
- Invite people

Guest authentication
---------------------

Guests can access the Mattermost server via email invitation, and be authenticated using AD/LDAP or SAML 2.0.

Before you proceed, ensure that the authentication method you wish to use is correctly configured on your server and enabled in Mattermost. For configuration steps and technical documentation, see :doc:`Active Directory/LDAP setup </administration-guide/onboard/ad-ldap>` and :doc:`SAML Single-Sign-On </administration-guide/onboard/sso-saml>`.

Converting a member user to a guest won't change the channels they are in. However, they will be restricted from discovering additional channels and are unable to direct message/group message users outside of the channels they are in. They can be added to channels by system admins and other roles that have the correct permissions to invite guests.

Configure AD/LDAP authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When enabled, the **Guest Filter** in Mattermost identifies external users whose AD/LDAP role is ``guest`` and who are invited to join your Mattermost server. These users will have the ``guest`` role applied immediately upon first login instead of the default member user role. This eliminates having to manually assign the role in the System Console.

1. Go to **System Console > Authentication > Guest Access** to enable guest access.
2. Go to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard, navigate to the **User Filters** section, then expand **Configure additional filters**.
3. Complete the **Guest Filter** field.
4. Select **Save**.

If a Mattermost guest user has the ``guest`` role removed in the AD/LDAP system, the synchronization process will not automatically promote them to a member user role. This is done manually via **System Console > User Management**. If a member user has the **Guest Attribute** added, the synchronization processes will automatically demote the member user to the guest role.

When a guest logs in without having any channels assigned to their account, they're advised to talk to a Mattermost system admin. 

Configure SAML 2.0 authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When enabled, the **Guest Attribute** in Mattermost identifies external users whose SAML assertion is guest and who are invited to join your Mattermost server. These users will have the ``guest`` role applied immediately upon first login instead of the default member user role. This eliminates having to manually assign the role in the System Console.

If a Mattermost guest user has the guest role removed in the SAML system, the synchronization processes will not automatically promote them to a member user role. This is done manually via **System Console > User Management**. If a member user has the **Guest Attribute** added, the synchronization processes will automatically demote the member user to the guest role.

1. Go to **System Console > Guest Access** to enable guest access.
2. Go to **System Console > Authentication > SAML 2.0**.
3. Complete the **Guest Attribute** field.
4. Select **Save**.

When a guest logs in without having any channels assigned to their account, they're advised to talk to a Mattermost system admin.

Manage guests
--------------

See the following documentation to learn more about managing guests:

- :doc:`invite people </end-user-guide/collaborate/invite-people>` to teams and channels
- :ref:`manage channel members <end-user-guide/collaborate/manage-channel-members:remove other members from a channel>`
- :ref:`remove people from teams <end-user-guide/collaborate/organize-using-teams:remove people from teams>`

.. tip:: 

  You can also use the ``/kick`` or ``/remove`` slash commands to remove a guest from a channel.

When a guest has been removed from all channels within a team, and if they belong to other teams, they will default into the last channel on the last team they have accessed. If they are removed from all channels on all teams, they'll be taken to a screen letting them know they have no channels assigned.

Promote and demote user roles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

System admins can demote a user from a member to a guest by updating the user's role in **System Console > User Management > Users**. Select the member, then select **Demote to Guest**. All system and custom roles assigned to the demoted user are removed. System admins should also purge all of the demoted guest's sessions by selecting the guest user, then selecting **Revoke Sessions**.

The demoted guest user retains their existing channel and team memberships, but is restricted from discovering public channels and collaborating with users outside of the channels they're in. This is useful if you're already collaborating with external contractors, and want to restrict their abilities within Mattermost.

System admins can also promote a guest to member by updating their role in **System Console > User Management > Users**. Select the guest, then select **Promote to Member**.

.. note::
  
  You can filter the list in **System Console > User Management > Users** to view all guests in the system.

Disable guest accounts
~~~~~~~~~~~~~~~~~~~~~~

To disable the guest accounts feature, go to **System Console > Authentication > Guest Access**, then set **Enable Guest Access** to **False**. To deactivate individual guest accounts, go to **System Console > User Management > Users**. Select a user, then select **Deactivate**. You can re-activate individual guest accounts by selecting **Activate**.

- When a single guest account is deactivated or the guest account feature is disabled, guests are marked as ``deactivated``, are logged out of Mattermost, and all guest sessions are revoked. In Mattermost Server versions prior to 5.18, disabling the guest account feature leaves current guest accounts as activated until they are manually deactivated.
- If you're using AD/LDAP and the guest access setting is disabled, the ``guest`` filter and existing guest users in System Console are deactivated. Additionally, no new guests can be invited or added using the filter as an authentication method. If a previous guest's credentials match the user filter (the only filter which is active when guest access is disabled), they will be reactivated and promoted to a member user upon their next login.
- Similarly, for SAML, when the guest access setting is disabled, the ``guest`` attribute and existing guest users in System Console are deactivated. Additionally, no new guests can be invited or added using the attribute as an authentication method. If a previous guest's credentials match the user attribute (the only attribute which is active when guest access is disabled), they will be reactivated and promoted to a member user upon their next login.

You can disable individual guest accounts in **System Console > User Management** via **Manage Members**. When a single guest account is disabled or the feature is disabled, the guest will be marked as ``deactivated``, be logged out of Mattermost, and all their sessions will be revoked.

Reinstate guest accounts
~~~~~~~~~~~~~~~~~~~~~~~~

When guest access is re-enabled for AD/LDAP, the ``guest`` filter is reinstated. 

New users matching the ``guest`` filter will be authenticated as new guest users on login.

Previous guest users will be activated with the next synchronization. If their credentials still match the ``guest`` filter, they will retain their guest status. If they no longer match the ``guest`` filter but do match the ``user`` filter, they will be not be promoted to member user automatically on login - this must be done manually. If a previous guest was reactivated as a member user when guest access was disabled, and now are identified by the ``guest`` filter once again, they will automatically be demoted to Guest upon their login.

Similarly, for SAML, when guest access is re-enabled, the SAML ``guest`` attribute is reinstated. New users matching the ``guest`` attribute will be authenticated as new guest users on login.

Previous guest users will be activated with the next synchronization. If their credentials still match the ``guest`` attribute, they will retain their guest status. If they no longer match the ``guest`` attribute but do match the ``user`` filter, they will be not be promoted to member user automatically on login - this must be done manually. If a previous guest was reactivated as a member user when guest access was disabled, and now are identified by the ``guest`` attribute once again, they will automatically be demoted to guest upon their login.

Frequently asked questions
---------------------------

How am I charged for guest accounts?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Guests are charged as a user seat.

Why doesn’t Mattermost have single-channel guests?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We wanted to support collaboration with external guests for the broadest use cases without limiting guests' access to channels. In the future, we may consider adding single-channel guests.

Can I set an expiration date for guests?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Currently, you cannot. This feature may be added at a later stage.

Can MFA be applied selectively?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If MFA is enforced for your users, it can be applied to guest accounts. Guests can configure MFA in by going to their profile picture and selecting **Profile > Security**. If MFA is not enforced for your users, it can't be applied to guest accounts.

Has the guest accounts feature been reviewed by an external security firm?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The guest account feature was reviewed by the Mattermost security team. We do not have an external firm review scheduled but will include this feature in future reviews.

How can I validate my guests' identity?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Guests can be authenticated via SAML and/or AD/LDAP to ensure that only the named guest can log in. Alternatively, you can whitelist domains via **System Console > Authentication > Guest Access > Whitelisted Guest Domains**.

Can I restrict guests' ability to upload content?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is not currently possible to selectively disable upload/download functionality as it is a server-wide configuration.
