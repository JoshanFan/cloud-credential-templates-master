# How-to create Service Accounts in GCP to provision F5XC GCP VPC site

## Prerequisites

**gcloud** - Google Cloud command-line SDK. See [Installing Google Cloud SDK](https://cloud.google.com/sdk/docs/install) for more information.

## Steps

1. Create the GCP role and permissions required to create F5XC GCP VPC site.

    ```sh
    gcloud iam roles create <ROLE_ID> --project=<GCP_PROJECT_ID> --file=f5xc_gcp_vpc_role.yaml
    ```

    * `ROLE_ID` - The id of the custom role to create. For example: `f5xc_gcp_vpc_role`.
    * `GCP_PROJECT_ID` - The project of the role you want to create.

2. Create the GCP service account.

    ```sh
    gcloud iam service-accounts create <SERVICE_ACCOUNT_NAME>  --display-name=<SERVICE_ACCOUNT_NAME>
    ```

    * `SERVICE_ACCOUNT_NAME` - The internal name of the new service account. For example: `f5xc-gcp-vpc-service-account`.

3. Get the IAM internal email address for the above-created service account.

    ```sh
    gcloud iam service-accounts list | grep <SERVICE_ACCOUNT_NAME> | awk '{print $2}'
    ```

    * `SERVICE_ACCOUNT_NAME` - the service account name used in the previous step.

4. Attach the role created on Step 1 to the IAM service account email address received from Step 3.

    ```sh
    gcloud projects add-iam-policy-binding <PROJECT_ID> --member='serviceAccount:<SERVICE_ACCOUNT_IAM_EMAIL_ADDRESS>' --role=projects/<PROJECT_ID>/roles/<ROLE_ID>
    ```

    * `SERVICE_ACCOUNT_IAM_EMAIL_ADDRESS` - the output of Step 3.
    * `PROJECT_ID` - the project ID.
    * `ROLE_ID` - the Role ID used in Step 1.

5. Create the service account key.

    ```sh
    gcloud iam service-accounts keys create --iam-account <SERVICE_ACCOUNT_IAM_EMAIL_ADDRESS> key.json
    ```

    * `SERVICE_ACCOUNT_IAM_EMAIL_ADDRESS` - the output of Step 3
    * `key.json` is the output of the above command and will be used to create `GCP Cloud Credentials` on `F5XC Console`.
