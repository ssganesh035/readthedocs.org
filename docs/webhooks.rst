Webhooks
========

Read the Docs uses *webhooks* to detect changes in your documentation and trigger builds automatically. When you update your repository (GitHub, Bitbucket, or GitLab), Read the Docs receives a webhook notification and determines if a build should be triggered for the active project version.

Webhook Integrations
--------------------

You can view and manage webhook integrations in your project's **Admin** dashboard under **Integrations**. Each integration page displays configuration details and logs of HTTP exchanges.

Use the provided URL (Payload URL) when setting up webhooks with your repository provider.

Webhook Creation
----------------

If you connected your Read the Docs account to GitHub, Bitbucket, or GitLab, webhooks are configured automatically. If not, you need to set them up manually:

1. Navigate to **Admin** > **Integrations** > **Add Integration**.
2. Select an integration type and follow the provided setup instructions.
3. The webhook URL follows this pattern: `https://readthedocs.org/api/v2/webhook/<project-name>/<id>/`.
4. Use this URL when setting up a webhook in your repository provider.

.. note::
   If the automatic setup fails, you can still configure the webhook manually.

Webhook Integration Guide
-------------------------

GitHub
~~~~~~

1. Go to **Settings** > **Webhooks** > **Add webhook**.
2. Set **Payload URL** to your integration URL from **Admin** > **Integrations**.
3. Choose **Content type** as `application/json` or `application/x-www-form-urlencoded`.
4. Leave **Secrets** blank.
5. Select **Let me select individual events** and enable:
   - Branch or tag creation
   - Branch or tag deletion
   - Pushes
6. Ensure **Active** is enabled and click **Add webhook**.

Check webhook status under **Recent Deliveries**. A `200 OK` response confirms successful configuration. A `403 Forbidden` usually indicates an incorrect Payload URL.

Bitbucket
~~~~~~~~~

1. Go to **Settings** > **Webhooks** > **Add webhook**.
2. Set **URL** to your integration URL from **Admin** > **Integrations**.
3. Under **Triggers**, ensure **Repository push** is selected.
4. Click **Save**.

GitLab
~~~~~~

1. Navigate to **Settings** > **Integrations**.
2. Set **URL** to your integration URL from **Admin** > **Integrations**.
3. Enable **Push events** and **Tag push events**.
4. Click **Add Webhook**.

Using the Generic API Integration
---------------------------------

If your repository is not hosted on a supported provider, use the generic API endpoint for triggering builds.

- Obtain the webhook URL from **Admin** > **Integrations**.
- Use the integration token for authentication.

Example cURL request:

.. code-block:: shell

    curl -X POST -d "branches=dev" -d "token=1234" https://readthedocs.org/api/v2/webhook/example-project/1/

This can be executed from a cron job or a VCS hook.

Authentication
~~~~~~~~~~~~~~

- If using an integration token, it must match the project.
- If using an authenticated user, they must be a project owner.

Debugging Webhooks
------------------

To debug webhook issues:

- Check logs in **Admin** > **Integrations**.
- Verify the payload received by Read the Docs.
- Ensure the repository provider correctly triggers webhooks.

Resyncing Webhooks
------------------

If a webhook stops working:

1. Go to **Admin** > **Integrations**.
2. Select the integration.
3. Follow the instructions to re-sync the webhook.

Payload Validation
------------------

If your project was imported via a connected account, a secret is created to verify webhook requests. Both GitHub and GitLab support webhook validation:

- [GitHub Webhook Security](https://developer.github.com/webhooks/securing/)
- [GitLab Webhook Security](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#secret-token)

Troubleshooting
---------------

My project isn’t automatically building
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Check webhook logs in **Admin** > **Integrations**.
- Ensure your VCS provider is correctly configured.
- Resync the webhook if needed.

GitHub Services Deprecation
~~~~~~~~~~~~~~~~~~~~~~~~~~~

GitHub deprecated GitHub Services on **January 31, 2019**. If your project relied on this, migrate to a new webhook:

- Use a connected GitHub account with a webhook integration.
- Use a generic webhook integration if not using a connected account.

Legacy Webhooks Deprecation
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Read the Docs deprecated legacy webhook endpoints on **March 1, 2019**. Update your webhooks using the integration setup described above.

Deprecated endpoints:

- `https://readthedocs.org/build`
- `https://readthedocs.org/bitbucket`
- `https://readthedocs.org/github`
- `https://readthedocs.org/gitlab`

Additional Testing Tools
------------------------

For testing and debugging webhooks, you can use:

- [Beeceptor](https://beeceptor.com/): Simulate and inspect incoming webhook requests.
- [Pipedream RequestBin](https://pipedream.com/requestbin): Capture and inspect webhook payloads in real time.
