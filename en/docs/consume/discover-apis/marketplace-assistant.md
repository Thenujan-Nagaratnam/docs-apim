# Marketplace Assistant Getting Started Guide

The Marketplace Assistant is a powerful tool provided by API Manager, utilizing AI to chat with your APIs and offer recommendations, moving beyond traditional keyword searches.

[![Marketplace Assistant Landing Page]({{base_path}}/assets/img/get_started/marketplace-assistant.png)]({{base_path}}/assets/img/get_started/marketplace-assistant.png)

Follow the steps below to get started with the Marketplace Assistant:

!!! tip
    If you've previously registered your environment for the [API Chat]({{base_path}}/consume/invoke-apis/invoke-apis-using-tools/test-apis-with-apichat), you can skip Step 1 by utilizing the same credentials for the Marketplace Assistant. Otherwise, complete Step 1 to register your on-premise environment.

## Step 1 - Sign in to Choreo

1. Navigate to Choreo using the URL: <a href="https://console.choreo.dev">https://console.choreo.dev</a>.

2. Sign-in to Choreo.

   [![Choreo sign-in options]({{base_path}}/assets/img/observe/sign-in-choreo.png)]({{base_path}}/assets/img/observe/sign-in-choreo.png)

## Step 2 - Register your environment

Follow the instructions below to register your on-premise environment:

1. Click the **Settings** on the bottom left corner.

      [![Settings Menu]({{base_path}}/assets/img/observe/settings-menu.png)]({{base_path}}/assets/img/observe/settings-menu.png)

2. If you are a member of multiple organizations, select the appropriate organization from the top left-hand corner.

3. Select the **On-prem Keys** tab and click **Generate Key**.

      [![On-prem Key]({{base_path}}/assets/img/observe/on-prem-key.png)]({{base_path}}/assets/img/observe/on-prem-key.png)

4. Enter a suitable name for your environment (e.g., dev).

5. Click **Generate**.
6. Copy the key that was generated before closing the dialog box.

You can generate and use separate keys for your different environments.

## Step 3 - Configure API Manager

To enable the Marketplace Assistant and populate the vector database, the API Manager requires configuration. Follow these steps:

### Configure the deployment.toml

1. The following configuration change must be done in the `<API-M_HOME>/repository/conf/deployment.toml` file. Update the `[apim.ai]` config by providing the on-premise token obtained from Step 2. Also, be sure to update the endpoint field as below.

      ```toml
      [apim.ai]
      enable = true
      endpoint = "https://dev-tools.wso2.com/apim-ai-service"
      token = "<use token that you generated>"
      ```

2. Restart the API Manager.

## Step 4 (optional) - Sync vector database with your current APIs

To ensure that the Marketplace Assistant is aware of all published APIs and to update the vector database with the current APIs, follow these steps:

1.  First you have to download and intialize the apictl. For more information, see <a href="https://apim.docs.wso2.com/en/latest/install-and-setup/setup/api-controller/getting-started-with-wso2-api-controller/#download-and-initialize-the-apictl">Download and initialize the apictl</a>.

