SAML Single Sign-On
===================

.. include:: ../../_static/badges/ent-pro-cloud-selfhosted.rst
  :start-after: :nosearch:

Single sign-on (SSO) is a way for users to log into multiple applications with a single user ID and password without having to re-enter their credentials. The SAML standard allows identity providers to pass credentials to service providers. Mattermost can be configured to act as a SAML 2.0 Service Provider. 

Mattermost can be configured to act as a SAML 2.0 Service Provider, and Mattermost officially supports Okta, OneLogin, and Microsoft ADFS as the identity providers (IDPs), please see links below for more details on how to configure SAML with these providers.

In addition to the officially supported identity providers, you can also configure SAML for a custom IdP. For instance, customers have successfully set up miniOrange, Azure AD, DUO, PingFederate, Keycloak, and SimpleSAMLphp as custom IdPs. Because we do not test against these identity providers, it is important that you test new versions of Mattermost in a staging environment to confirm it will work with your identity provider. You can also set up MFA on top of your SAML provider for additional security.

The SAML Single sign-on integration offers the following benefits:

- **Single sign-on.** Users can log in to Mattermost with their SAML credentials.
- **Centralized identity management.** Mattermost accounts automatically pull user attributes from SAML upon login, such as full name, email, and username.
- **Automatic account provisioning.** Mattermost user accounts are automatically created the first time a user signs in with their SAML credentials on the Mattermost server.
- **Sync groups to predefined roles in Mattermost.** Assign team and channel roles to groups via LDAP Group Sync.
- **Compliance alignment with administrator management.** Manage Administrator access to Mattermost in the System Console using SAML attributes.

.. important::
  - SAML Single sign-on itself does not support periodic updates of user attributes nor automatic deprovisioning. However, SAML with AD/LDAP sync can be configured to support these use cases.
  - Account creation and updates is influenced by the attributes configured for identification. 
  
    - If the email address is used as the unique identifier and it changes, Mattermost might interpret this as a new user. If an ID attribute is not set, or if it's set to use the email address, a new account may be created in Mattermost when the email address changes.
    - If an ID attribute that doesn't change, such as employeeID, is configured, then Mattermost can use this consistent ID to recognize and match the user account. This allows for updates to other attributes, such as email, without creating a new account. The user details are updated based on the non-changing ID attribute, ensuring continuity and correct user identification without duplicate accounts.
  
  - If configuring Mattermost to use the EU-Login system for authentication, please be aware that their `issuerURI` field is what Mattermost calls "Service Provider Identifier".
  - For more information about SAML, see `this article from Varonis <https://www.varonis.com/blog/what-is-saml/>`_, and `this conceptual example from DUO <https://duo.com/blog/the-beer-drinkers-guide-to-saml>`_.
  - See the encryption options documentation for details on what :ref:`encryption methods <deployment-guide/encryption-options:saml encryption support>` Mattermost supports for SAML.

.. toctree::
  :titlesonly:
  :hidden:

  Okta SAML Configuration <sso-saml-okta>
  Generate self-signed certificates </scripts/generate-certificates/gencert>
  OneLogin SAML Configuration <sso-saml-onelogin.rst>
  Microsoft ADFS SAML Configuration for Windows Server 2012 <sso-saml-adfs>
  Microsoft ADFS SAML Configuration for Windows Server 2016 <sso-saml-adfs-msws2016>
  Keycloak SAML Configuration <sso-saml-keycloak>

Using SAML attributes to apply roles
-------------------------------------

You can use attributes to assign roles to specified users on login. To access the SAML attribute settings navigate to **System Console > SAML 2.0**.

Username attribute
~~~~~~~~~~~~~~~~~~

(Optional) Enter a SAML assertion filter to use when searching for users.

1. Navigate to **System Console > Authentication > SAML 2.0** (or **System Console > SAML** in versions prior to 5.12).
2. Complete the **Username Attribute** field.
3. Choose **Save**.

When the user accesses the Mattermost URL, they log in with same username and password that they use for organizational logins.

Guest attribute
~~~~~~~~~~~~~~~

When enabled, the ``guest`` attribute in Mattermost identifies external users whose SAML assertion is guest and who are invited to join your Mattermost server. These users will have the guest role applied immediately upon first login instead of the default member user role. This eliminates having to manually assign the role in the System Console.

If a Mattermost guest user has the guest role removed in the SAML system, the synchronization processes will not automatically promote them to a member user role. This is done manually via **System Console > User Management**. If a member user has the ``guest`` attribute added, the synchronization processes will automatically demote the member user to the guest role.

1. Enable Guest Access via **System Console > SAML 2.0**.
2. Navigate to **System Console > Authentication > SAML 2.0**.
3. Complete the Guest Attribute field.
4. Choose **Save**.

When a guest logs in for the first time they are presented with a default landing page until they are added to channels.

See the :doc:`Guest accounts documentation </administration-guide/onboard/guest-accounts>` for more information about this feature.

Admin attribute
~~~~~~~~~~~~~~~

(Optional) The attribute in the SAML Assertion for designating system admins. The users selected by the query will have access to your Mattermost server as system admins. By default, system admins have complete access to the Mattermost System Console.

Existing members that are identified by this attribute will be promoted from member to system admin upon next login. The next login is based upon Session lengths set in **System Console > Session Lengths**. It is recommended that users are manually demoted to members in **System Console > User Management** to ensure access is restricted immediately.

1. Navigate to **System Console > Authentication > SAML 2.0**.
2. Set **Enable Admin Attribute** to ``true``.
3. Complete the **Admin Attribute** field.
4. Choose **Save**.

.. note:: 
  If the ``admin`` attribute is set to ``false`` the member's role as system admin is retained. However if the attribute is removed/changed, system admins that were promoted via the attribute will be demoted to members and will not retain access to the System Console. When this attribute is not in use, system admins can be manually promoted/demoted in **System Console > User Management**.

Configuration assistance
------------------------

We are open to providing assistance when configuring your custom IdP by answering Mattermost technical configuration questions and working with your IdP provider in support of resolving issues as they relate to Mattermost SAML configuration settings. However, we cannot guarantee your connection will work with Mattermost.

For technical documentation on SAML, see :doc:`sso-saml-technical`.

To assist with the process of getting a user file for your custom IdP, see this `documentation <https://github.com/icelander/mattermost_generate_user_file>`_.

Please note that we may not be able to guarantee that your connection will work with Mattermost, however we will consider improvements to our feature as we are able. You can see more information on getting support `here <https://mattermost.com/support/>`_ and submit requests for official support of a particular provider on our `feature idea portal <https://portal.productboard.com/mattermost/33-what-matters-to-you>`_.
