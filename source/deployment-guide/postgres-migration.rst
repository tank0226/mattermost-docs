Migration guidelines from MySQL to PostgreSQL
=============================================

.. include:: ../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

From Mattermost v8.0, :ref:`PostgreSQL <deployment-guide/software-hardware-requirements:database software>` is our database of choice for Mattermost to enhance the platform’s performance and capabilities. Recognizing the importance of supporting the community members who are interested in migrating from a MySQL database, we have taken proactive measures to provide guidance and best practices.

.. toctree::
   :maxdepth: 1
   :hidden:
   :titlesonly:

   Automate PostgreSQL migration </deployment-guide/postgres-migration-assist-tool>
   Manually migrate to PostgreSQL </deployment-guide/manual-postgres-migration>

* :doc:`Automated migration from MySQL to PostgreSQL </deployment-guide/postgres-migration-assist-tool>` - A comprehensive set of guidelines and a ``migration-assist`` tool to streamline the migration process, alleviate potential challenges, and faciliate a smooth transition.
* :doc:`Manually migrate from MySQL to PostgreSQL </deployment-guide/manual-postgres-migration>` - A good option if your organization has database administrators to own the migration process, or if you want to learn what the ``migration-assist`` tool automates for you.

Frequently asked questions
------------------------------

Why is Mattermost dropping support for MySQL?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mattermost has decided to drop support for MySQL databases to streamline development and focus on delivering more efficient and optimized features. By supporting only PostgreSQL, which is generally considered more advanced and better-suited for enterprise environments, Mattermost can reduce complexity, improve performance, and better allocate resources to enhance the product. This change helps ensure more consistent, robust, and scalable database interactions for all Mattermost deployments.

Is migrating to PostgreSQL before upgrading Mattermost recommended?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes. We also recommend upgrading to Mattermost server v8.x or later before migrating to PostgreSQL.

Can the migration-assist be run on the Mattermost server?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Technically, yes. The ``migration-assist`` tool can be run on the Mattermost server. However, it is recommended to run the tool on a separate server to avoid any performance issues. We advise running the migration against a copy of the MySQL database to ensure that the migration process does not impact the production environment.

How large should the PostgreSQL server be?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The size of the PostgreSQL server should match that of the MySQL server initially. We recommend monitoring the performance of the PostgreSQL server and adjusting the resources as needed.

How large should the server running ``migration-assist`` server be?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The tool itself is lightweight and does not require a large server. A server with 2 CPU cores and 16 GB of RAM should be sufficient for default configurations. However, you may need to adjust the resources based on the size of the MySQL database, your downtime limitations, and the configuration of ``pgloader``.

Do we/will Mattermost bundle pgloader or is that a separate install?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mattermost doesn't bundle pgloader with the Mattermost server. You will need to install pgloader separately. For more information, see the :ref:`install pgloader <deployment-guide/manual-postgres-migration:install pgloader>` documentation.

Are there any other migrations available for plugins, or just Boards, Playbooks, and Calls?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We're developing migrations for other plugins, such as NPS-plugin. Please stay tuned for updates.

Do these processes support AWS RDS databases?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes, the processes support AWS RDS databases. However, you may need to adjust the security group settings to allow the migration process to access the databases.

Does Mattermost support MariaDB? If not, why not?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mattermost does not support MariaDB. The primary reason for this decision is to streamline development and maintenance by focusing on a single database technology. By standardizing on PostgreSQL, Mattermost can deliver optimized features, improved performance, and a more focused engineering effort. PostgreSQL is well-regarded for its robustness, advanced features, and suitability for enterprise use, making it the chosen database for Mattermost. 

MariaDB, while compatible with MySQL to a large extent, introduces additional complexity and potential inconsistency, which the Mattermost development team aims to avoid by limiting database support.

