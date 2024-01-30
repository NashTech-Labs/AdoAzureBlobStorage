# Azure Pipeline for Uploading Files to Azure Blob Storage

This Azure Pipeline script is designed to upload files from a local directory to Azure Blob Storage using the Azure CLI and AzCopy.

## Parameters


| Parameter            | Type | Description                                                                                  |
|----------------------|--------|----------------------------------------------------------|
| `storageAccount`     | String | The name of your Azure Storage Account, a unique identifier within the Azure ecosystem.    |
| `containerName`      | String | The name of the Azure Storage Container where files will be stored.           |
| `blobPrefix`         | String | A prefix to be appended to the Blob's name, allowing you to categorize or organize related data.    |
| `sourceDirectory`    | String | The local directory path where your source files are located.                 |
| `ARMserviceConnection` | String  | The name of your Azure Resource manager Service Connection having scope of the Storage Account|
| `resourceGroup`      | String | The Azure resource group where the Storage Account will be created if it doesn't exist.    |
| `location`           | String | The Azure region or location where the Storage Account will be created if it doesn't exist.  |
| `accountKey`         | String | The storage account key used for authentication. It's recommended to use secure methods for production scenarios. |

## use case

You need to have a service connection related to GitHub on the Azure DevOps project.
To call this in your pipeline you can follow this example:

   ```yaml
  # azure-pipeline.yml
  parameters:
  - name: storageAccount
    type: string
    default: YourDefaultStorageAccount

  - name: containerName
    type: string
    default: YourDefaultContainerName

  - name: blobPrefix
    type: string
    default: YourDefaultBlobPrefix

  - name: sourceDirectory
    type: string
    default: YourDefaultSourceDirectory

  - name: ARMserviceConnection
    type: string
    default: ARMserviceConnection

  - name: resourceGroup
    type: string
    default: YourResourceGroup

  - name: location
    type: string
    default: YourLocation

  - name: accountKey
    type: string
    default: YourAccountKey

  resources:
    repositories:
      - repository: blob
        type: github
        name: NashTech-Labs/AdoAzureBlobStorage
        ref: main
        endpoint: 'GitHubServiceConnection'

  steps:

  - template: push.yml@blob
    parameters:
        storageAccount:  "${{parameters.storageAccount}}"
        containerName:   "${{parameters.containerName}}"
        blobPrefix:      "${{parameters.blobPrefix}}"
        sourceDirectory: "${{parameters.sourceDirectory}}"
        ARMserviceConnection: "${{parameters.ARMserviceConnection}}"
        resourceGroup: "${{parameters.resourceGroup}}"
        location: "${{parameters.location}}"
        accountKey: "${{parameters.accountKey}}"
  ```

You can also use Variable Group and Azure Key Vault for values.
