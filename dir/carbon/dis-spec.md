# Data Ingestion Service (DIS) Specification v1.0

The Data Ingestion Service allows to ingest data files like saft and persist this files in Carbon BigData.

# To send a file by API:

 You need to do a post request, using the URL request formation: ``` /api/v1/{Type of file} ```. For example, when you want to send a saft, the URL request is formed like this: ``` / api / v1 / saft```
 And you need to send a form-data body type with the parameters:
 
 Metadata - The json metadata key scopes like:
 
 ```
 Metadata - The json metadata key scopes like:
 
 {
	"scopes": [
		{
			"key": "physical",
			"value": "PRIMAVERA$PAA",
			"items": [
				{
					"key": "source",
					"value": "PRIMAVERA"
				},
				{
					"key": "product",
					"value": "PAA"
				}
			]
		},
		{
			"key": "business",
			"value": "123456789$ALIAS$ORG$COMPANY",
			"items": [
				{
					"key": "account",
					"value": "123456789"
				},
				{
					"key": "alias",
					"value": "ALIAS"
				},
				{
					"key": "organization",
					"value": "ORG"
				},
				{
					"key": "companyKey",
					"value": "COMPANY"
				}
			]
		}
	]
 }

 File - The file do you want to send
 ```
 
 The request returns a json with the internal id file. 

# To view a list of sended file status by API:

 You need to do a get request, using the URL request formation: `` `/` api/v1/{Type of file}/{physical key}/{business key}`` `. For example  `` /api/v1/saft/PRIMAVERA$PAA/123456789$ALIAS$ORG$COMPANY ```
 You can optionally put on this request the initial date search using the startDate field and pagination information using pageSize and pageIndex fields by querystring.

 
# To view the sended file status by API:

 You need to do a get request, using the URL request formation: `` `/` api/v1/{Type of file}/{id}`` `. For example  `` /api/v1/saft/15d891eb-ece1-4510-b8e1-49956d51a2d0 ```


