# Capability - Monitoring and Health Checks

> Tags: Health, Probe, Diagnostics

All Lithium microservices provide 4 monitoring endpoints that should be used to check the service health and monitor its behavior.

> These endpoints are not secured (they do not require any authorization scope).

## Probe

This endpoint allows probing the service health and it should perform fast validation of the service dependencies and minimal functionality.

The probe endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/probe`

## Diagnostics

This endpoint allows probing the service health in more detail. It should allow validating the service performance and more complex functionality.

The diagnostics endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/diagnostics`

## Endpoints

This endpoint lists all the endpoints provided by the service Web API.

The endpoints endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/endpoints`

## Configuration

This endpoint lists all the configuration options active in the service application.

> This endpoint does not return the configuration options. It saves them in a blob storage container named `ConfigAnalyzer`, if the service references the Blob Storage Service, or in a file - `.\.Config\ConfigAnalyzer.json` - otherwise.

The configuration endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/configuration`