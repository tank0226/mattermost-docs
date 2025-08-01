Manage user surveys
===================

.. include:: ../../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

In your self-hosted Mattermost deployment, use the Mattermost User Survey integration to gather direct feedback from your Mattermost users to identify what's working well and what's not with your Mattermost instance. All user responses are stored in and remain within your self-hosted deployment, and no telemetry data is sent back to Mattermost. You can export a CSV report of NPS scores and user responses for further analysis, or to share your feedback data with Mattermost.

You can schedule when each survey begins, define how long each survey lasts, specify teams excluded from the survey, as well as customize both a welcome message and a user question you want feedback on.

Setup
------

You must be a Mattermost system admin to `upload the plugin <#upload>`__ to your Mattermost self-hosted deployment `enable it <#enable>`__,  `create surveys <#create-surveys>`__, and `export survey responses <#export-survey-responses>`__ using the System Console.

The User Survey integration is compatible with the following Mattermost Server versions:

  - v9.11.2 (Extended Support Release)
  - v9.8 or later
  - v9.5.2+ (Extended Support Release)
  - v8.1 (Extended Support Release)

Install
--------

.. important::

  Mattermost offers an additonal :doc:`User Satisfaction Surveys </administration-guide/manage/user-satisfaction-surveys>` option with limited customization options within the **Plugins** list where surveys are enabled by default. We recommend :ref:`disabling the User Satisfaction Surveys functionality <administration-guide/manage/user-satisfaction-surveys:how can surveys be disabled?>` when using this user survey integration.

1. Log in to your Mattermost :doc:`workspace </end-user-guide/end-user-guide-index>` as a system administrator.
2. In Mattermost, from the Product menu |product-list|, select **App Marketplace**.
3. Search for or scroll to User Survey, and select **Install**.
4. Once installed, select **Configure**. You're taken to the System Console, directly to the **User Survey** integration page, under **Plugins**.

.. note::

  From Mattermost v9.11.2 (ESR) and Mattermost Cloud v10, this plugin is pre-packaged with the Mattermost Server. If your Mattermost deployment is on a release prior to v9.11.2, download the `latest plugin binary release <https://github.com/mattermost/mattermost-plugin-user-survey/releases>`_, and upload it to your server via **System Console > Plugin Management**.

Upgrade
~~~~~~~

We recommend updating this integration as new versions are released. Generally, updates are seamless and don't interrupt the user experience in Mattermost.

Visit the `Releases page <https://github.com/mattermost/mattermost-plugin-user-survey/releases>`_ for the latest release, available releases, and compatibiilty considerations.

Enable
------

Go to **System Console > Plugins > User Survey** to enable this integration.

Once the integration is installed and enabled, create surveys by completing configuration in the System Console, as described below.

Create surveys
~~~~~~~~~~~~~~~

Under **Survey setup**, specify the date, time, and details for a new survey:

1. **Send next survey at**: Specify the date and time when the survey will begin rolling out to users.

  .. note::

    A single survey can be active at a time. If you already have an active survey running, you'll need to reschedule your new survey to start on a date after the current survey expires. Alternatively, you can end the active survey early by selecting **End survey**.

2. **Survey expiry (days)**: Specify how long the survey will be open to responses in days.
3. **Exclude specific teams**: Specify who will receive the survey. You can send the survey to all users, to all users on specific teams, or omit all users from specific teams.

.. image:: ../../images/survey-schedule.png
  :alt: An example of the System Console configuration screen for scheduling a new user survey.

4. **Survey message text**: Customize the introductory message text users see when prompted to complete the survey.
5. **Linear scale question (1-10)**: This question is required that helps calculate the NPS score.
6. **Textual question**: This question is required that helps gather user feedback.
7. **Textual question (Optional)**: You can optionally specify a text-based question for your survey.
8. Select **Save**. Your new survey is scheduled, and displays under **Active and past surveys** once the survey is shared with users.

.. image:: ../../images/survey-contents.png
  :alt: An example of the System Console configuration screen for defining the contents of a new user survey.

Active surveys
--------------

When a survey starts, all users recieve a direct message from a feedback bot containing the active survey. The survey includes one mandatory scale-based question, and optional text-based questions. 

.. image:: ../../images/user-feedback.png
  :alt: An example of the user survey provided to users.

Users must answer all required questions in order to submit their response. When a user selects **Submit**, their responses are recorded.

Export survey responses
-----------------------

Select **Export responses** to download a CSV file containing NPS scores and user responses gatherered through the survey. You can export data from active surveys which will contain data collected so far.

.. image:: ../../images/active-past-surveys.png
  :alt: An example of the System Console configuration page where all active and past surveys are available for export.