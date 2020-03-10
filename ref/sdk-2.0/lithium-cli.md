# Lithium CLI

`li.exe` is the command-line interface for the Lithium Framework and it provides a set of commands that can be used to automate tasks and perform actions on known microservices and framework components.

This tools is installed by the Lithium SDK setup and can be used from the command line, PowerShell, Windows Terminal, etc.

> The `PATH` environment variable is updated by the SDK setup to point to the directory where this tools is installed (typically `C:\Program Files (x86)\PRIMAVERA\Lithium SDK v2.00\Tools`).

The generic usage of the Lithium CLI is:

```console
Usage: li [command] [options]

Options:
  --help     Show help information (optional)
  --version  Show version information (optional)

Commands:
  list       List services
  probe      Invoke the probe endpoint of a service
  token      Retrieve access tokens from the authority server
```

## Commands

The following commands are available.

### List

Allows obtaining a list of known microservices.

```console
Usage: li list [options]

Options:
  --help             Show help information (optional)
  --filter <FILTER>  The filter that should be applied to the results
  --deployments      Show deployment information
```

To list all known microservices:

```console
li list
```

To list all known microservices, including information about their deployments (base addresses):

```console
li list --deployments
```

To list the known microservices that match a given search term (matching the service identifier and/or name):

```console
li list --filter Notifications
```

### Probe

Allows invoking the probe endpoints of a specific microservice.

```console
Usage: li probe [options]

Options:
  --help                         Show help information (optional)
  --service <SERVICE>            The service that should be probed (required)
  --deployment <DEPLOYMENT>      The deployment that should be probed (optional)
  --diagnostics                  Invoke the diagnostics endpoint (optional)
  --authority <ADDRESS>          The authority server address (optional only for --diagnostics)
  --clientid <CLIENTID>          The client identifier (required only for --diagnostics)
  --clientsecret <CLIENTSECRET>  The client secret (required only for --diagnostics)
  --scope <SCOPE>                The scope (required only for --diagnostics)
```

To probe a specific microservice (production deployment):

```console
li probe --service SS
```

To probe a specific deployment of a microservice (staging):

```console
li probe --service SS --deployment ST
```

To invoke the diagnostics endpoint of a microservice:

```console
li probe --service SS --deployment ST --diagnostics --authority https://stg-identity.primaverabss.com --clientid MyClientId --clientsecret MySecret --scope lithium-settings
```

### Token

Allows obtaining access tokens from an authority server.

```console
Usage: li token [options]

Options:
  --help                         Show help information (optional)
  --authority <ADDRESS>          The authority server address (optional)
  --clientid <CLIENTID>          The client identifier (required)
  --clientsecret <CLIENTSECRET>  The client secret (required)
  --scope <SCOPE>                The scope (optional)
```

To retrieve an access token for a given client application from the default authority server:

```console
li token --clientid MyClientId --clientsecret MySecret
```

To retrieve an access token for a given client application from a specific authority server and for a specific scope:

```console
li token --clientid MyClient --clientsecret MySecret --scope MyScope --authority https://stg-identity.primaverabss.com
```
