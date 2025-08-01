AD/LDAP setup
=============

.. include:: ../../_static/badges/ent-pro-cloud-selfhosted.rst
  :start-after: :nosearch:

Overview
--------

Mattermost offers “Same Sign-On” with Microsoft AD/LDAP (formally known as Active Directory/LDAP). Enable the same credentials used in on-prem AD/LDAP deployments to be reused in Mattermost, with optional :doc:`multi-factor authentication </administration-guide/onboard/multi-factor-authentication>`.

AD/LDAP is a service that stores authentication and authorization details of users on your organization's network. When you integrate your AD/LDAP system with Mattermost, users can log into Mattermost without having to create new credentials. User accounts are managed in AD/LDAP, and changes are synchronized with Mattermost.

Mattermost provides a step-by-step AD/LDAP setup wizard in the System Console that guides you through the configuration process with sections and incremental testing to ensure each part of your setup works correctly before proceeding to the next step.

Benefits of integrating AD/LDAP with Mattermost include:

- **Single sign-on.** Users can log in to Mattermost with their AD/LDAP credentials.
- **Centralized identity management.** Mattermost accounts can display user information from AD/LDAP, such as first and last name, email, and username.
- **Automatic account provisioning.** A Mattermost user account is automatically created the first time a user signs in with their AD/LDAP credentials.
- **Sync groups to predefined roles in Mattermost.** Assign team and channel roles to groups via AD/LDAP Group Sync.
- **Compliance alignment with administrator management.** Manage Administrator access to Mattermost in the System Console using AD/LDAP filters.

Pre-installation notes
-----------------------

If you're using AD/LDAP with **nested security groups** you need to write a PowerShell script, or similar, to flatten and aggregate the tree into a single security group to map into Mattermost.

We strongly recommend the following as you prepare to set up AD/LDAP:

Attribute Naming and Case Sensitivity:

* Attribute names in both AD/LDAP and Mattermost configurations are **case-sensitive**.
* Ensure that the attribute names in the AD/LDAP claim rules **exactly match** the expected attribute names in Mattermost. Case deviations will result in issues.

Essential attributes:

* The ``NameID`` element is required for user identification in SAML assertions.
* All required attributes (e.g., ``Email``, ``Username``, ``FirstName``, and ``LastName``) must be included and correctly mapped.

How to choose a stable unique identifier for ``NameID``:

* Using a stable and unique identifier (``EmployeeID`` or ``ObjectGUID``) for the ``NameID`` helps prevent issues in cases where user details could change over time (e.g., ``LastName`` or ``Email``).
* If stable, unique attributes aren't available in AD, using attributes that might change over time can result in future issues.

Getting started
----------------

There are two ways to set up AD/LDAP:

1. **Configure AD/LDAP using the System Console setup wizard**

  - Log in to your workspace and create a new account using email and password. This is assigned the system admin role as the first user created.
  - Next, use the AD/LDAP setup wizard to configure AD/LDAP step-by-step, testing each section as you go, and then convert your system admin account to use the AD/LDAP login method.

2. **Configure AD/LDAP by editing ``config.json``**

  - Edit ``config.json`` to enable AD/LDAP based on the :ref:`AD/LDAP settings documentation <administration-guide/configure/authentication-configuration-settings:ad/ldap>`. When you log in to Mattermost the first user to log in with valid AD/LDAP credentials will be assigned the system admin role.

Configure AD/LDAP login
--------------------------

1. **Create a system admin account using email authentication.**

  - Create a new workspace and create an account using email and password, which is automatically assigned the **system admin** role since it is the first account created. You may also assign the role to another account.

2. **Configure AD/LDAP using the setup wizard.**

  - Go to **System Console > Authentication > AD/LDAP** to open the AD/LDAP setup wizard. The wizard is organized into sections that you can navigate through using the sidebar:
  
    - **Connection Settings**: Configure server connection details
    - **User Filters**: Set up user identification and filtering
    - **Account sync**: Map AD/LDAP attributes to Mattermost user fields
    - **Group Synchronization**: Configure group settings and group attributes (if using LDAP groups)
    - **Sync Performance**: Adjust synchronization timing and performance settings
    - **Sync History**: View synchronization status and manually trigger syncs
  
  - Each section includes a test button that allows you to verify your configuration before proceeding to the next step. This incremental testing helps identify and resolve issues early in the setup process.

