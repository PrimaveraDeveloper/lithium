# Digital Archive Service (DAS) Specification v1.0

The Digital Archive Service allows sending digital archives to be stored.

## Accepted Digital Archive types.

Are only accepted pdf, jpg, tiff and zip files and inside the zip files.

## To send a file by API:

 You need to do a post request, using the URL request formation: ``` /api/v1/archives/{business key} ``` and you need to send a form-data body type with the parameters:

 ``` 

 File - The file do you want to send.
 
 Metadata - The json metadata archives like:
 

 {
	"archives": [
		{
			"name": "{file name}",
			"metadata": [
				{
					"key": "{file metadata key}",
					"value": "{file metadata key}",
				}
			]
		}
	]
}

 ```
 
 For example  
 
 ```
 
 Request - /api/v1/archives/NUCASE$CAFECENTRAL$FG01A1
 
 Body Metadata Parameter:
 
 {
	"archives": [
		{
			"name": "teste.jpg",
			"metadata": [
				{
					"key": "isFiscalRelevant",
					"value": "1"
				}
			]
		}
	]
}

 ```
 
 The request returns a json with the internal id file. 

## To view a list of sended digital archive metadatas by API:

 You need to do a get request, using the URL request formation: ```/api/v1/archives/{business key}``` and you can send optionally by querystring the state, the category and the pagination parameters (pageSize and pageIndex). 
 For instance  ```/api/v1//archives/NUCASE$CAFECENTRAL$FG01A1?state=3&category=Unknown&pagesize=5&pageindex=1 ```
 
## To get a sended digital archive (file and metadata):

 You need to do a get request, using the URL request formation: ```/api/v1/archives/{business key}/{archive id}```. 
 For example  ```/api/v1/archives/ac2aa44d-54e5-48ac-82c9-447c7aeef257 ```.
 
 This request returns a zip file with the digital archive metadata json file and the digital archive file.

 ## To get a sended digital archive file:

 You need to do a get request, using the URL request formation: ```/api/v1/archives/{business key}/{archive id}/file```. 
 For instance  ```/api/v1/archives/ac2aa44d-54e5-48ac-82c9-447c7aeef257/file ```.
 
 This request returns the digital archive file.
 
 ## To update a sended digital archive file:

 You need to do a put request, using the URL request formation: ```/api/v1/archives/{business key}/{archive id}``` and a json object like:
 
 ```
 
{
    "state": {state},
    "isLocked": { false/true },
	"Metadata": [
		{
			"key": "{{file metadata key}}",
			"value": "{file metadata key}"
		}
	]
}
 
 ```
 
 All of the keys are optional and the accepted int states are 4 (Stored Fiscal Relevant), 5(Stored Not Fiscal Relevant) and 6(In Accounting).
 
 
 For example:  
 
 ```
 
 Request - /api/v1/archives/NUCASE$CAFECENTRAL$FG01A1/ac2aa44d-54e5-48ac-82c9-447c7aeef257 
 
 Body:
 
 {
    "state": 4,
    "isLocked": true,
	"Metadata": [
		{
			"key": "IsFiscalRelevant",
			"value": "1"
		}
	]
}
 
 ```
 
## To send a file by Client:

```csharp
	using DigitalArquiveClient client = new DigitalArquiveClient(...);
	
	try
		{
			metadata = File.ReadAllText(metadataJsonFilePath);

			using (FileStream digitalArchiveFile = File.OpenRead(filePath))
			{
				ServiceOperationResult<UploadMessage> response = await client.Archives.UploadDigitalArchivesAsync(metadata, digitalArchiveFile).ConfigureAwait(false);
			
			}
		
		}
	// (...)
	}
	catch (ServiceException ex)
	{
		// (...)
	}
	
 ```
 
 ## To update a file by Client:

```csharp
	using DigitalArquiveClient client = new DigitalArquiveClient(...);
	
	try
		{
			archiveJsom = File.ReadAllText(archiveJsonFilePath);
			
			ArchiveMessage archive = JsonSerializer.Deserialize<ArchiveMessage>(archiveJsom);

			ServiceOperationResult response = await client.Archives.UpdateDigitalArchivesAsync(businessKey, id, archive).ConfigureAwait(false);
			
		}
	// (...)
	}
	catch (ServiceException ex)
	{
		// (...)
	}
	
 ```
 
 ### Other Operations on Digital Archive

The service provides additional operations on digital archive:

- `GetDocumentAsync(string businessKey, string id)` - allows getting the sended digital archive (file and metadata).
- `GetDocumentFileAsync(string businessKey, string id)` - allows getting the sended digital archive file.
- `GettDigitalArchiveMetadataAsync(string businessKey, int state, DateTime startDate, DateTime endDate, int pageIndex, int pageSize, string category)` - allows viewing a list of sended digital archive metadatas.
- `DeleteDigitalArchivesAsync(string businessKey, string id)` - allows deleting the sended digital archive file.

