# Provides solc download management and contract verification RPC service

## Installation
```
make install
```

## Project Initialization
```
solc-compiler init --platform macosx-amd64/linux-amd64 -a
```


Initialization requires specifying the project runtime platform. During initialization, you can specify whether to download all `solc` versions for the runtime platform.

You can reinitialize to override the current configuration. If file downloads fail during initialization, you can:

```
// Retry initialization - this process will automatically skip already downloaded compiler versions

solc-compiler init --platform macosx-amd64 -a

// Manually fetch a specific version using the executable

solc-compiler fetch v0.8.0
```

## Managing Compiler Versions
```
// Download
solc-compiler fetch v0.8.0

// Delete
solc-compiler delete v0.8.0
```

## Compiling Solidity Files
```
solc-compiler compile --scope bin,abi,hashes --name Counter v0.5.11 [path_to_file] --optimize --optimize-runs 200 --evm-version "default"
--scope         Specify compilation output content
--name          Specify contract to compile
--optimize      Specify whether to optimize compilation
--optimize-runs Specify optimization parameter
--evm-version   Specify EVM version
For detailed parameters, please refer to the help documentation
```

## Starting the RPC Server
```
solc-compiler rest-server
Provides the following endpoints:

contract_ping: Heartbeat

curl --location --request POST 'http://127.0.0.1:1212' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "contract_ping",
    "params": [],
    "id": 1
}'

contract_verify: Returns ABI and compiled binary hex string

curl --location --request POST 'http://127.0.0.1:1212' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "contract_verify",
    "params": [
        {
            "name": "Test",
            "compiler_version": "v0.5.11+commit.22be8592",
            "code":"pragma solidity ^0.5.11; contract Test {}",
	        "optimize": false,
            "optimize_runs": 0,
            "evm_version": "default"
        }
    ],
    "id": 1
}'

contract_listVersions: Returns supported compiler versions for the current platform

curl --location --request GET 'http://127.0.0.1:1212' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "contract_listVersions",
    "params": [],
    "id": 1
}'
```
