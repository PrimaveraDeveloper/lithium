# Digital Archive Service (DAS) Specification v1.0

The Digital Archive Service allows sending digital archives to be stored.

## Accepted Digital Archive types.

Are only accepted pdf, jpg, tiff and zip files and inside the zip files are only accepted the other accepted files.

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
 
 For example  ```
 
 Request - /api/v1/archives/NUCASE$CAFECENTRAL$FG01A1
 
 Body Metadata Parameter:
 
 {
	"archives": [
		{
			"name": "teste.jpg",
			"metadata": [
				{
					"key": "IsFiscalRelevant",
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
 For example  ```/api/v1//archives/NUCASE$CAFECENTRAL$FG01A1?state=3&category=Unknown&pagesize=5&pageindex=1 ```
 
## To get a sended digital archive (file and metadata):

 You need to do a get request, using the URL request formation: ```/api/v1/archives/{business key}/{archive id}```. 
 For example  ```/api/v1/archives/ac2aa44d-54e5-48ac-82c9-447c7aeef257 ```.
 
 This request returns a zip file with the digital archive metadata json file and the digital archive file.

 ## To get a sended digital archive file:

 You need to do a get request, using the URL request formation: ```/api/v1/archives/{business key}/{archive id}/file```. 
 For example  ```/api/v1/archives/ac2aa44d-54e5-48ac-82c9-447c7aeef257/file ```.
 
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
 
 All of the keys are optional and the accepted int states are 4, 5 and 6.
 
 
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
 
 ```.
 
## To view the sended file status by Client:

 