3. **Confirm that AD/LDAP sign-on is enabled.**

  - After configuring AD/LDAP through the wizard, confirm that users can log in using AD/LDAP credentials.

4. **Switch your system admin account from email to AD/LDAP authentication.**

  - Navigate to your profile, and select **Security > Sign-in Method > Switch to AD/LDAP** and log in with your AD/LDAP credentials to complete the switch.

5. **(Optional) Restrict authentication to AD/LDAP.**

  - Go to **System Console > Authentication > Email** and set **Enable sign-in with email** to **false** and **Enable sign-in with username** to **false**.
  - Then choose **Save** to save the changes. This should leave AD/LDAP as the only login option.

6. **(Optional) If you configured `First Name Attribute` and `Last Name Attribute` in the System Console.**

  - Navigate to **System Console > Site Configuration > Users and Teams** and set **Teammate Name Display** to **Show first and last name**. This is recommended for a better user experience.

.. note::

   If you've made a mistake and lock yourself out of the system somehow, you can set an existing account to system admin using the :ref:`mmctl roles <administration-guide/manage/mmctl-command-line-tool:mmctl roles>` command.

Configure AD/LDAP synchronization
----------------------------------

In addition to configuring AD/LDAP sign-in, you can also configure AD/LDAP synchronization. When synchronizing, Mattermost queries AD/LDAP for relevant account information and updates Mattermost accounts based on changes to attributes (first name, last name, and nickname). When accounts are disabled in AD/LDAP users are deactivated in Mattermost, and their active sessions are revoked once Mattermost synchronizes the updated attributes.

The AD/LDAP synchronization depends on email. Make sure all users on your AD/LDAP server have an email address, or ensure their account is deactivated in Mattermost.

When Mattermost is configured to use AD/LDAP for user authentication, the following user attribute changes can't be made through the API: first name, last name, position, nickname, email, profile image, or username. LDAP must be the authoritative source for these user attributes.

To configure AD/LDAP synchronization with AD/LDAP sign-in:

1. Go to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard and navigate to the **Connection Settings** section. Set **Enable Synchronization with AD/LDAP** to **true**.

2. Navigate to the **Sync Performance** section and configure the **Synchronization Interval (minutes)** to specify how often Mattermost accounts synchronize attributes with AD/LDAP. The default setting is 60 minutes. The profile picture attribute is only synchronized when the user logs in.

  - If you want to synchronize immediately after disabling an account, use the **AD/LDAP Synchronize Now** button in the **Sync History** section of the wizard.
  - To configure AD/LDAP synchronization with SAML sign-in, see the :doc:`SAML documentation </administration-guide/onboard/sso-saml>`.

.. note::

   - Ensure at least one AD/LDAP user is in Mattermost or the sync won't complete.
   - Synchronization with AD/LDAP settings in the System Console can be used to determine the connectivity and availability of arbitrary hosts. System admins concerned about this can use custom admin roles to limit access to modifying these settings. See the :ref:`delegated granular administration <administration-guide/onboard/delegated-granular-administration:edit privileges of admin roles (advanced)>` documentation for details.

3. From Mattermost v10.9, you can configure Mattermost to automatically :ref:`re-add members of an LDAP group to group-synchronized teams or channels <administration-guide/configure/authentication-configuration-settings:re-add removed members on sync>` during LDAP synchronization, even if those members were previously removed. This option enables you to maintain uninterrupted collaboration and address specific organizational needs, ensuring users who were unintentionally removed due to changes in LDAP group membership, synchronization errors, or exceptions to the standard group sync rules can be seamlessly restored.

  .. note::

    The :ref:`mmctl ldap sync <administration-guide/manage/mmctl-command-line-tool:mmctl ldap sync>` command takes precedence over this server configuration setting. If you have this setting disabled, and run the mmctl command with the ``--include-removed-members`` flag, removed members will be re-added during LDAP synchronization.

