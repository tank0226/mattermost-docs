General deployment troubleshooting
===================================

This document summarizes common deployment troubleshooting issues and resolutions. Some of these suggestions can be done directly, and others may need consultation from your network administrator.

Start Mattermost at system boot
-------------------------------

To have the Mattermost Server start at system boot, the systemd unit file needs to be enabled. Run the following command:

.. code-block:: sh

  sudo systemctl enable mattermost.service

If your database is on the same system as your Mattermost Server, we recommend editing the default ``/lib/systemd/system/mattermost.service`` systemd unit file to add ``After=postgresql.service`` and ``BindsTo=postgresql.service`` to the ``[Unit]`` section.

Run Mattermost without a proxy
------------------------------

Mattermost binds to 443 instead of 8065. The Mattermost binary requires the correct permissions to do that binding. You must activate the ``CAP_NET_BIND_SERVICE`` capability to allow the new Mattermost binary to bind to ports lower than 1024 by running the following command:

.. code-block:: sh

  sudo setcap cap_net_bind_service=+ep ./mattermost/bin/mattermost

.. note::

  We highly recommend using a proxy in front of Mattermost server for up to 200 concurrent users. If you have fewer than 200 concurrent users, you can :doc:`set up TLS </deployment-guide/server/setup-tls>`. If you're exceeding 200 concurrent users, you'll need :doc:`a proxy </deployment-guide/server/setup-nginx-proxy>`, such as NGINX, in front of Mattermost to manage the traffic.

Review Mattermost logs
----------------------

You can access logs for Mattermost and use them for troubleshooting. These steps assume that you have appropriate :doc:`system admin permissions </administration-guide/onboard/advanced-permissions>` to do so.

Mattermost Server logs 
~~~~~~~~~~~~~~~~~~~~~~~

- Ensure that log files are being created: Navigate to **System Console > Environment > Logging**, confirm that **Output logs to file** is set to **true**.
- You can obtain the path for the log files in **System Console > Environment > Logging > File Log Directory**.

The resulting server log file is called ``mattermost.log`` and can be opened with a standard text editor or shared directly.

.. note::
    For a more complete log open **System Console > Environment > Logging**, set **File Log Level** to **DEBUG**, then replicate the issue to log it again. Make sure to revert the file log level to **INFO** after troubleshooting to save disk space.

If filesystem access is not possible, navigate to **System Console > Reporting > Server Logs** to locate the current system logs which can be copied to a file.

You can find more on logging settings :ref:`here <administration-guide/configure/environment-configuration-settings:logging>`.

Mattermost Desktop App logs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Access desktop app logs by going to **Help > Show logs** from the menu bar.

Alternatively, you can access desktop app log files in the following directory:

- **Windows:** ``%userprofile%\AppData\Roaming\Mattermost\logs``
- **Linux:** ``~/.local/share/Mattermost/logs`` OR ``~/.config/Mattermost/logs``
- **MacOS:** ``~/Library/Logs/Mattermost`` (DMG installation) OR ``~Library/Containers/Mattermost.Desktop/Data/Library/Logs/Mattermost`` (Appstore installation only)

Mattermost web logs
~~~~~~~~~~~~~~~~~~~~

The browser-based app does not produce additional log files. If the app has to be debugged, use the development tools integrated in your browser for action history. 

Mattermost Push Notification Service logs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Logging for the Mattermost Push Notification Service is handled via system log with logger and is appended to ``/var/log/syslog``.

Review Mattermost environment
-----------------------------

Put together a timeline to eliminate events prior to the error/problem occurring. For example, if you recently reconfigured your firewall and are now having connection issues it might be worth reviewing the settings or rolling back to see whether that resolves the problem.

- If the problem occurred subsequent to some period of normal operation, did anything change in the environment?

    - Was the client, host, or server upgraded?
    - Was an operating system update applied?
    - Did the network environment change? For example, was a server moved or a domain migrated?
    - Did the system (client or server) recently fail or abnormally terminate?
- How many users are impacted?

    - Is this problem affecting one, some, or all users?
    - Is the problem occurring only for a user who was recently added to the environment, such as a new employee?
    - Do differences exist between the users who are affected and the users who are not affected?

You can also search the error messages online. Existing solutions from our `forum <https://forum.mattermost.com/t/how-to-use-the-troubleshooting-forum/150>`__ can often be found and applied.

