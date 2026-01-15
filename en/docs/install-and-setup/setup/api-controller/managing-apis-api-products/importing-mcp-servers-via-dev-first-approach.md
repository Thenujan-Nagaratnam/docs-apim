# Importing MCP Servers Via Dev First Approach

**WSO2 API Controller (apictl)** allows you to create and deploy MCP (Model Context Protocol) Servers without using the Publisher Portal of the WSO2 API Manager (WSO2 API-M). You can use this feature to create an MCP Server **from scratch** or **using an existing MCP specification** and then deploy it to the desired WSO2 API-M environment.

!!! info
    **Before you begin** 

    -   Make sure that the apictl is downloaded and initialized, if not, follow the steps in [Download and Initialize the apictl]({{base_path}}/install-and-setup/setup/api-controller/getting-started-with-wso2-api-controller/#download-and-initialize-the-apictl).

    -   Make sure you already have added an environment using the apictl for the WSO2 API-M environment you plan to import the MCP Server to. 

        If not, follow the steps in [Add an Environment]({{base_path}}/install-and-setup/setup//api-controller/getting-started-with-wso2-api-controller#add-an-environment).

## Initialize an MCP Server project

1. Open a terminal window and navigate to the path you need to create the project.

2. You can follow either of the following ways to generate the project.

    1.   **From Scratch**
        -   Command
            ```bash
            apictl init mcp-server <Project Path>   
            ```
            ```bash
            apictl init mcp-server <Project Path> --definition <MCP Server definition template file>  --force=<force create project>
            ```

            !!! example
                ```bash
                apictl init mcp-server SampleMCPServer                
                ```
                ```bash 
                apictl init mcp-server SampleMCPServer --definition definition.yaml --force=true                
                ```
            
            As an example, you can use the [Sample-MCP-Server.yaml](https://github.com/wso2/product-apim-tooling/blob/4.1.x/import-export-cli/integration/testdata/sample-mcp-server.yaml) here to generate an MCP Server Project.

        -   Response    
            ```go
            Initializing a new WSO2 API Manager MCP Server project in /home/user/work/SampleMCPServer
            Project initialized
            Open README file to learn more
            ```
        
            !!! info
                In this case, the project artifacts are generated according to a set of predefined templates. Therefore, after initializing the project, you need to go and edit the project artifacts manually.     


    2.   **From MCP Specification**  
        You can use an MCP specification to generate an MCP Server. The file format should be YAML or JSON.

        -   Command
            ```bash
            apictl init mcp-server <Project Path> --spec <Path to MCP specification>
            ```
            ```bash
            apictl init mcp-server <Project Path> --spec <Path to MCP specification> --definition <MCP Server definition template file> --force=<force create project>
            ```

            !!! example
                ```bash
                apictl init mcp-server ChatGPTServer --spec chatgpt-mcp.yaml
                ```
                ```bash
                apictl init mcp-server ChatGPTServer --spec https://example.com/chatgpt-mcp.json
                ```
                ```go
                apictl init mcp-server ChatGPTServer --spec chatgpt-mcp.yaml --definition definition.yaml --force=true
                ```

        -   Response

            ```go
            Initializing a new WSO2 API Manager MCP Server project in /home/user/work/ChatGPTServer
            Project initialized
            Open README file to learn more
            ```

            !!! info
                When you initialize an MCP Server project using an MCP specification, apictl will automatically read the MCP definition and populate a certain set of configs in the MCP Server definition file, `mcp-server.yaml`.     

            !!! info
                **Flags:**  
                    
                -   Optional :  
                        `--definition` or `-d` : Provide a YAML definition of MCP Server  
                        `--spec` : Provide an MCP specification file/URL for the MCP Server   
                        `--force` or `-f` : To overwrite the directory and create the project 

        !!! note
            You can define scopes for a resource when defining an MCP specification to generate an MCP Server.

            !!! note
                The following example is a template file. Please do the necessary changes to the template file before using this example to generate an MCP Server.

            !!! example
                ```yaml
                mcp:
                  version: "2024-11-05"
                  implementation:
                    name: "ChatGPT MCP Server"
                    version: "1.0.0"
                  capabilities:
                    tools: {}
                    prompts: {}
                    resources: {}
                  server:
                    name: ChatGPTServer
                    description: MCP Server for ChatGPT integration
                    version: "1.0.0"
                  prompts:
                    - name: "code-review"
                      description: "Review code for best practices"
                      arguments:
                        - name: "code"
                          description: "The code to review"
                          required: true
                  tools:
                    - name: "generate-code"
                      description: "Generate code based on requirements"
                      inputSchema:
                        type: "object"
                        properties:
                          requirements:
                            type: "string"
                            description: "The requirements for code generation"
                        required: ["requirements"]
                ```

3. **Configure the MCP Server project**

    1. **MCP Server information**

        Navigate to `<MCP_Server_Project>/mcp-server.yaml` file. 

        You can edit the mandatory information given below.

        | **Field**                     | **Description**                               |
        |-------------------------------|-----------------------------------------------|
        | **name**                      | The name of the MCP Server                   |
        | **description**               | A brief description about the MCP Server      |
        | **context**                   | MCP Server context (Proxy context path)      |
        | **version**                   | MCP Server version                            |

        For more information about the MCP Server definition, see [MCP Server Definition]({{base_path}}/design/create-api/create-rest-api/create-a-rest-api-from-an-open-api-definition/#mcp-server-definition).

        !!! note
            **If you need to add scopes** 
            
            Define the scopes in the `mcp-server.yaml` file as follows:

            ```yaml
            data:
              name: ChatGPTServer
              description: MCP Server for ChatGPT integration  
              context: /chatgpt
              version: 1.0.0
              provider: admin
              scopes:
                - scope_1
                - scope_2
            ```

    2. **Configure the deployment environment related details**

        To configure the MCP Server project directory, you can either manually add/edit the deployment configuration file or run the `apictl init` command to generate a deployment directory based on an existing deployment.yaml file.

        Navigate to `<MCP_Server_Project>/Configs` folder. This directory contains the deployment-related artifacts and configurations. This directory contains an `mcp_server_params.yaml` file and a directory named Deployment.

        - **mcp_server_params.yaml file** - Contains environment-specific details.

            !!! tip
                For more information about the environment-specific parametrization, see [Configuring Environment Specific Parameters]({{base_path}}/install-and-setup/setup/api-controller/advanced-topics/configuring-environment-specific-parameters/#configuring-environment-specific-parameters).

        - **Deployment directory** - Contains environment-specific deployment configuration files. By default, this directory contains a file named `deployment_environments.yaml`.

            !!!note
                **deployment_environments.yaml file** - Contains the environment-specific gateway environments and their configurations. This file specifies the gateway environments where the MCP Server will be deployed.

                For more information about gateway environments, see [Deploying an MCP Server to the gateway environments]({{base_path}}/deploy-and-publish/deploy-on-gateway/deploy-mcp-server-to-gateway-environments).

            You can create an environment-specific deployment configuration file by copying the default deployment configuration file and renaming it with the preferred environment name.

            !!! info "For example" 
                
                If you need to create a deployment configuration file for the **dev** environment, copy the `deployment_environments.yaml` file and rename the file as `dev.yaml`. 

            !!! note 
                The file extension should be `.yaml`.

            !!!tip
                For more information, see [Configure deployment configurations for the MCP Server]({{base_path}}/install-and-setup/setup/api-controller/advanced-topics/configuring-deployment-configurations-for-mcp-server).

    3. **Further configurations** 

        The initialized project contains the following additional directories.

        - **Definitions** - Contains the MCP Server specification files
        - **Docs** - Contains documents (This directory is currently not supported)
        - **Endpoints** - Contains the endpoint-related files
        - **Image** - Contains an image file
        - **Libs** - Contains libraries (This directory is currently not supported)
        - **Meta-information** - Contains artifacts related to the MCP Server
        - **Policies** - Contains all the MCP Server policies including the operational policies
        - **Sequences** - Contains all the mediation logic files

        !!! tip
            For more information, see [MCP Server project structure]({{base_path}}/install-and-setup/setup/api-controller/advanced-topics/mcp-server-project-structure).

    4. **Add the MCP Server specification**

        You need to add the MCP Server specification file under `<MCP_Server_Project>/Definitions` folder. When you initialize an MCP Server project using an existing MCP specification, apictl will automatically include the specification. The supported specification type is JSON and YAML.

    5. **Add a thumbnail image**

        You can add an MCP Server thumbnail image under `<MCP_Server_Project>/Image` folder. If you do not add a thumbnail image, a default image will be used as the thumbnail image.

4. **Import the MCP Server** 

    You can now import the MCP Server using the following command:

    ```bash
    apictl import mcp-server -f <path to MCP Server project> --environment <environment>  
    ```

    !!! tip
        For more information, see [Import an MCP Server]({{base_path}}/install-and-setup/setup/api-controller/managing-apis-api-products/managing-mcp-servers#import-an-mcp-server).

    !!! example
        ```bash
        apictl import mcp-server -f ./ChatGPTServer --environment dev  
        ```

    !!!note
        **Flags:**  
            
        -   Required :  
            `--file` or `-f` : The file path of the MCP Server to import.  
            `--environment` or `-e` : Environment to which the MCP Server should be imported.  
        -   Optional :  
            `--preserve-provider` : Preserve existing provider of MCP Server after importing. Default value is `true`.  
            `--update` : Update an existing MCP Server or create a new MCP Server in the importing environment.  
            `--params` : Provide a YAML file with environmental parameters to be substituted.  
            `--rotate-revision` : If the maximum revision limit reached, delete the oldest revision and create a new revision.

    **Response**

    ```bash
    Successfully imported MCP Server.
    ```
