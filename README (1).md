
# Anomaly detection PoC  

![pyton](https://img.shields.io/pypi/pyversions/numpy)  ![Contributor](https://img.shields.io/badge/Contributor-4-orange) 
![Language](https://img.shields.io/badge/Language-Python%20%2F%20C%23-blue)
![Organization](https://img.shields.io/badge/Organization-Unisys-red)
![Team](https://img.shields.io/badge/Team-White%20Lotus-orange)

This repository contain the Anomaly detection PoC details with high level architectural approach and details of each functionality. 
The main objective of this PoC is to figure out the existing anomalies from the given dataset. Get the better result after removing the anomalies 
which will give enrichment output, and seamless service for customer satistiction. We are using **UIT data** for _Azure_ and _Aws_ metrics in 
**Azrue blob storage**, This data is further process for _preprocessing in **Azure Data Factory(ADF)**_, and store into the blob storage. 
An Azure **Univariate anomaly detector**, and **Multivariate anomaly detector** are used to detect anomalies. 
Finally, the detected anomalies result are stored and displayed through a **dashboard**.

## Contributors
- [Ryan Mathison]()
- [Daniel Sollis]()
- [Patrick Deziel]()
- [Palash Mondal]()

## Licence
All the © copyrights are reserved by Unisys Corporation. For more information reach out to [Unisys](https://unisys.com). 

## Prerequisite
- [Python 3](https://www.python.org/, "Download")
- Access to Unisys [Bitbucket](https://ustr-bitbucket-1.na.uis.unisys.com:8443/projects/AIOPSCF/repos/aiopscf-azure-anomaly-detection-poc/browse)
- An IDE which support python
- Access in [Azure](https://portal.azure.com/#home, "Azure Portal")


## Pre-commit hook
In this project we are using python pre-commit framework for code formating and consistant linting practices for all contributors, and comminuty of
`AIOPSCF-Azure-Anomaly-Detection-PoC` repository. Natigate to your git directory and install pre-commit.
```bash
pre-commit install
```

## Clone the repository
`Clone` the repository into your local environment. 
Open Command Prompt (CMD) to clone the `AIOPSCF-Azure-Anomaly-Detection-PoC`repository. 

```bash
$ git clone ssh://git@ustr-bitbucket-1.na.uis.unisys.com:7999/aiopscf/aiopscf-azure-anomaly-detection-poc.git
``` 

## Install requirements
Some libraries has been used for anomaly detector function and in the notebook to get the result from anomaly detector. After clone the repository
install the requirements for contineous development. 

```bash
$ cd aiopscf-azure-anomaly-detection-poc
$ pip install -r functions/anomaly_detector_function_app/requirement.txt
$ pip install -r anomalyDetectorResult/requirements.txt
```

## Branching
The `AIOPSCF-Azure-Anomaly-Detection-PoC` repository set up accroding to the [Git Flow](https://guides.github.com/introduction/flow/, "Git flow")
of master/development/feature branch. The developer should push their code through a `feature branch`. Create a Pull Request (PR) from the pushed code and assign the reviewer, 
After post approval from the reviewer  the `feature branch` code should mearge into the `development branch`. The `master branch` contain the stable code for latest release, and the 
admin has permission only to push the latest code from `develop branch` to `master branch`. 

### Create a feature branch
Developer can create a `feature branch` directly from the `Jira` in `Create branch` of `development` section. 

Create `feature branch`: 
```bash
git checkout -b feature/<branch name>
```

## Projects Layout (High level)
[_**AIOPSCF-Azure-Anomaly-Detection-PoC**_](.AIOPSCF-Azure-Anomaly-Detection-PoC/)
```bash
├───.vscode
├───anomalyDetector
├───anomalyDetectorResult
├───dataFactory
|   ├───dataflow
|   ├───datapipeline
|   ├───dataset
|   ├───integrationRuntime
|   └───linkedService
├───eventHub
├───functions
|   ├───anomaly_detector_function_app
|   └───AnomalyDetectionPocPipeline
├───workbook
|   ├───AnomalyDetectorOverview
|   └───AnomalyDetectorVMOverview
├───.flake8
├───.pre-commit-config.yaml
├───pyproject.toml
└───README.md
```

## Azure-Anomaly-Detection-PoC components
## 1. [.vscode](..vscode/)
This is a simple setting.json file kept the information
of azure function filepath in anomaly detection pipeline.  

## 2.  [anomalyDetector](.anomalyDetector/)
Holds the ARM template for the anomaly detector used for ```Azure-Anomaly-Detection-PoC```.

## 3. [anomalyDetectorResult](./anomalyDetectorResult)
To get the Anomaly detector result and store the result into the blobstorage. Read the VMs metrics and model details for each VM
from blob storage after running the data pipeline. Get the univariate and multivariate anomalies and validate the results with historical anomalies. 
Write the validated anomalies for univariate and multivariate into the blob storage.

[_**anomalyDetectorResult**_](.anomalyDetectorResult/)
```bash
├───config
|   └───config.ini
├───Document
|   └───Anomaly Detection results.docx
├───anomaly_detector_result.py
├───README.md
└───requirements.txt
```
The [anomal_detector_result.py](.anomalyDetectorResult/) is the main script to get the detected anomalies result, and write the result into blob storage. 
All the configurable variable are kept secret in `config.ini`. In the Document folder [Anomaly Detection result.docs](.anomalyDetectorResult/) contains the details to get the detected anomalies. 
A [README.md](.anomalyDetectorResult/) file contains high level overview. The `requirements.txt` contains a list of libraries to run the `anomalies_detector_result.py`.

The list of environment variable need to set up to run the `anomaly_detector_result.py`. 
| Environment Variable                | Required | Description                                                    |
|-------------------------------------|----------|----------------------------------------------------------------|
| AZURE_TENANT_ID                     | true     | Azure active directory (tenant) ID                             |
| AZURE_CLIENT_ID                     | true     | Azure active directory Application (client) ID                 |
| AZURE_CLIENT_SECRET                 | true     | The client secrets value of register app in Azure active directory |

#### Steps to Reproduce:
- Set the AZURE_TENANT_ID, AZURE_CLIENT_ID and AZURE_CLIENT_SECRET as an environment variable.
- Run the Azure data factory pipeline.
- Install the requirement: _pip install -r anomalyDetectorResult/requirements.txt_
- Run anomaly_detector_result.py

## 4. [dataFactory](.dataFactory/)
This folder contains Azure data factory resources configuration used for ``` Azure-Anomaly-Detection-PoC ```.
#### dataFactory folder structure
_**dataFactory**_
```bash
├───dataflow
|   ├───AwsAllocationDataflow_PoC
|   ├───AwsUtilizationDataflowMetrics
|   ├───AzureAllocationDataflow_PoC
|   ├───AzureUtilizationDataflowMetrics
|   └───README.md
├───datapipeline
|   ├───AnomalyModel
|   ├───AnomalyModelLoop
|   ├───AwsAllocationPipeline-PoC
|   ├───AwsUtilizationPipeline_PoC
|   └───AzureAllocationPipeline-PoC
|   ├───AzureUtilizationDataflowMetrics
|   ├───AzureUtilizationPipeline_PoC
|   ├───CreateAnomalyDetectorModel
|   ├───CreateAnomalyDetectorModelLoop
|   └───README.md
├───dataset
|   ├───ADF
|   ├───AllocationCsvDataSet
|   ├───Azure_Utilization_CSV
|   ├───Azure_Utilization_CSV_Format_Final
|   ├───Azure_Utilization_ZIP
|   ├───copy_aws_utilizaiton
|   ├───copydata
|   ├───DF_AwsUtilizationJsonDataset
|   ├───Output_aws_allocation
|   ├───Output_aws_utilization
|   ├───Output_azure_allocation
|   ├───paste_zipdata
|   └───README.md
├───integrationRuntime
|   ├───AutoResolveIntegrationRuntime
|   └───README.md
├───linkedService
|   ├───aiops_key_vault
|   ├───AzureBlobStorageGeneric
|   ├───CreateModelJson
|   ├───GetSasToken
|   ├───key_value_metrics_data
|   ├───LinkedServices_StorageAccount_Metrics_PoC
|   └───README.md
└───README.md
```

- The [**_dataFactory_/_dataflow_/**](.dataFactory/dataflow/) contains the list of data flow for ```Azure```, ```Aws``` of metrics ```Allocation```, and ```Utilization```. 
  - **AwsAllocationDataflow_PoC** contains Aws allocation data flow. 
  - **AwsUtilizationDataflowMetrics** contains Aws utilization data flow. 
  - **AzureAllocationDataflow_PoC** contains Azure allocation data flow. 
  - **AzureUtilizationDataflowMetrics** contains Azure utilization data flow. 
- The **_dataFactory_/_datapipeline_/** folder contains the list of data pipeline which are associate with the above dataflow. A main datapipeline (**AnomalyModelLoop**) has been created irrespective of any metrics of a customer. 
  - **AnomalyModel**, and **AnomalyModelLoop** are the main pipeline. 
  - **AwsAllocationPipeline-PoC**, and **DirectoryAwsUtilizationPipeline_PoC** pipeline for `Aws` of metrics type `Allocation`, and `Utilization`.
  - **AzureAllocationPipeline-PoC**,  and **AzureUtilizationPipeline_PoC** pipeline for `Azure` of metrics type `Allocation`, and `Utilization`. 
  - **CreateAnomalyDetectorModel**, and **CreateAnomalyDetectorModelLoop** are the main pipeline for development, and testing it's functionality.

- The [**_dataFactory_/_dataset_/**](.dataFactory/dataset/)  contains the reference point of list of dataset of ```Azure```, and ```Aws``` .

- All the data factory task has been performed with inbuild Azure [AutoResolveIntegrationRuntime](.dataFactory/integrationRuntime/), and the details are kept in [**_dataFactory_/_integrationRuntime_/**](.dataFactory/integrationRuntime/).

- Azure data factory link services are associate with Azure other resources which are kept in [**_dataFactory_/_linkedService_/**](.dataFactory/linkedService/). 
    1. **aiops_key_vault**: Azure key-vault linked service.
    2. **AzureBlobStorageGeneric**: Azure blob storage linked service. 
    3. **CreateModelJson**: Azure function link service.
    4. **GetSasToken**: Azure function link service.
    5. **key_value_metrics_data**: Azure key-vault linked service. 
    6. **LinkedServices_StorageAccount_Metrics_PoC**: Azure blob storage linked service. 
---
## 5. [eventHub](.eventHub/)
Contain the Azure event hub configuration. The `template.json` file contains the event hub variable, and configuration details. 
| Parameter                                   | Required | Description                                                |
|---------------------------------------------|----------|------------------------------------------------------------|
| Azure Anomaly detector namespace name       | true     | The Azure event hub name                                   |
| Storage account externalid                  | true     | The storage account external id                            |



## 6. [functions](.functions/)
_**functions**_ contains Azure event hub and data factory functions. 
```bash
├───anomaly_detector_function_app
├───AnomalyDetectionPocPipeline
└───README.md
```


## 6a. [_AnomalyDetectionPocPipeline_](.functions/anomaly_detector_function_app/) 
A set of functions for Azure event hub that supports anomaly detection. 

**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*
- [Functions](#functions)
  - [BlobTrigger](#blobtrigger)
  - [MultivariateAnomalyDetectorAlertEventHubTrigger](#multivariateanomalydetectoralerteventhubtrigger)
  - [MultivariateContributorAnomalyDetectorAlertEventHubTrigger](#multivariatecontributoranomalydetectoralerteventhubtrigger)
  - [UnivariateAnomalyDetectorAlertEventHubTrigger](#univariateanomalydetectoralerteventhubtrigger)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Debug](#debug)
  - [Azure Storage Emulator](#azure-storage-emulator)
  - [Azure Storage Explorer](#azure-storage-explorer)


#### Functions

##### [BlobTrigger](.functions/anomaly_detector_function_app/BlobTrigger/README.md)

Triggers on csv and json files created by the data pipelines and then queries the anomaly detector for results which it then posts to the event hub.

#### [MultivariateAnomalyDetectorAlertEventHubTrigger](.functions/anomaly_detector_function_app/MultivariateAnomalyDetectorAlertEventHubTrigger/README.md)

Triggers on a message to the multivariate anomaly event hub and then writes the anomalies to log analytics.

#### [MultivariateContributorAnomalyDetectorAlertEventHubTrigger](.functions/anomaly_detector_function_app/MultivariateContributorAnomalyDetectorAlertEventHubTrigger/README.md)

Triggers on a message to the multivariate contributor event hub and then writes the anomalies to log analytics.

#### [UnivariateAnomalyDetectorAlertEventHubTrigger](.functions/anomaly_detector_function_app/UnivariateAnomalyDetectorAlertEventHubTrigger/README.md)

Triggers on a message to the univariate anomaly event hub and then writes the anomalies to log analytics.

#### Helpers

#### [Log Analytics](.functions/anomaly_detector_function_app/helper/README.md)

Adds common functions for storing custom data to log analytics

#### Configuration

| Environment Variable                       | Required | Description                                                |
|--------------------------------------------|----------|------------------------------------------------------------|
| AnomalyDetectorEndpoint                    | true     | The endpoint to access the anomaly detector                |
| AnomalyDetectorKey                         | true     | The key to access the anomaly detector                     |
| BlobConnectionString                       | true     | The blob storage connection string                         |
| ContributorEventhubName                    | true     | The event hub for multivariate contributor messages        |
| EventHubConnectionString                   | true     | The eventhub namespace connection string                   |
| LogAnalyticsCustomerID                     | true     | The log analytics workspace id                             |
| LogAnalyticsSharedKey                      | true     | The log analytics workspace access key                     |
| MultivariateContributorLogAnalyticsLogType | true     | The log analytics table name for multivariate contributors |
| MultivariateEventhubName                   | true     | The event hub for multivariate anomaly messages            |
| MultivariateLogAnalyticsLogType            | true     | The log analytics table name for multivariate anomalies    |
| UnivariateEventhubName                     | true     | The event hub for univariate anomaly messages              |
| UnivariateLogAnalyticsLogType              | true     | The log analytics table name for univariate anomalies      |

#### Deployment
**Make sure you are logged into azure from vscode and all [prerequisites](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=python#prerequisites) are satisfied**

Follow these [instructions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=python#prerequisites) to deploy the functions to azure

#### Debug
Follow these [instructions](https://code.visualstudio.com/docs/python/debugging) for debugging in vscode

#### Azure Storage Emulator
Follow these [instructions](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator) for using the storage emulator

#### Azure Storage Explorer
Follow these [instructions](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) for using the storage explorer


### 6b. [_AnomalyDetectionPocPipeline_](functions/AnomalyDetectionPocPipeline/) 
A set of functions that support the `Azure-Anomaly-Detection-PoC` pipeline. 

**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Functions](#functions)
  - [GetSasToken](#getsastoken)
  - [CreateModelJSON](#createmodeljson)
    - [Format](#format)
    - [Example](#example)
  - [CleanupModelJSON](#cleanupmodeljson)
  - [API](#api)
  - [Configuration](#configuration)
  - [Deployment](#deployment)
  - [Debug](#debug)
  - [Postman](#postman)

#### Functions

#### GetSasToken
Generates a sas token for read access to specified blob.

#### CreateModelJSON
Creates a json file that contains information about an included.

#### Format

| Key       | Description                                                    |
|-----------|----------------------------------------------------------------|
| source    | The sas token of the zip file that contains the detection data |
| startTime | The start time of the detection data                           |
| endTime   | The end time of the detection data                             |
| modelName | The name of the model                                          |
| model     | The url path of the model                                      |
| status    | The creation status of the model                               |

#### Example

```python
{
    "source": "https://cfaiopsstorageaccount.blob.core.windows.net/metrics-data/monitor-metrics/azure/utilization-2/AZRAUE-BUFDNS2.ap.uis.unisys.com/utilization.zip?sv=2020-08-04&se=2021-07-13T11%3A19%3A59Z&sr=b&sp=rw&sig=zfupk3IQQnT9Rbh%2sdgas%2Blp3L7lOqaRvtVV5DKYs%3D",
    "startTime": "2021-05-31T00:00:00Z",
    "endTime": "2021-07-13T10:20:01.4423011Z",
    "modelName": "AZRAUE-BUFDNS2.ap.uis.unisys.com",
    "model": "https://anomalydetector-poc-2.cognitiveservices.azure.com/anomalydetector/v1.1-preview/multivariate/models/e9gdsge38-e3c3-11eb-a337-4ae9a50a6332",
    "status": "READY"
}
```

#### CleanupModelJSON
Deletes all failed models along with models that match the specified model descriptions.

#### API

Latest OpenAPI Specification for the function API are available [here](.functions/AnomalyDetectionPocPipeline/api-specs).

#### Configuration

| Environment Variable | Required | Description                                                  |
|----------------------|----------|--------------------------------------------------------------|
| ConnectionString     | true     | The connection string for the storage account being accessed |
| SubscriptionKey      | true     | The subscription key of the anomaly detector                 |

#### Deployment

**Make sure you are logged into azure from vscode and all [prerequisites](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=csharp#prerequisites) are satisfied**

Follow these [instructions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=csharp#prerequisites) to deploy the functions to azure.

#### Debug

**Make sure the [c# extenision](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) has been installed and that the necessary [environmental variables](#configuration)**

Follow these [instructions](https://code.visualstudio.com/docs/editor/debugging) for debugging in vscode.

#### Postman 

Example postman requests and local env configuration has been provided [here](.functions/AnomalyDetectionPocPipeline/api-specs/postman).


## 7. [workbook](./workbook/)
```bash
├───AnomalyDetectorOverview
├───AnomalyDetectorVMOverview
└───README.md
```
Contains the Anomaly detector `AnomalyDetectorOverview` configuration, and Anomaly detector VM `AnomalyDetectorVMOverview` configurations for the workbook dashboards. 


## 8. .flake8
Global configuration to check code base against coding style.


## 9. .pre-commit-config.yaml
To manage and maintain multi-language pre-commit hooks.


## 10. pyproject.toml unified Python project settings file.
Unified python project settings file for linting/formatting.


## 11. README.md
The main mark down file for `AIOPSCF-Azure-Anomaly-Detection-PoC`.
