# Lithium CLI

`li.exe` is the command-line interface for the Lithium Framework and it provides a set of commands that can be used to automate tasks and perform actions on known microservices and framework components.

This tools is installed by the Lithium SDK setup and can be used from the command line, PowerShell, Windows Terminal, etc.

> The `PATH` environment variable is updated by the SDK setup to point to the directory where this tools is installed (typically `C:\Program Files (x86)\PRIMAVERA\Lithium SDK v2.00\Tools`).

The generic usage of the Lithium CLI is:

```console
Usage: li [command] [options]

Options:
  --help          Show help information (optional).
  --version       Show version information (optional).

Commands:
  configuration   Invoke the configuration endpoint of a service.
  doc             Show Lithium documentation in the browser.
  endpoints       Invoke the endpoints endpoint of a service.
  list            List known microservices.
  mergespec       Merge spec files.
  probe           Invoke the probe endpoint of a service.
  token           Retrieve access tokens from the authority server.
```

## Commands

The following commands are available.

## Configuration

Allows invoking the configuration endpoint of a service.

```console
Usage: li configuration [options]

Options:
  --help                     Show help information (optional).
  --service <SERVICE>        The service that should be invoked (required).
  --deployment <DEPLOYMENT>  The deployment that should be invoked (optional).
```

To analyze the configuration of a specific microservice (production deployment):

```console
li configuration --service SS
```

To analyze the configuration of a specific deployment of a microservice (staging):

```console
li configuration --service SS --deployment ST
```

## Documentation

Allows opening the Lithium documentation site in the browser.

```console
Usage: li doc [options]

Options:
  --help  Show help information (optional).
  ```

## Endpoints

Allows invoking the endpoints endpoint of a service.

```console
Usage: li endpoints [options]

Options:
  --help                     Show help information (optional).
  --service <SERVICE>        The service that should be invoked (required).
  --deployment <DEPLOYMENT>  The deployment that should be invoked (optional).
```

To analyze the endpoints of a specific microservice (production deployment):

```console
li endpoints --service SS
```

To analyze the configuration of a specific deployment of a microservice (staging):

```console
li endpoints --service SS --deployment ST
```

### List

Allows obtaining a list of known microservices.

```console
Usage: li list [options]

Options:
  --help             Show help information (optional).
  --filter <FILTER>  The filter that should be applied to the results (optional).
  --deployments      Show deployment information (optional).
```

To list all known microservices:

```console
li list
```

To list all known microservices, including information about their deployments (base addresses):

```console
li list --deployments
```

To list the known microservices that match a given search term (matching the service identifier, name or category):

```console
li list --filter Notifications
```

### Merge Specifications

Allows merging two or more service specifications (`*.lsspec` files).

```console
Usage: li mergespec [options]

Options:
  --help           Show help information (optional).
  --input <FILE>   The full path of an input file (file that should be merged). Required (at least twice).
  --output <FILE>  The client identifier (required). Required.
```

To merge 2 files:

```console
li mergespec --input "C:\Temp\File1.lsspec" --input "C:\Temp\File2.lsspec" --output "C:\Temp\File3.lsspec"
```

To merge 3 files:

```console
li mergespec --input "C:\Temp\File1.lsspec" --input "C:\Temp\File2.lsspec" --input "C:\Temp\File3.lsspec" --output "C:\Temp\File4.lsspec"
```

### Probe

Allows invoking the probe endpoints of a specific microservice.

```console
Usage: li probe [options]

Options:
  --help                     Show help information (optional).
  --service <SERVICE>        The service that should be invoked (required if --all not specified).
  --all                      Probe all services (if specified, --service and --diagnostics are ignored).
  --deployment <DEPLOYMENT>  The deployment that should be invoked (optional).
  --diagnostics              Invoke the diagnostics endpoint (optional).
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
li probe --service SS --deployment ST --diagnostics
```

### Token

Allows obtaining access tokens from an authority server.

```console
Usage: li token [options]

Options:
  --help                         Show help information (optional).
  --authority <ADDRESS>          The authority server address (optional).
  --clientid <CLIENTID>          The client identifier (required).
  --clientsecret <CLIENTSECRET>  The client secret (required).
  --scope <SCOPE>                The scope (optional).
```

To retrieve an access token for a given client application from the default authority server:

```console
li token --clientid MyClientId --clientsecret MySecret
```

To retrieve an access token for a given client application from a specific authority server and for a specific scope:

```console
li token --clientid MyClient --clientsecret MySecret --scope MyScope --authority https://stg-identity.primaverabss.com
```
