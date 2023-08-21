# How-to create Service Principal in Azure to provision F5XC Azure VNET site

## Prerequisities

**az** - The Azure CLI. See [Install the Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) for more information.

## Steps

1. Run the Azure login command:

    ```sh
    az login
    ```

    The CLI opens your default browser and load an Azure sign-in page. Sign-in with your account credentials.

2. List the Azure accounts and subscriptions:

    ```sh
    az account list --output table
    ```

    Get the desired Azure `SubscriptionId` from the output of above command.

3. Set the active account to the desired `SubscriptionId`:

    ```sh
    export SUBSCRIPTION_ID=<subscription_id>
    az account set --subscription $SUBSCRIPTION_ID
    ```

4. Create the `Azure Custom Role` using the JSON file - f5xc-azure-custom-role.json:

    ```sh
    az role definition create --role-definition ./f5xc-azure-custom-role.json
    ```

    `NOTE`
    * Replace the value of $SUBSCRIPTION_ID to the relevant `SubscriptionId` in the JSON file.
    * Modify `Name` of the role in the JSON file if required (default custom role name is `f5xc-azure-role`).

5. Create the `Service Principal` and assign the custom role created on Step 4:

    ```sh
    az ad sp create-for-rbac --role="f5xc-azure-role" --scopes="/subscriptions/$SUBSCRIPTION_ID" --name "SP_NAME"
    ```

    * `f5xc-azure-role` is the Custom Role created on Step 4.
    * `SP_NAME` is the service principal name that will be created.
    * The resulting JSON ouput will be used to create `Azure Client Secret for Service Principal` on `F5XC Console`.
