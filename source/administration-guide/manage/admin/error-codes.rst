Mattermost error codes
======================

.. include:: ../../../_static/badges/allplans-selfhosted.rst
  :start-after: :nosearch:

Mattermost is designed to deploy in private networks which may be disconnected or “air-gapped” from the internet. In these deployments, links to Mattermost’s online documentation may be unavailable. 

Therefore, Mattermost error codes are provided to enable administrators to use internet connected machines to look up error messages and resolutions in online resources, including Mattermost documentation and user forums, as well as providing Mattermost support teams a concise way to identify issues.

Mattermost error codes are used in logs, administrative tools, as well as on-screen messages to concisely identify issues and connect them with additional resources. 

In advanced deployments, error codes can be overwritten by administrators to reference internal documentation and guides.

ERROR_SAFETY_LIMITS_EXCEEDED
----------------------------

A safety limits exceeded error (``ERROR_SAFETY_LIMITS_EXCEEDED``) displays in the :ref:`free version of Mattermost <product-overview/editions-and-offerings:mattermost team edition>`, and certain functionality may be limited, when usage grossly exceeds the recommended limit for users in a safe deployment, including more than 2,500 users are registered on the server.

2,500 users represents a “high upper limit” for deployments that are approximately 100 times the recommended size, which is far beyond the intended design of the product. 

The free version of Mattermost is intended for approximately 50 users. If your Mattermost materially exceeds this recommended size, system admins should seek to either `purchase a commercial license <https://mattermost.com/pricing/>`_, or apply for a :doc:`non-profit subscription </product-overview/non-profit-subscriptions>` license. Alternatively, admins can :ref:`deactivate users <administration-guide/configure/user-management-configuration-settings:deactivate users>` until the user count falls below the high upper limit.

ERROR_LICENSED_USERS_LIMIT_EXCEEDED
-----------------------------------

A licensed user limit exceeded error (``ERROR_LICENSED_USERS_LIMIT_EXCEEDED``) displays when attempting to create or activate users on a licensed Mattermost server where the user count would exceed the maximum number of users allowed by the license.

This error occurs when:

- Creating new users would exceed the license user limit.
- Activating deactivated users would exceed the license user limit.

To resolve this error, system administrators can:

- :ref:`Deactivate users <administration-guide/configure/user-management-configuration-settings:deactivate users>` to reduce the active user count below the license limit.
- Contact `Mattermost Sales <https://mattermost.com/contact-sales/>`_ to discuss license options.
