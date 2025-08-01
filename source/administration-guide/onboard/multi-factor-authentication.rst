..  _auth_mfa:

Multi-factor authentication
===========================

.. include:: ../../_static/badges/allplans-cloud-selfhosted.rst
  :start-after: :nosearch:

Multi-factor authentication (MFA) is a secondary layer of user authentication applied to your Mattermost server that applies to all users on all teams within your Mattermost workspace.

With MFA enabled, users need to provide a secure one-time code in addition to their username and password in order to log in to Mattermost. MFA is useful in organizations that have specific security and compliance policies. It can also be used in organizations where Mattermost is not behind a firewall and doesn't have access to existing MFA infrastructure.

Mattermost offers a smartphone-based MFA check in addition to email-password or Active Directory/LDAP authentication to log in to the Mattermost server.

Supported devices include iOS, Android, or other devices that are able to install a software-based authenticator such as FreeOTP, Google Authenticator, Microsoft Authenticator, 1Password, LastPass, or a similar app. After the app is installed, the device does not require internet access.

.. note::
  
  As the MFA implementation relies on Time-based One-time passwords (TOTP) ensure that your server system time is accurate. If there is a discrepancy, users may not be able to log in successfully.

Enabling MFA
------------

System admins can enable this option by going to **System Console > Authentication > MFA**, then setting **Enable Multi-factor Authentication** to **true**.

Once enabled, users can choose to :doc:`set up multi-factor authentication </end-user-guide/preferences/manage-your-security-preferences>` on their account by selecting **Profile > Security > Multi-factor Authentication** from their profile picture.

Disabling MFA
~~~~~~~~~~~~~

System admins can disable this option by going to **System Console > Authentication > MFA**. When **Enable Multi-factor Authentication** is set to **false**, users can't opt to use or disable multi-factor authentication on their account via **Profile > Security > Multi-factor Authentication**.

MFA can be disabled for user accounts using the API. See the `Mattermost API Reference <https://api.mattermost.com/#tag/users/paths/~1users~1{user_id}~1mfa/put>`__ for details.

Enforcing MFA
--------------

.. include:: ../../_static/badges/ent-pro-only.rst
  :start-after: :nosearch:

Admins can fulfill Multi-Factor Authentication (MFA) compliance requirements by enforcing an MFA requirement for login with email and LDAP accounts. Go to **System Console > Authentication > MFA**, then set **Enforce Multi-factor Authentication** to **true**.

When MFA enforcement is set to **true**, users with email or LDAP authentication who don't have MFA set up will be directed to the MFA setup page when they log in to Mattermost. They will not be able to access the site until MFA setup is complete. Any new users will be required to set up MFA during the sign up process.

Users will not be able to remove MFA from their account while enforcement is on.

.. note::

  Turning on MFA enforcement prevents users from accessing the site until set up is complete. We recommended that you turn on enforcement during non-peak hours when people are less likely to be using Mattermost.
