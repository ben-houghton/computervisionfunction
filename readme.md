﻿# Image Analysis Function Using Azure Cognitive Services - C<span>#</span>

This Function will process images uploaded to an Azure Storage Blob container using the [Azure Cognitive Services Computer Vision](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api)
## How it works

When the an image is uploaded to the Azure Storage Account blob container (You can use [Azure Storage Explorer](http://storageexplorer.com/) for this) a `BlobTrigger` event is generated and the ImageAnalysisBlobTrigger puts the blob URL on an Azure Storage Queue.

Adding a message containing the image blob URL to the queue generates a `QueueTrigger` event which activates the ImageAnalysisQueueTrigger Function. This Function retrieves the message containing the image blob URL and posts this to the 
[Azure Cognitive Services Computer Vision](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api) API. The API performs images analysis and returns a JSON payload of data.

When the data is succesfully returned from the Computer Vision API, the JSON document is written to an [Azure DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/) where we could perform queries or analytics against the data.

To make this demo work, you need to - 

###Edit the appsettings.json file with your own values

```javascript
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "DefaultEndpointsProtocol=https;AccountName=[StorageAccountName];AccountKey=[Storage Account Key];",
    "AzureWebJobsDashboard": "DefaultEndpointsProtocol=https;AccountName=[StorageAccountName];AccountKey=[Storage Account Key];",
    "DocumentDBConnection": "AccountEndpoint=[Document DB EndPoint];AccountKey=[Document DB Account Key];"
  }
}
```
###Edit the ImageAnalysisQueueTrigger\run.csx file with your own [Computer Vision Subscription Key](https://www.microsoft.com/cognitive-services/en-US/sign-up?ReturnUrl=/cognitive-services/en-us/subscriptions)
```javascript

//Computer Vision Subscription Key
private const string SubscriptionKey = "[Your Cognitive Services Computer Vision Subscription Key]";

```
## Learn more about developing [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference)