Because of these reasons we also don't support migrations directly from MariaDB to PostgreSQL. 
Oracle provides documentation on [how to migrate from MariaDB to MySQL](https://blogs.oracle.com/mysql/post/how-to-migrate-from-mariadb-to-mysql-80) which can be used as a starting point for the migration to PostgreSQL.

Troubleshooting
---------------

Unsupported authentication for MySQL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are facing an error due to authentication with MySQL v8, it may be related to a `known issue <https://github.com/dimitri/pgloader/issues/782>`__ with pgloader. The fix is to set the default authentication method to ``mysql_native_password`` in your MySQL configuration. To do so, add the ``default-authentication-plugin=mysql_native_password`` value to your ``mysql.cnf`` file. Also, do not forget to update your user to use this authentication method.

.. code-block:: sql

  ALTER USER '<mysql_user>'@'%' IDENTIFIED WITH mysql_native_password BY '<mysql_password>';

Errors during the pgloader command execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you encounter errors during the execution of the ``pgloader`` command, ensure that both of the databases are accessible and that the users have the necessary permissions to access the database. Do not continue with the migration if there are errors during the execution of the ``pgloader`` command.

.. note::

  For experienced users, it is recoverable to run the ``pgloader`` without requiring a restart of the migration from scratch. In this case, you will need to manually fix the issues with the table, and run the ``pgloader`` command with a tailored configuration specifically for those tables. Also ensure that the schema name is reverted back to ``public``, and the ``search_path`` is restored (or remove necessary clauses from the configuration).

The following sections detail how to resolve some common errors you may encounter during the execution of the ``pgloader`` command:

Invalid input syntax for type JSON
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you receive an error message similar to the following:

.. code-block:: text

  ERROR Database error 22P02: invalid input syntax for type json

The data in the MySQL database is not in a valid JSON format. You can fix this issue by updating the data in the MySQL database to be in a valid JSON format. To find out which row is causing the issue, run the following query (where ``<table_name>`` and ``<column_name>`` should be replaced with the actual table and column names indicated in the ``pgloader`` output):

.. code-block:: sql

  SELECT * FROM <table_name> WHERE JSON_VALID(<column_name>) = 0;

You can find and update the data in the MySQL database to be in a valid JSON format with the query above. After updating the data, you can run the ``pgloader`` command again.

Failed to find column or table
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you receive an error message similar to the following:

.. code-block:: text

   pgloader failed to find column

The column or table is missing in the PostgreSQL database. You can fix this issue by checking whether you have created the correct version of Postgres schema. After re-creating the schema, you can run the ``pgloader`` command again.

Fell through ECASE expression
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you receive an error message similar to the following:

.. code-block:: text

   ERROR mysql: 76 fell through ECASE expression.

It is a `known issue <https://github.com/dimitri/pgloader/issues/1183>`__ with pgloader. You can fix this issue by either compiling ``pgloader`` from source or simply avoid this by running ``pgloader`` with our docker image. See: :ref:`install pgloader <deployment-guide/manual-postgres-migration:install pgloader>` for more information.  

.. note::

  Also, there may be cases where pgloader continues to migrate remaining tables and skip one or more tables during migration. In such cases, we recommend identifying issues with the table and fixing them before running the ``pgloader`` command again with a clean database. It is possible to run the ``pgloader`` command with the ``--debug`` flag to get more information about the errors.


Mattermost can't connect to the PostgreSQL database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are facing an issue where Mattermost can't connect to the PostgreSQL database, ensure that the PostgreSQL server is running and that the database is accessible. If there were errors during the execution of the ``pgloader`` command, it can fail to revert the schema name back to ``public``, or potentially fail to restore the ``search_path``. You can manually revert the schema name back to ``public`` and restore the ``search_path`` by running the following commands:

1. Connect to PostgreSQL using ``sudo -u postgres psql``.
2. Select the ``mattermost`` database using ``\c mattermost``. Verify you are using the right database by running ``SELECT current_database();``. The command should output ``mattermost``.
3. Revert the schema name change (optional)

.. code-block:: sql

  ALTER SCHEMA <schema_name> RENAME TO public;

Also ensure that the database user has the necessary settings to have default access to the ``public`` schema. You can do this by running the following commands:

4. Set the search_path for the mmuser:

   .. code-block:: sql

      ALTER USER mmuser SET search_path TO "$user", public;


5. Terminate the connection and connect again to your psql server.

6. Verify the search_path is set correctly:

   .. code-block:: sh

      SHOW search_path;

   This should return ``"$user", public``.

7. If the issue persists, also run:

   .. code-block:: sql

      SELECT pg_catalog.set_config('search_path', '"$user", public', false);

Permission issues when accessing the schema in PostgreSQL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you run into a permission issue during step 2 where the command reports a permission issue similar to the following:

.. code-block:: text

   An Error Occurred: could not check schema owner: the user “mmuser” is not owner of the “public” schema

Ensure that the ``mmuser`` user in PostgreSQL is the owner of the schema.

1. Connect to PostgreSQL using ``sudo -u postgres psql``.
2. Select the ``mattermost`` database using ``\c mattermost``. Verify you are using the right database by running ``SELECT current_database();``. The command should output ``mattermost``.
3. Run the following commands:

   .. code-block:: sql

      ALTER SCHEMA public OWNER TO mmuser;
      GRANT USAGE, CREATE ON SCHEMA public TO mmuser;

Then, re-run the command from step 2.

Incoming webhook channel names become case-sensitive after migration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After migrating from MySQL to PostgreSQL, incoming webhooks that previously worked with mixed-case channel names (e.g., ``"Tech-Talk"``) will fail with the following error:

.. code-block:: text

  GetChannelByName: Channel does not exist.

This occurs because Mattermost doesn't allow the creation of channels with uppercase characters. All new channels names must only contain lowercase characters.
Additionally, MySQL is case-insensitive when matching channel names, while PostgreSQL is case-sensitive. This means that webhook payloads that worked with MySQL's forgiving case-insensitive matching will fail with PostgreSQL's strict case-sensitive matching.
In rare cases, older channels may still exist with mixed-case names in your database. These channels are considered invalid and may cause issues when performing operations like updating channel headers.

**Solution:**

Update all webhook payloads to use only lowercase channel names.

**Prevention:**

Before migrating to PostgreSQL:

1. Audit your incoming webhooks and update any that use mixed-case channel names in their payloads.
2. Check for any legacy channels with mixed-case names that may have been created in earlier versions.
3. Consider renaming any mixed-case channels to lowercase to prevent issues.

This is expected behavior with PostgreSQL and reflects Mattermost's channel naming requirements.

Legacy Boards tables causing migration errors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

During database migration from MySQL to PostgreSQL, while using schema comparison tools (such as dbcmp) you may encounter the following error:

.. code-block:: text

  error during comparison: no pk defined for table: focalboard_file_info

This typically relates to legacy tables from older versions of Boards that are no longer in use. If you're using Boards version 8.0.0 or later, these tables are no longer required and can be ignored in the migration process.

**Applies To:**

* Boards version 8.0.0 and above
* All staging, UAT, QA, or production environments

.. note::
  Does not apply to standalone Boards versions prior to v8.0.0

**Solution:**

If you're using Boards version 8.0.0 or higher, the following legacy tables which were used only in the standalone version and can be safely ignored:

* ``focalboard_file_info`` - Used in standalone version; no longer relevant in plugin and later versions. Stores uploaded files and attachments for boards.
* ``focalboard_sessions`` - Used in standalone version. Tracks user login sessions for boards.
* ``focalboard_teams`` - Used in standalone version. Legacy team mapping not used in later versions. Stores team board settings and permissions.
* ``focalboard_users`` - Used in standalone version. Stores user preferences and settings for boards.

These tables may be empty or unused in your current environment. If so, they do not need to be migrated to PostgreSQL.


Contact Support
---------------

Mattermost customers looking for guidance tailored to their Mattermost deployment can contact a `Mattermost Expert <https://mattermost.com/contact-sales/>`_ for guidance.