Connect to another server
-------------------------

1. Create an account at https://community.mattermost.com.
2. Erase your mobile application and reinstall it.
3. In your mobile app, enter the server URL https://community.mattermost.com and then your login credentials to test whether the connection is working.

Connect with another device
---------------------------

- If you have another mobile device available, try connecting with that to see if your issue still reproduces.
- If you don’t have another device available, check with other teammates to see if they are having the same issue.

Opening a support ticket for self-hosted deployments
----------------------------------------------------

If you have a :doc:`paid subscription to a Mattermost offering </product-overview/editions-and-offerings>`, such as Mattermost Professional or Mattermost Enterprise, you're entitled to open support tickets via our `online support portal <https://support.mattermost.com/hc/en-us/requests/new>`_. 

When opening a Support ticket as part of your paid subscription, it's important that you provide us with as much information as you can in a timely manner. Knowing what information is relevant can be confusing. We use the anagram C.L.U.E.S. to remember what we need:

* Configurations
* Logs
* Users affected
* Environment
* Steps to reproduce

C.L.U.E.S. represents all of the information that can clarify your issue. With these details, we can begin searching for a cause, whether it's a simple configuration change or a product bug. It also helps us when we need to escalate the issue to our developers so they can spend as much time as possible improving our product.

General guidelines for information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow these guidelines when providing diagnostic data to us:

* Make sure the files you provide are as complete as possible, rather than providing a few lines. Entire log files and configurations provide us with important context.
* Provide configuration and log files in plaintext format if possible, as these are far easier for us to search than screenshots.
* Be sure to sanitize configuration and log files to remove usernames, passwords, and LDAP groups. Replace these details with example strings that contain the same special characters if possible, as special characters are common causes of configuration errors.
* Provide screenshots or screen recordings of unexpected product behavior so that we know exactly what your users are seeing.

Configuration
~~~~~~~~~~~~~

Why we need your configuration data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On Linux systems, settings are generally stored in configuration files. Many issues can be resolved by enabling or disabling a configuration setting. In order to find a resolution, we need to have as complete a picture of your system setup as possible. This also helps us to reproduce bugs so our developers can fix them.

What configuration data includes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration includes (but is not limited to):

- The Mattermost ``config.json`` file.
- The configuration for the reverse proxy, e.g. NGINX, HAProxy, AWS.
- The database configuration.
- SAML configuration when the issue is regarding SAML authentication. The configuration for the Mattermost service is in the SAML IdP.
- Any other systems that Mattermost connects to or systems that exist between the user and the Mattermost server.

How to access your configuration data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Mattermost configuration**

The Mattermost configuration is usually stored at ``/opt/mattermost/config/config.json``. If you've migrated the Mattermost configuration to the database, you can get the configuration using ``mmctl`` or by running this database query:

.. code-block:: SQL
    
  SELECT Value FROM Configurations WHERE Active = 1;

**Reverse Proxy configuration**

NGINX usually splits its configuration into two parts: the main server configuration at ``/etc/nginx/nginx.conf``, and a virtual server configuration. On Ubuntu, this is stored in ``/etc/nginx/sites-available``. Providing both of these configuration files is helpful, but providing the latter is more important.

**SAML configuration**

If the issue you're seeing is with SAML login, we will need to see the full configuration for the Mattermost service in the SAML provider. The configuration for the Mattermost service is in the SAML IdP. Providing screenshots similar to the ones in the setup documentation is sufficient because most SAML providers are configured using a web interface.

**LDAP configuration**

The LDAP administrator should confirm the correct values for the following Mattermost LDAP settings:

- LDAP server hostname.
- LDAP connection port, security, and certificates.
- BaseDN, bind username, and bind password.
- User, Group, Guest, and Admin filters.
- Display attributes.

These can be provided as a text file or as screenshots from the LDAP server.

**Other configurations**

If you're experiencing an issue on mobile, and you're using an MDM or VPN to connect to the server, those configurations will be necessary to diagnose the problem. A system admin for the external system should be able to provide you with the configuration.

Logs
~~~~

Why we need them
^^^^^^^^^^^^^^^^

Nearly all computer systems have logs of errors and application behavior that can show us what's happening when an application is running. Error logs are invaluable when diagnosing a problem, but only if they're as complete as possible.

What logs are available
^^^^^^^^^^^^^^^^^^^^^^^

