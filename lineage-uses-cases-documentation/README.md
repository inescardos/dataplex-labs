# Data lineage use cases

This repository contains the code you use to set up the environment required for completing the [data lineage use cases](https://docs.cloud.google.com/dataplex/docs/lineage-use-cases-overview).

## Prerequisites

*   A Google Cloud Project with **Billing** enabled.
*   Google Cloud CLI (`gcloud`, `bq`) [installed and configured](https://docs.cloud.google.com/sdk/docs/install-sdk). Alternatively, you can use Cloud Shell.
*   Sufficient IAM permissions in your project:
     * Data Lineage Viewer
     * BigQuery Data Viewer
     * BigQuery Resource Viewer
     * Dataplex Catalog Viewer 
     * Dataform Editor
       
    For more information about the required roles, see [required roles](https://clouddocs.devsite.corp.google.com/dataplex/docs/lineage-use-cases-prerequisites?db=bczyz-dataplex#required_roles)

*   The following APIs enabled in your project:
    *   BigQuery API
    *   BigQuery Data Transfer API
    *   Data Lineage API
    
    You can enable APIs using the Cloud Console or by running the command: `gcloud services enable <service.googleapis.com>`.

## Steps

To set the pre-built state in your Google Cloud Console for running the lineage use cases you must do the following:

* Create a  GitHub Personal Access Token (PAT) and add it to Secret Manager.
* Connect the Github repository to Dataform. 

### 1. Create a  GitHub Personal Access Token (PAT) and add it to Secret Manager

#### Create a GitHub Token (PAT)

To connect your GitHub repository to Google Cloud, you need a **key/token** from GitHub. Follow this steps to create a key: 

1. In GitHub, click your profile icon and go to **Settings** > **Developer Settings** > **Personal access tokens** > **Tokens (classic)**.
1. Select **Generate new token (classic)** from the menu.
1. In the **Note field**, enter the name `Dataform-Client-Access`.
1. For **Expiration**, select `No expiration` (or a long duration).
1. For **Scopes**, check the box for **repo** to allow Dataform to see your code.
1. Click **Generate token**.
1. Copy the token and save it somewhere safe. Once you leave the page, you won't be able to access the token.

#### Save the token in Secret Manager

Now you need to store the generated key securely in Google Cloud.

1. In the Google Cloud Console, search for **Secret Manager**.
1. In the **Secrets** tab, click **Create Secret**.
1. In the **Name** field, provide the name. For example `github-token`.
1. In the **Secret value** field, paste the token you copied from GitHub.
1. Click **Create secret**.

#### Give Dataform permission to read the secret

By default, Dataform isn't allowed to see your secrets. You have to give it "Permission to peek."

1. In the **Secret Manager**, click the `github-token` secret you just created.
1. Go to the **Permissions** tab.
1. Click **Grant access** to add the Dataform Service Account.
1. In the **Add principals** field, add the service account. It usually looks as follows: `service-[PROJECT_NUMBER]@gcp-sa-dataform.iam.gserviceaccount.com`.
1. In the **Assign roles** section, select the **Secret Manager Secret Accessor** role.
1. Click **Save**.


### 2. Connect Dataform to the Github repository

To have the pre-built state necessary for each use case, perform the following steps:

1. Fork this GitHub repository.
1. In the Google Cloud Console, go to **Dataform** page.
1. Select **Create Repository**
   1. Fill in  **Repository ID** and **Region** fields, and click **Create**.
1. Co back to the main page. Click your repository name.
1. Click **Settings**.
1. Click **Connect with Git** and fill the fields: 
  1. In the **Remote Git repository URL** field, enter your forked repository URL.
  1. In the **Default branch name** field, enter the branch name (most probably main (or master)).
  1. In the **Secret** field, select the `github-token` secret you have already created in the previous step.
  1. Click **Link**.
 
#### Create a workspace

A development workspace allows you to browse and execute code in Dataform

1. In the **Dataform** page, click the name of the linked repository.
1. Go to **Development Workspaces** tab.
1. Click **Create development workspace**.
1. Provide the workspace ID.
1. Click **Create**.

Performing these steps allows you to see the code from GitHub repository in your Dataform repository.

## How to run a specific use case

To run the defined code for a specific use case:

1. Ensure that you have changed `defaultProject` to your <GCP_PROJECT_NAME> in `workflow_settings.yaml`.
1. In **Dataform** page, click your repository name. From there, click the workspace name.
1. In the top menu, select **Start execution** > **Actions** > **Multiple actions**.
1. Click **Selection of tags** and select a tag depending on the use case:
   * For use case **Impact Analysis**, choose the tag `impact-analysis`
   * For use case **Optimize Costs**, choose the tag `optimize-costs`
   * For use case **PII Leakage**, choose the tag `pii-leakage`
1. Click **Start Execution**.

