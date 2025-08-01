Work with runs
==============

.. include:: ../../_static/badges/ent-cloud-selfhosted.rst
  :start-after: :nosearch:

A run is the execution of a collaborative playbook. You can start each run in a new channel or you can elect to use the same channel for multiple runs.

To access runs, select the product menu in the top-left corner of Mattermost, then select **Playbooks**. In the runs list, you can select a run to view more details, such as the overview and retrospective. This is an easy way to assess all the active runs to which you have access.

Follow and participate
----------------------

You don't have to be in a run's channel to follow the run. You can:

- **Follow**: If you're following a playbook, you won't necessarily be added to each of the playbook's runs but you will be added as a follower. To join a run channel, select **Participate** in the header of that run.
- **Participate**: If you're participating in a run it's likely because you're in a team or group of people who've been added to it. In this case, you'll be able to follow the run in the run channel and also view the details in the list of runs in the **Playbooks** tab.

View run details
----------------

When you’re in a channel with an active run, select the **Toggle Run Details** icon in the channel header to open the right-hand pane to view the run details. Information such as run name and description can be edited in-line, and the checklists can be collapsed and filtered based on their status.

Some run actions can be edited while the run is in progress. This increases visibility into the run's progress and can improve accountability.

Runs and channel behavior
-------------------------

When you configure your playbook, you can decide whether each run of that playbook starts in a new channel or uses the same channel. You can run multiple playbooks in the same channel, simultaneously. Each playbook in use is listed in the RHS of the run channel.

If you decide to run a playbook in a new channel, you can do this when you start the run. In the channel RHS, select **Start run**. Then select how you'd like it to be executed.

.. tip::

  - When deciding whether to reuse a channel for multiple runs, or create new channels for each playbook run, multiple runs in a single channel can help avoid too many channels being created, which can lead to channel overload. 
  - Playbook run channels aren't automatically archived when runs are marked as complete; however, you can :ref:`archive channels <end-user-guide/collaborate/archive-unarchive-channels:archive a channel>` you no longer need, and system admins can :ref:`allow user access to archived channels <administration-guide/configure/site-configuration-settings:allow users to view archived channels>` if needed. See the :ref:`multiple runs in a channel <end-user-guide/workflow-automation/work-with-playbooks:multiple runs in a channel>` documentation for additional considerations.
  - In contrast, using a dedicated channel for each playbook run can be helpful particularly in cases where strict :doc:`compliance </administration-guide/comply/compliance-export>` and :doc:`channel data export </administration-guide/comply/export-mattermost-channel-data>` is required.

Send outgoing webhooks
----------------------

1. In your run, select **Toggle Run Details** to open the right-hand sidebar.
2. Select **Run details**.
3. In the **Run details** page, scroll down to **Actions**.
4. Add your webhook URLs in the field provided. You can turn off this option using the toggle.
5. Select **Save**.