# geo-location-api
aws-samples .NET Web API for retrieving geolocations. Geolocation data provided by MaxMind GeoLite2.

This project contains the fully functional and final code for the Secure & Observe .NET Containerized Workloads on Amazon ECS on Fargate Workshop. You can find the content and instructions for the workshop here.

## Prerequisites
To get started you will need:
- [Docker](https://docs.docker.com/install/) installed on your local machine.
- [.NET](https://dotnet.microsoft.com/)
- An IDE for building .NET applications such as [Visual Studio](https://visualstudio.microsoft.com/)
- If you are building this sample on your own, sign up and download the free GeoLite-City.mmdb file from [MaxMind](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) and install the file in [/src/GeoLocationAPI](/src/GeoLocationAPI/). If you are attending an AWS workshop, we will provide you a link to download the file in the workshop instructutions. 

## MaxMind GeoLite2 Data
This product includes GeoLite2 data created by MaxMind, available from [https://www.maxmind.com](https://www.maxmind.com). 

This link leads to a Third-Party Dataset. AWS does not own, nor does it have any control over the Third-Party Dataset. You should perform your own independent assessment, and take measures to ensure that you comply with your own specific quality control practices and standards, and the local rules, laws, regulations, licenses and terms of use that apply to you, your content, and the Third-Party Dataset. AWS does not make any representations or warranties that the Third-Party Dataset is secure, virus-free, accurate, operational, or compatible with your own environment and standards. AWS does not make any representations, warranties or guarantees that any information in the Third-Party Dataset will result in a particular outcome or result.

## Running Locally

To get started, simply run the application in an IDE such as Visual Studio or issue the following command from the root of the project:
```sh
dotnet run --project GeoLocationAPI/GeoLocationAPI.csproj
```
Sample URL's:
- Swagger Definition: 
  - http://localhost:5254/swagger/index.html
- IPAddress of your machine:
  - http://localhost:5254/api/v1/geolocation/
- Look up an IPAddress:
  -  http://localhost:5254/api/v1/geolocation/8.8.8.8
- Healthcheck:
  - http://localhost:5254/hc

You can also run the project in Docker by running:
```sh
docker-compose -f docker-compose.yml -f docker-compose.development.yml up
```
Once the container is up and running you can simply do something like this:

```sh
curl -X GET "http://localhost:5254/api/v1/GeoLocation/71.168.176.139" -v
```

Sample Payload:
```
{
	"date": "2021-08-12T13:45:02.2587451Z",
	"ipAddress": "71.168.176.139",
	"city": "Trenton",
	"timeZone": "America/New_York",
	"continent": "North America",
	"country": "United States",
	"ipFoundInGeoDB": true,
	"message": "71.168.176.139 found in the GeoDB"
}
```
You can check the healthcheck by issuing something like this. Note that it will return *ProcessArchitecture* which is useful to see if the workload is running on Arm64 or X64. The HealtcheckIPToTest (in this case 8.8.8.8) is set in the appsettings.json files:

```sh
curl -X GET "http://localhost:5254/hc" -v
```

Sample Payload:
```
{
	"Status": "Healthy",
	"Duration": "00:00:00.0084379",
	"FrameworkDescription": ".NET 6.0.12",
	"ProcessArchitecture": "Arm64",
	"Results": {
		"GeoLocationHealthCheck": {
			"Status": "Healthy",
			"Description": "The healthcheck is healthy - 8.8.8.8 found in the GeoDB",
			"Data": {}
		}
	}
}
```
## Running on ECS Fargate

The [AWS CDK](https://aws.amazon.com/cdk/) is used to deploy the application to [ECS Fargate](https://aws.amazon.com/fargate/) and is protected with [AWS WAF](https://aws.amazon.com/waf/) via the CDK for C#. Follow the instructions in the [README.md](CdkGeoLocationApi/README.md).