2.  Run the following apictl commands to update the vector Database with the current APIs.

    1.  Set token as a config variable

         - **Command**

               ```bash
               apictl set --ai-token "<use token that you generated>"
               ```

    2.  Delete APIs and API Products from vector database.

        - **Command**

               ```bash
               apictl ai delete artifacts -e "<environment>"
               ```

               ```bash
               apictl ai delete artifacts --token "<use token that you generated>" -e "<environment>"
               ```

               ```bash
               apictl ai delete artifacts --token "<use token that you generated>" --endpoint "<endpoint of ai service>" -e "<environment>"
               ```

            !!! info
                **Flags:**  
                
                  -  Required :  
                     `--environment` or `-e` : Environment to be searched
                  -  Optional :  
                     `--token` : On prem key of AI services  
                     `--endpoint` : Endpoint url of AI services

            !!! example
                  ```bash
                     apictl ai delete artifacts -e dev
                  ```

                  ```bash
                     apictl ai delete artifacts --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts -e dev
                  ```

                  ```bash
                     apictl ai delete artifacts --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts --endpoint https://dev-tools.wso2.com/apim-ai-service -e dev
                  ```
            !!! note
                  - Note that if you have already set the token to the config variable, you dont have to use the --token flag


    3.  Upload APIs to vector database.

         - **Command**

            ```bash
            apictl ai upload apis -e "<environment>"
            ```

            ```bash
            apictl ai upload apis -e "<environment>" --all
            ```

            ```bash
            apictl ai upload apis --token "<use token that you generated>" --endpoint "<endpoint of ai service>" -e "<environment>"
            ```

            !!! info
                **Flags:**

                  -   Required :  
                     `--environment` or `-e` : Environment to be searched
                  -   Optional :  
                     `--token` : On prem key of AI services  
                     `--endpoint` : Endpoint url of AI services  
                     `--all` : Upload both APIs and API Products.

            !!! example
                  ```bash
                  apictl ai upload apis -e dev
                  ```

                  ```bash
                  apictl ai upload apis -e dev --all
                  ```

                  ```bash
                  apictl ai upload apis --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts -e dev
                  ```

                  ```bash
                  apictl ai upload apis --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts --endpoint https://dev-tools.wso2.com/apim-ai-service -e dev
                  ```
            !!! note
                  - Note that if you have already set the token to the config variable, you dont have to use the --token flag

    4.  Upload API Products to vector database.

         - **Command**

            ```bash
            apictl ai upload api-products -e "<environment>"
            ```

            ```bash
            apictl ai upload api-products -e "<environment>" --all
            ```

            ```bash
            apictl ai upload api-products --token "<use token that you generated>" --endpoint "<endpoint of ai service>" -e "<environment>"
            ```

            !!! info
                **Flags:**

                  -   Required :  
                     `--environment` or `-e` : Environment to be searched
                  -   Optional :  
                     `--token` : On prem key of AI services  
                     `--endpoint` : Endpoint url of AI services  
                     `--all` : Upload both APIs and API Products.  

            !!! example
                  ```bash
                  apictl ai upload api-products -e dev
                  ```

                  ```bash
                  apictl ai upload api-products -e dev --all
                  ```

                  ```bash
                  apictl ai upload api-products --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts -e dev
                  ```

                  ```bash
                  apictl ai upload api-products --token 2fdca1b6-6a28-4aea-add6-77c97033bdb9 artifacts --endpoint https://dev-tools.wso2.com/apim-ai-service -e dev
                  ```
            !!! note
                  - Note that if you have already set the token to the config variable, you dont have to use the --token flag

These instructions involve removing any existing APIs and API Products from the vector database, then uploading all the public APIs and API Products from the currently logged-in user's tenant within a specified environment. Similarly, this process can be repeated for all tenants.

If you intend to use the same access token across different deployments, you can continue using it without generating a new one each time.

This process ensures that the Marketplace Assistant is up-to-date with all published APIs, enhancing its ability to provide accurate and relevant assistance.

## Step 5 - Engage with the Marketplace Assistant

Now that your environment is configured, you're ready to interact with the Marketplace Assistant. Utilize its capabilities to chat with your APIs and receive tailored recommendations.

## What's Next?

Explore the features and functionalities of the Marketplace Assistant to streamline your API management experience further.

### This error message is prompted in the terminal when I use the apictl to upload my APIs: `context deadline exceeded (Client.Timeout exceeded while awaiting headers)`.

The dafault HTTP request timeout for apictl is 10s. Due to network connnectivity or some other issues, the uploading might exceed this limit. If you encounter this issue you can simply set the timeout to a higher value.

you can modify the `main_config.yaml` file, which you can find in the `APICTL_CONFIG_DIR` and increase the `http_request_timeout` value (Note that the value should be given in milli seconds, e.g: `http_request_timeout: 20000`).
