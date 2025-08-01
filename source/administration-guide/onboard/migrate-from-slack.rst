Migrate from Slack
==================

.. include:: ../../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

.. note::

  `Slack's data control policies may change at any time <https://x.com/Austen/status/1752064934970896626>`_. Also, Slack may choose to change its export capabilities, and/or charge fees to customers for exporting data stored in Slack. Support for negotiating export of customer IP from Slack Enterprise can be requested by contacting a `Mattermost Expert <https://mattermost.com/contact-sales/>`_.

Migrating from Slack to Mattermost involves the following steps:

1. `Prepare your Mattermost server <#prepare-your-mattermost-server>`__
2. `Generate a Slack import <#generate-a-slack-import>`__
3. `Download file attachments and email addresses <#download-file-attachments-and-email-addresses>`__
4. `Convert the Slack import to Mattermost's format <#convert-slack-import-to-mattermost-s-bulk-export-format>`__
5. `Import data into Mattermost <#import-data-into-mattermost>`__

1. Prepare your Mattermost server
---------------------------------

During the import process, we advise to create a new team for importing the Slack workspace data. If merging multiple Slack workspaces into a single team is the desired end-result, we recommend completing the import to separate teams, validating the results, then using :ref:`mmctl <administration-guide/manage/mmctl-command-line-tool:mmctl channel move>` to move channels between teams.

Also, system administrator roles will be overwritten if the usernames match and the user isn't an admin on the Slack workspace.

Make sure you are running on a supported version of :doc:`Mattermost </product-overview/mattermost-server-releases/>` to benefit from the most up-to-date functionality and fixes.

2. Generate a Slack import
--------------------------

Slack offers two ways to `export your data from their product <https://slack.com/help/articles/201658943-Export-your-workspace-data>`_.

1. Regular export - Contains only public channel posts. This does not include private channels, DMs, or group conversations. This can be generated from **Slack > Administration > Workspace settings > Import/Export Data > Export > Start Export**.
2. Corporate export - Contains all posts. This includes public channels, private channels, DMs and group messages. You must `request this export type from Slack directly <https://slack.com/help/articles/1500001548241-Request-to-export-additional-data-from-your-workspace-or-Enterprise-Grid-org>`_.

You will receive a zip file containing the following contents:

- Channels (``channels.json``)
- Users (``users.json``)
- Direct messages (``dms.json``) (Corporate export)
- Private channels (``groups.json``) (Corporate export)
- Group direct messages (``mpims.json``) (Corporate export)
- App activity logs (``integration_logs.json``)
- Folders containing posts for every public channel
- Folders containing posts for every private channel (Corporate exports)

.. note::

  - Refer to the `Slack help article <https://slack.com/help/articles/220556107-How-to-read-Slack-data-exports>`__ for additioanl details on zip file contents.
  - As a proprietary SaaS service, Slack is able to change its export format quickly and without notice. If you encounter issues not mentioned in the following documentation, please let the Mattermost Product Team know by `filing an issue <https://handbook.mattermost.com/contributors/contributors/ways-to-contribute>`__.

3. Download file attachments and email addresses
------------------------------------------------

When you download your Slack export zip file, some data will be missing from the zip file:

- User emails
- Uploaded attachment contents

We need to create a Slack app in order to gather these contents. Follow these steps to create a Slack app:

1. Go to https://api.slack.com/apps.
2. Select **Create New App**.
3. Select **From scratch**.
4. Name the app **Slack Advanced Exporter** and select the workspace. You'll have to do this for every workspace. Then select **Create App**.
5. Select **OAuth & Permissions** on the left-hand side of the screen. Then scroll down to **Scopes**.
6. Under **Bot Token Scopes** type in and select the following scopes:

  - ``users:read``
  - ``users:read.email``

7. Scroll up and select **Install to Workspace**.
8. Grant the app permissions when prompted.
9. Copy the **Bot User OAuth Token** and save it somewhere convenient.

We'll now use **Bot User OAuth Token** with the ``slack-advanced-exporter`` tool to download emails and attachments. Download the latest release of ``slack-advanced-exporter`` for your OS and architecture `here <https://github.com/grundleborg/slack-advanced-exporter/releases/>`__ and extract the executable from the download.

Once you have the program downloaded locally, run the commands below to fetch the emails, and then fetch file attachments. Replace ``<SLACK TOKEN>`` with the Slack token you generated earlier and ``<SLACK EXPORT FILE>`` with the `path <https://www.geeksforgeeks.org/absolute-relative-pathnames-unix/>`__ to your export file.

.. code-block:: sh

  ./slack-advanced-exporter --input-archive <SLACK EXPORT FILE> --output-archive export-with-emails.zip fetch-emails --api-token <SLACK TOKEN>
  ./slack-advanced-exporter --input-archive export-with-emails.zip --output-archive export-with-emails-and-attachments.zip fetch-attachments --api-token <SLACK TOKEN>

.. note::

  - The first command collects all of the user emails and creates the file ``export-with-emails.zip``. The second command fetches attachments and creates the file ``export-with-emails-and-attachments.zip``, which we will use going forward.
  - The second command can take a long time if you have a large number of file uploads. If it's interrupted, delete the file generated (if any), and start again.

