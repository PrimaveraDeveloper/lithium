# Capability - Monitoring and Health Checks

> Tags: Health, Probe, Diagnostics

All Lithium microservices provide 4 monitoring endpoints that should be used to check the service health and monitor its behavior.

## Probe

This endpoint allows probing the service health and it should perform fast validation of the service dependencies and minimal functionality.

The probe endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/probe`

> This endpoint is not secured (it does not require an authorization scope).

## Diagnostics

This endpoint allows probing the service health in more detail. It should allow validating the service performance and more complex functionality.

The diagnostics endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/diagnostics`

> This endpoint is secured (required the default service scope).

## Endpoints

This endpoint lists all the endpoints provided by the service Web API.

The endpoints endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/endpoints`

> This endpoint is secured (required the default service scope).

## Configuration

This endpoint lists all the configuration options active in the service application.

The configuration endpoint is available in the following route: `/api/v{version:apiVersion}/monitoring/configuration`

> This endpoint is secured (required the default service scope).