Configure AD/LDAP sign-in using filters
----------------------------------------

Using filters assigns roles to specified users on login. To access AD/LDAP filter settings, navigate to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard and go to the **User Filters** section.

User filter
~~~~~~~~~~~

(Optional) Enter an AD/LDAP filter to use when searching for user objects. Only the users selected by the query will be able to access Mattermost. For AD/LDAP, the query to filter out disabled users is ``(&(objectCategory=Person)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))``.

1. Navigate to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard.
2. Go to the **User Filters** section and complete the **User Filter** field.
3. Use the **Test Filters** button to verify your filter works correctly.
4. Choose **Save**.

When the user accesses Mattermost, they log in with same username and password that they use for organizational logins.

Filters can also be used for excluding users who belong to certain groups. For AD/LDAP, the query to filter out groups is ``(&(memberof=cn=ACME_ALL,ou=Users,dc=sademo,dc=com)(!(memberof=cn=DEV_OPS,ou=Users,dc=sademo,dc=com)))``.

Guest filter
~~~~~~~~~~~~

(Optional) When enabled, the Guest Filter in Mattermost identifies external users whose AD/LDAP role is guest and who are invited to join your Mattermost workspace. These users will have the Guest role applied immediately upon first login instead of the default member user role. This eliminates having to manually assign the role in the System Console.

If this filter is removed/changed, active guests will not be promoted to a member and will retain their Guest role. Guests can be promoted in **System Console > User Management**.

1. Navigate to **System Console > Authentication > Guest Access** and set Guest Access to ``true``.
2. Navigate to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard.
3. Go to the **User Filters** section and expand **Configure additional filters**.
4. Complete the **Guest Filter** field.
5. Use the **Test Filters** button to verify your filter works correctly.
6. Choose **Save**.

When a guest logs in for the first time they are presented with a default landing page until they are added to channels.

See the :doc:`Guest Accounts documentation </administration-guide/onboard/guest-accounts>` for more information about this feature.

Admin filter
~~~~~~~~~~~~

(Optional) Enter an AD/LDAP filter to use for designating system admins. The users selected by the query will have access to your Mattermost workspace as system admins. By default, system admins have complete access to the Mattermost System Console. Existing members that are identified by this attribute will be promoted from member to system admin upon next login.

The next login is based upon Session lengths set in **System Console > Session Lengths**. It is recommended that users are demoted to members manually in **System Console > User Management** to ensure access is restricted immediately.

1. Navigate to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard.
2. Go to the **User Filters** section and expand **Configure additional filters**.
3. Set **Enable Admin Filter** to **true**.
4. Complete the **Admin Filter** field.
5. Use the **Test Filters** button to verify your filter works correctly.
6. Choose **Save**.

.. note::

     If the Admin Filter is set to ``false``, the member's role as system admin is retained. However if this filter is removed/changed, system admins that were promoted via this filter will be demoted to members and won't retain access to the System Console.

When this filter isn't in use, members can be manually promoted/demoted via **System Console > User Management**.

Configure AD/LDAP deployments with multiple domains
-----------------------------------------------------

Organizations using multiple domains can integrate with Mattermost using a "Forest" configuration to bring together multiple domains. Please see `Forests as Collections of Domain Controllers that Trust Each Other <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10)?redirectedfrom=MSDN>`_ for more information.

For forest configurations that contain multiple domains which do NOT share a common root, you can search across all of the domains using the Global Catalog. To do so, update your ``config.json`` as follows:

- Set the LdapPort to 3268 (instead of 389)
- Set the BaseDN to " " (A single space character)

See `Global Catalog and LDAP Searches <https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc978012(v=technet.10)?redirectedfrom=MSDN>`_ for additional details.

Troubleshooting/FAQ
-------------------

The following are frequently asked questions and troubleshooting suggestions on common error messages and issues. It is recommended that you check your logs for errors as they can provide an idea of what the issue is.