## To send a authorized senders by API:

 You need to do a post request, using the URL request formation: ``` /api/v1/authorizedsenders/{business key} ``` and a json object like:
 
 ```
 
[
    {
        "companyTaxId": "{company tax id}",
        "sender": "{sender}",
        "type": "{sender type}"
    }
]
 
 ```

 For example:  
 
 ```
 
 Request - /api/v1/authorizedsenders/NUCASE$CAFECENTRAL$FG01A1
 
 Body:
 
 [
    {
        "companyTaxId": "505280070",
        "sender": "teste@primaverabss.com",
        "type": "email"
    }
]
 
 ```
 
 ## To view a list of authorized senders by API:

 You need to do a get request, using the URL request formation: ```/api/v1/authorizedsenders/{business key}``` and you can send optionally by querystring the pagination parameters (pageSize and pageIndex). 
 For instance  ```/api/v1//authorizedsenders/NUCASE$CAFECENTRAL$FG01A1?pagesize=5&pageindex=1 ```
 
  ## To delete a authorized sender by API:

 You need to do a delete request, using the URL request formation: ```/api/v1/authorizedsenders/{business key}/{Authorized Sender Id}```. 
 For example  ```/api/v1//authorizedsenders/NUCASE$CAFECENTRAL$FG01A1/TESTE@PRIMAVERABSS.COM$505280069 ```
 
   ## To delete a authorized senders by API:

 You need to do a delete request, using the URL request formation: ```/api/v1/authorizedsenders/{business key}```. 
 For instance  ```/api/v1//authorizedsenders/NUCASE$CAFECENTRAL$FG01A1 ```
 
 ## To send a authorized sender by Client:

```csharp
	using DigitalArquiveClient client = new DigitalArquiveClient(...);
	
	try
		{
			string businessKey = (...);
			authoSendersJson = File.ReadAllText(authoSendersJsonFilePath);
			
			List<AuthSenderMessage> authoSenders = JsonSerializer.Deserialize<List<AuthSenderMessage>>(authoSendersJson);

			ServiceOperationResult response = await client.AuthorizedSenders.PostAuthorizedSendersAsync(businessKey, authoSenders).ConfigureAwait(false);
			
		}
	// (...)
	}
	catch (ServiceException ex)
	{
		// (...)
	}
	
 ```
  ### Other Operations on Authorized Senders

The service provides additional operations on authorized senders:

- `GetAuthorizedSendersAsync(string businessKey, int pageIndex, int pageSize)` - allows viewing a list of authorized senders.
- `DeleteAuthorizedSendersAsync(string businessKey)` - allows deleting all authorized senders for a sended business key.
- `DeleteAuthorizedSenderAsync(string businessKey, string senderId)` - allows deleting the authorized sender.

## To send a authorized route by API:

 You need to do a post request, using the URL request formation: ``` /api/v1/authorizedroutes ``` and a json object like:
 
 ```
 
    {
        "companyTaxId": "{companyTaxId}",
        "workspace": "{workspace}",
        "organization": "{organization}",
	"appInstance": "{appInstance}"
    }
 
 ```

 For example:  
 
 ```
 
 Request - /api/v1/authorizedroutes
 
 Body:
 
{
	"companyTaxId": "PT$505280031",
	"workspace": "NUCASE",
	"organization": "CAFECENTRAL",
	"appInstance": "FG01A5"
}
 
 ```
 
## To view a list of authorized routes by API:

 You need to do a get request, using the URL request formation: ```/api/v1/authorizedroutes/{business key}``` and you can send optionally by querystring the pagination parameters (pageSize and pageIndex). 
 For instance  ```/api/v1//authorizedroutes/NUCASE$CAFECENTRAL$FG01A1?pagesize=5&pageindex=1 ```
 
## To delete a authorized routes by API:

 You need to do a delete request, using the URL request formation: ```/api/v1/authorizedroutes/{business key}```. 
 For example  ```/api/v1//authorizedroutes/NUCASE$CAFECENTRAL$FG01A1 ```
 
## To send a authorized route by Client:

```csharp
	using DigitalArquiveClient client = new DigitalArquiveClient(...);
	
	try
		{
			authRouteJson = File.ReadAllText(authRoutesJsonFilePath);
			
			AuthRouteMessage authRoute = JsonSerializer.Deserialize<AuthRouteMessage>(authRouteJson);

			ServiceOperationResult response = await client.AuthorizedRoutes.PostAuthorizedRoutesAsync(AuthRouteMessage).ConfigureAwait(false);
			
		}
	// (...)
	}
	catch (ServiceException ex)
	{
		// (...)
	}
	
 ```
  ### Other Operations on Authorized Senders

The service provides additional operations on authorized senders:

- `GetAuthorizedRoutesAsync(string businessKey, int pageIndex, int pageSize)` - allows viewing a list of authorized routes.
- `DeleteAuthorizedRoutesAsync(string businessKey)` - allows deleting a authorized route.