**Mattermost**

Mattermost has two log files, one for general messages and the other for notification-related messages. These are found at:

* ``/opt/mattermost/logs/mattermost.log``
* ``/opt/mattermost/logs/notification.log``

**Proxy**

The location of these depend on your proxy configuration, but a good place to start looking is in ``/var/log``. Your proxy administrator should be able to help you find the logs.

**Database**

PostgreSQL and MySQL have different logs, and their location varies based on your configuration. If the issue is related to database connectivity, check the database documentation to locate the logs.

**SAML, LDAP, and other systems**

Your organization's system admin should be able to find these for you.

How to access logs
^^^^^^^^^^^^^^^^^^

**Mattermost**

Make sure :ref:`debug logging is enabled <administration-guide/manage/logging:how do i enable debug logging?>` so that we can get the most information from the logs. To do this, go to **System Console > Environment > Logging**, then set both **Console File Level** and **File Log Level** to **DEBUG**. Remember to save your changes.

If the behavior started at a known time or date, use ``journalctl`` to get the logs like this:

.. code-block:: sh

   sudo journalctl -u mattermost --since "2020-08-23 17:15:00" > mattermost_journalctl.log

Replace 2020-08-23 17:15:00 with the date and time (relative to the server) when the behavior started. To get the server time, use the ``date`` command. If the log files generated are too large to send, compress them with this command:

.. code-block:: sh
   
   tar -czf /tmp/mattermost.log.tgz

The compressed logs will be located on the server at ``/tmp/mattermost.log.tgz``.

If the compressed file is still too big, use these commands to split the compressed file into two or more 20MB files:

.. code-block:: sh
   
   mkdir -p /tmp/mattermost-logs
   cd /tmp/mattermost-logs
   tar czf - /opt/mattermost/logs/mattermost.log | split -b 20m - mattermost.log.tgz.

The compressed files will be located on the server at ``/tmp/mattermost-logs`` and be named ``mattermost.log.tgz.aa``, ``mattermost.log.tgz.ab``, and so on. Use a file transfer client that supports SSH/SFTP, such as Cyberduck, to copy these files from the server.

If you are experiencing issues with Elasticsearch, LDAP, or the database, you can enable trace logging in ``config.json`` by setting ``Trace`` to ``true`` under their respective settings. Combining this with ``DEBUG`` level file log output will result in huge log files, so only leave trace logging on long enough to replicate the behavior. The resulting logs will also contain a lot more sensitive data, including user data, so be sure to sanitize it completely before sharing it with us.

**System logs**

The location of log files for other systems varies, but a good way to get the logs for all processes on the Mattermost server is to use ``journalctl`` like this:

.. code-block:: sh
  
   sudo journalctl --since "2020-08-23 17:15:00" > mattermost_journalctl.log