If the **Test Connection** button fails, how can I troubleshoot the connection?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check that your AD/LDAP connection settings are correct by running an AD/LDAP user query in an external system. See `LDAP Connection Test Example <https://ldaptool.sourceforge.net/>`__. If the AD/LDAP connection is verified to be working outside of Mattermost, try the following:

- Check your AD/LDAP system to verify your ``Bind Username`` format.
- Check your **AD/LDAP Port** and **Connection Security** settings in the System Console. (**AD/LDAP Port** set to 389 typically uses **Connection Security** set to ``None``. **AD/LDAP Port** set to 636 typically ties to **Connection Security** set to **TLS**).
- If you're seeing ``x509: certificate signed by unknown authority`` in your logs, try installing an intermediate SSL certificate or have your LDAP server send the complete certificate chain.

If these options don't work, please `contact our support team <https://mattermost.com/support/>`_.

When I first set up and synchronize AD/LDAP, are the users automatically created in Mattermost?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No, each user is created on their first login.

When I try to synchronize AD/LDAP, why does the status show as ``Pending`` and not complete?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Go to **System Console > Authentication > AD/LDAP** to open the AD/LDAP wizard, navigate to the **Connection Settings** section, and make sure that the **Enable Synchronization with AD/LDAP** setting is set to **true**.

If the issue persists, try selecting the **Test Filters** button to test that the User Filter is correctly formatted. Refer to this :ref:`document <administration-guide/configure/authentication-configuration-settings:user filter>` for guidance on setting a correct syntax format.

Make sure that you also have at least one AD/LDAP user in Mattermost or the synchronization will not complete.

What's the difference between the Username Attribute, ID Attribute, and Login ID Attribute?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are three AD/LDAP attributes that apear to be similar but serve a different purpose:

1. **Username Attribute:** Used within the Mattermost user interface to identify and mention users. For example, if **Username Attribute** is set to ``john.smith``, a user typing ``@john`` will see ``@john.smith`` in their autocomplete options and posting a message with ``@john.smith`` will send a notification to that user that they’ve been mentioned.
2. **ID Attribute:** Used as the unique identifier in Mattermost. It should be an AD/LDAP attribute with a value that does not change, such as ``ObjectGUID``. If a user's ID attribute changes, it will create a new Mattermost account unassociated with their old one. If you need to change this field after users have already logged in, use the :ref:`mattermost ldap idmigrate mmctl tool <administration-guide/manage/mmctl-command-line-tool:mmctl ldap idmigrate>`.
3. **Login ID Attribute:** The attribute in the AD/LDAP server used to log in to Mattermost. Normally this attribute is the same as the **Username Attribute** field above, or another field that users can easily remember.

How do I deactivate users?
~~~~~~~~~~~~~~~~~~~~~~~~~~

If a user has logged into Mattermost through AD/LDAP or SAML, you can choose how they are deactivated, whether manually or automatically.

There are three main ways to do this:

1. **User deletion:** If the user is completely removed from the AD/LDAP server, they will be deactivated in Mattermost on the next synchronization.
2. **User filter:** Set the :ref:`user filter <administration-guide/configure/authentication-configuration-settings:user filter>` to only select the subset of AD/LDAP users you want to have access to Mattermost. When someone is removed from the selected group, they will be deactivated in Mattermost on the next synchronization.
3. **Manually deactivate**: Go to **System Console > User Management > Users**, select a user's role, and select **Deactivate**. When you manually deactivate a user, they can reactivate themselves by logging back in.

For AD/LDAP, to filter out deactivated users you must set the user filter to:

``(&(objectCategory=Person)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))``

Filters can also be used for excluding users who belong to certain groups. For AD/LDAP, the query to filter out groups is: 

``(&(memberof=cn=ACME_ALL,ou=Users,dc=sademo,dc=com)``

``(!(memberof=cn=DEV_OPS,ou=Users,dc=sademo,dc=com)))``