The file ``export-with-emails-and-attachments.zip`` now contains all the information necessary to be imported into Mattermost.

It's preferable to fetch e-mails first to avoid copying large attachments around. Make sure you choose different file names at each stage, as the tool does not support in-place modifications.

.. important::

  Avoid unzipping and rezipping the Slack export. Doing so can modify the directory structure of the archive which could cause issues with the import process.

4. Convert Slack import to Mattermost's bulk export format
----------------------------------------------------------

Now that you have a Slack export file with emails and attachments, let's convert this information into Mattermost's bulk import format using the import preparation tool ``mmetl``. 

`Download the latest release of mmetl <https://github.com/mattermost/mmetl/releases/>`__ for your OS and architecture. Run ``mmetl help`` to learn more about using the tool.

Next, run the command below to create a Mattermost bulk import file. Replace ``<TEAM-NAME>`` with the name of your team in Mattermost. Note that the name needs to be one word and lowercase (i.e. if you named your team ``My Team``, ``<TEAM-NAME>`` would be ``my-team``).

.. code-block:: sh

    ./mmetl transform slack --team <TEAM-NAME> --file export-with-emails-and-attachments.zip --output mattermost_import.jsonl

The tool outputs a `.jsonl <https://jsonlines.org/examples>`__ file containing all of your users, channels, and posts. It also creates a ``data`` folder that contains all of your attachments. Users' default authentication method will be configured as email/password. See the `section below <#use-the-imported-team>`__ for instructions on migrating the auth method to another choice.

.. important::

  It doesn't matter what you name the ``.jsonl`` file. You can name it what you want with the `--output` flag as shown above. It just needs to be a ``.jsonl`` file.

5. Import data into Mattermost
------------------------------

You can upload the export through Mattermost's API from the server or from another computer using mmctl commands. The server will save the import in its file store before running the import (e.g. AWS S3), so there will be time spent uploading/downloading the file in this case. 

The migration is idempotent, meaning that you can run multiple imports that contain the same posts, and there won't be duplicated created posts in Mattermost. Each post is imported with the correct user/author and ``created_at`` value from your Slack instance. Threads are kept intact with the import.

Ensure you have the Mattermost command line tool ``mmctl`` installed. This allows you to perform different tasks that communicate to Mattermost's API. You'll also want to :ref:`configure authentication <administration-guide/manage/mmctl-command-line-tool:mmctl auth>` for the tool.

To prepare our files to be uploaded to the server, we need to put both the ``.jsonl`` file and ``data`` folder together into a zip file.

.. code-block:: sh

  zip -r mattermost-bulk-import.zip data mattermost_import.jsonl

Then we can upload the zip file to our Mattermost server:

.. code-block:: sh

  mmctl import upload ./mattermost-bulk-import.zip

Run this command to list the available imports:

.. code-block:: sh

  mmctl import list available

Run this command to process the import. Replace ``<IMPORT FILE NAME>`` with the name you got from the ``mmctl import list available`` command:

.. code-block:: sh

  mmctl import process <IMPORT FILE NAME>

Finally, run this command to view the status of the import process job. If the job status shows as ``pending``, then wait before running the command again. The ``--json`` flag is required to view the possible error message. Replace ``<JOB ID>`` with the id you got from the ``mmctl import list process`` command:

.. code-block:: sh

  mmctl import job show <JOB ID> --json

Debug imports
^^^^^^^^^^^^^

You can use the ``mmctl import job show`` command to view any relevant errors that may have occurred.

.. important::
    Note that if you are part of the user group being imported from Slack, your Mattermost profile must have a matching username and email to the corresponding ``user`` line of the ``jsonl`` file. You can manually edit the file to ensure it matches your Mattermost user.

Additional tools
^^^^^^^^^^^^^^^^

* `mm-emoji <https://github.com/users/maxwellpower/packages/container/package/mm-emoji>`__ - Designed to smoothly transition emojis from a Slack instance to Mattermost.
* `mm-importjs <https://github.com/mickmister/mmimportjs>`__ - Breaks up large import files into smaller ones, as well as automatically remove null characters in post content when importing data to Mattermost.
* `slack-migrate-pinned-posts <https://github.com/svelle/slack-migrate-pinned-posts>`__ - Migrates pinned posts from Slack to Mattermost.

Use the imported team
^^^^^^^^^^^^^^^^^^^^^

* During the import process, the emails and usernames from Slack are used to create new Mattermost accounts. If emails are not present in the Slack export archive, then placeholder values will be generated and the system admin will need to update these manually. We recommend administrators search the final import ``jsonl`` file for ``user`` lines for with ``@example.com`` in the email property to address and resolve the missing information prior to import.
* Slack users can activate their new Mattermost accounts by using Mattermost's **Password Reset** screen with their email addresses from Slack to set new passwords for their Mattermost accounts.
* Once logged in, Mattermost users will have access to previous Slack messages in the public channels imported from Slack.
* Instructions on how to migrate user authenticatation to LDAP or SAML can be found :ref:`here <administration-guide/manage/mmctl-command-line-tool:mmctl user migrate-auth>`.