Replace 2020-08-23 17:15:00` with the date and time (relative to the server) when the error occurred. You can use ``--until`` with the same timestamp format to get the logs between two times:

.. code-block:: sh
  
   sudo journalctl --since "2020-08-23 17:15:00" --until "2020-08-23 16:30:00" > mattermost_journalctl.log

Users affected
~~~~~~~~~~~~~~

Why we need it
^^^^^^^^^^^^^^

Mattermost servers are chaotic places. Thousands of posts, websocket actions, and webhook calls happen every second while users can be in dozens of channels across multiple teams. Knowing which users are affected by a problem can help us sift through all this information to find the root cause.

What information to include
^^^^^^^^^^^^^^^^^^^^^^^^^^^

This should be a detailed explanation of anything the end users who are reporting the unexpected behavior have in common. This includes (but is not limited to):

- Team and Channel memberships, including Direct and Group Messages.
- Authentication methods.
- Client operating system and app versions.
- How users connect to the Mattermost server.
- Any other things these users have in common such as when they joined, whether their login information recently changed, or if they are being synchronized via LDAP.

Note for Agents: This information is also required:

- Customer name
- Customer contacts
- Customer license, e.g. Enterprise/Professional
- Customer tier

Environment
~~~~~~~~~~~

Where the Mattermost server sits in your architecture has a lot of impact on potential issues. For example, a misconfigured proxy server can prevent users from connecting even if there's nothing wrong with Mattermost.

What information to include
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Because of this, having a complete picture of the servers and network that the Mattermost server operates in is key to solving problems. This includes (but is not limited to):

- Mattermost version (e.g. 7.3.0, 7.8.3)
- Server OS and version (e.g. RHEL7, Ubuntu 20.04)
- Any orchestration/automation used like Docker or Kubernetes
- Reverse proxy and version (e.g. NGINX 1.16)
- Database type and version (e.g. PostgreSQL 13)
- SAML provider (e.g. Windows Server 2012 Active Directory, Okta, KeyCloak)
- LDAP provider (e.g. Windows Server 2016 Active Directory, Okta, OpenLDAP)
- The type and version of any proxies or VPNs on the network that the Mattermost server is connecting through

Be as specific as possible when describing the environment. If you are seeing errors like **Connection Refused** be sure to include any firewalls or filtering proxies that may be on your network, either inbound or outbound.

**Examples**

Mattermost server

 - External hostname: mattermost.example.com
 - Internal hostname: mattermost.lan
 - Mattermost v7.3.0
 - Zoom plugin v1.4.1
 - NGINX v1.18.0

Database server

 - Internal hostname: postgresql.lan
 - PostgreSQL v13
 - LDAP Provider - 192.168.1.102
 - Internal hostname: ldap.lan
 - OpenLDAP 2.4.54 (Docker container)

Mattermost servers

 - Hostnames: mm1.local.lan, mm2.local.lan, mm3.local.lan, mm4.local.lan

Mattermost server versions

 - mm1-3: 5.25.4
 - mm4: 5.21.0

Proxy server

 - External hostname: mattermost.example.com
 - Internal hostname: proxy.local.lan
 - NGINX v1.16.0

Database servers

 - Hostnames: db1.local.lan, db2.local.lan, db3.local.lan
 - Primary: db1.local.lan
 - Read-Only: db2.local.lan, db3.local.lan  
 - PostgreSQL v13

Elasticsearch server

 - Hostname: elastic.local.lan
 - Elasticsearch v7.9 with these plugins
 - analysis-icu

Steps to reproduce
~~~~~~~~~~~~~~~~~~

What it is
^^^^^^^^^^

If the behavior only happens when the user performs a specific action, providing detailed steps to reproduce it will help us make sure we find and fix the right bug. These details should be as descriptive as possible, but nothing is better than a screenshot or a screen recording of the behavior.

A short summary of the steps to reproduce is also helpful. If you want some examples, look at the bug tickets on some Mattermost Jira tickets.

How to provide these details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**macOS**

Press :kbd:`⌘` :kbd:`⇧` :kbd:`5` to open the screen recording tool and select the region of the screen you want to record. To take a screenshot, press :kbd:`⌘` :kbd:`⇧` :kbd:`4` and select the region to take a screenshot. The screenshot files are placed on the desktop by default.

**Windows**

Press :kbd:`Ctrl` :kbd:`Shift` :kbd:`S` to open the snipping tool to take a screenshot. If you want to take a screen recording you'll need to install third-party software such as `OBS <https://obsproject.com/>`__.

**iOS**

Take a screenshot or screen recording `on iPhone <https://support.apple.com/guide/iphone/take-a-screenshot-iphc872c0115/ios>`_.

**Android**

Take a screenshot or record your screen on your `Android device <https://support.google.com/android/answer/9075928?hl=en>`_.

Appendix
--------

**A note on mobile issues**

Because the mobile app doesn't have a debug mode, diagnosing issues stemming from user data requires a proxy like Charles or mitmproxy. These will intercept and record traffic from the client which can then be replayed to reproduce issues. Talk to a `Mattermost Expert <https://mattermost.com/contact-sales/>`_ for help setting these up.

**SAML login issues**

If the issue is with SAML login, one important piece of context is the SAML login flow. This contains headers and authentication information that can reveal issues that are easy to fix. Follow these instructions to view the SAML login flow if you are experiencing SAML authentication.

Checking keys and certificates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Key and certificate files should never be shared, but if the error indicates a problem with the format of a key or certificate, then you should verify the format of the keys and certificates by running this command:

.. code-block:: sh

  cat -A /path/to/key-or.cert

The output must meet these criteria exactly to be valid:

* Start with ``-----BEGIN CERTIFICATE-----$``.
* All lines must end with ``$``. If they end with ``^M$`` then convert them to UNIX line endings with ``dos2unix``.
* End with ``-----END CERTIFICATE-----$``.