When a user is deactivated in Mattermost via options one or two above, all the user's current sessions are revoked and they will be unable to log in or access Mattermost.

Can I connect to multiple AD/LDAP servers?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There is currently no built-in way to connect to multiple AD/LDAP servers. You will need to connect the instances in a forest before connecting to Mattermost. Consider upvoting the `feature request <https://portal.productboard.com/mattermost/33-what-matters-to-you>`_ on our forum.

When trying to log in, I see the error ``AD/LDAP not available on this server``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This indicates that there is a problem somewhere with your configuration. We recommend that you check your Mattermost configuration settings to ensure that AD/LDAP is enabled, and the settings are correct.

If you're still having issues, you can `contact support <https://mattermost.com/support/>`__ for additional troubleshooting.

I see the error ``User not registered on AD/LDAP server``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This means the query sent back to the AD/LDAP server returned no results. We recommend that you:

1. Check that the user credentials were entered properly - you should log in with the field set as the :ref:`*ID Attribute* <administration-guide/configure/authentication-configuration-settings:id attribute>`.
2. Check that the user account exists in the AD/LDAP server.
3. Check the AD/LDAP configuration settings are correct.

If you're still having issues, you can `contact Mattermost Support <https://mattermost.com/support/>`__ for additional troubleshooting.

I updated a user account in AD/LDAP, and they can no longer log in to Mattermost
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the user can no longer log in to Mattermost with their AD/LDAP credentials - for example, they get an error message ``An account with that email already exists``, or a new Mattermost account is created when they try to log in - this means the **ID Attribute** for their account has changed.

The issue can be fixed by changing the value of the field used for the **ID Attribute** back to the old value. If you're currently using a field that sometimes changes for an **ID Attribute** (e.g. username, email that changes when someone gets married), we recommend you switch to using a non-changing field such as a GUID.

To do this, you can set the :ref:`Login ID Attribute <administration-guide/configure/authentication-configuration-settings:id attribute>` to whatever you would like users to log in with (e.g. username or email).

.. note::
   Currently the value is case sensitive. If the **ID Attribute** is set to the username and the username changes from ``John.Smith`` to ``john.smith``, the user will experience problems logging in.

I see the log error ``LDAP Result Code 4 "Size Limit Exceeded"``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This indicates that your AD/LDAP server configuration has a maximum page size set and the query coming from Mattermost is returning a result set in excess of that limit.

To address this issue you can set the :ref:`max page size <administration-guide/configure/authentication-configuration-settings:maximum page size>` in your Mattermost configuration to match the limit on your AD/LDAP server. This will return a sequence of result sets that do not exceed the max page size, rather than returning all results in a single query. A max page size setting of 1500 is recommended.

If the error is still occurring, it is likely that no AD/LDAP users have logged into Mattermost yet. Ensure that at least one AD/LDAP user has logged into Mattermost and re-run the synchronization. The error should disappear at that point.

I see the log error ``Missing NameID Element``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This indicates that the  AD/LDAP server configuration doesn't include the ``NameID`` element in the SAML assertion. The ``NameID`` element is required for user identification in SAML assertions. Ensure the ``NameID`` is mapped to a unique user identifier, such as the user's email address or another stable attribute that isn't subject to change over time.

I see the log error ``Username Attribute is Missing``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``Username`` attribute in the SAML assertion was either missing or is incorrectly named. Verify that all required attributes are included in the SAML assertion. Attribute names are case-sensitive and must match exactly what Mattermost expects. Update the claim rules in AD/LDAP to correctly map LDAP attributes to the expected outgoing claim types, ensuring proper casing (e.g., ``Username`` instead of ``UserName``).

Can the Enter ID User Filter read security groups?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes it can, but make sure that:

- Permissions are correctly configured on the service account you are using.
- Each user object is a direct member of the security group.

How do I know if an AD/LDAP sync job fails?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mattermost provides the status of each AD/LDAP sync job in the **Sync History** section of the AD/LDAP wizard (**System Console > Authentication > AD/LDAP**). Here you can see the number of users updated and if the job succeeded or failed.
