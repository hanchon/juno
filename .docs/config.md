## Configuration
The default `config.toml` file should look like the following:

<details>

<summary>Default config.toml file</summary>

```toml
[cosmos]
modules = []
prefix = "cosmos"

[rpc]
address = "http://localhost:26657"
client_name = "juno"
max_connections = 20

[grpc]
address = "localhost:9090"
insecure = true

[parsing]
fast_sync = true
listen_new_blocks = true
parse_genesis = true
parse_old_blocks = true
start_height = 1
workers = 1

[database]
host = "localhost"
max_idle_connections = 0
max_open_connections = 0
name = "database-name"
password = "password"
port = 5432
schema = "public"
ssl_mode = ""
user = "user"

[pruning]
interval = 10
keep_every = 500
keep_recent = 100

[logging]
format = "text"
level = "debug"

[telemetry]
enabled = false
port = 5000
```

</details>

Let's see what each section refers to:

- [`cosmos`](#cosmos)
- [`rpc`](#rpc)
- [`grpc`](#grpc)
- [`parsing`](#parsing)
- [`database`](#database)
- [`pruning`](#pruning)
- [`logging`](#logging)
- [`telemetry`](#telemetry)

## `cosmos`
This section contains the details of the chain configuration regarding the Cosmos SDK.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `modules` | `array` | List of modules that should be enabled | `[ "auth", "bank", "distribution" ]` |
| `prefix` | `string` | Bech 32 prefix of the addresses | `cosmos` | 

### Supported modules
Currently we support the followings Cosmos modules:

- `auth` to parse the `x/auth` data
- `bank` to parse the `x/bank` data
- `consensus` to parse the consensus data
- `distribution` to parse the `x/distribution` data
- `gov` to parse the `x/gox` data
- `mint` to parse the `x/mint` data
- `modules` to get the list of enabled modules inside Juno
- `pricefeed` to get the token prices
- `slashing` to parse the `x/slashing` data
- `staking` to parse the `x/staking` data

## `rpc`
This section contains the details of the chain RPC to which Juno will connect.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `address` | `string` | Address of the RPC endpoint | `http://localhost:26657` |
| `client_name` | `string` | Client name used when subscribing to the Tendermint websocket | `juno` |
| `max_connections` | `int` | Max number of connections that can created towards the RPC node (any value less or equal to `0` means to use the default one instead) | `20` | 

## `grpc`
This section contains the details of the gRPC endpoint that Juno will use to query the data.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `address` | `string` | Address of the gRPC endpoint | `localhost:9090` |
| `insecure` | `boolean` | Whether the gRPC endpoint is insecure or not | `false` |

## `parsing`

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `fast_sync` | `boolean` | Whether Juno should use the fast sync abilities of different modules when enabled | `false` |
| `listen_new_blocks` | `boolean` | Whether Juno should parse new blocks as soon as they get created | `true` | 
| `parse_genesis` | `boolean` | Whether Juno needs to parse the genesis state or not | `true` |
| `parse_old_blocks` | `boolean` | Whether Juno should parse old chain blocks or not | `true` | 
| `start_height` | `integer` | Height at which Juno should start parsing old blocks | `250000` | 
| `workers` | `integer` | Number of works that will be used to fetch the data and store it inside the database | `5` |

## `database`
This section contains all the different configuration related to the PostgreSQL database where Juno will write the data.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `host` | `string` | Host where the database is found | `localhost` | 
| `port` | `integer` | Port to be used to connect to the PostgreSQL instance | `5432` |
| `name` | `string` | Name of the database to which connect to | `juno` | 
| `user` | `string` | Name of the user to use when connecting to the database. This user must have read/write access to all the database. | `juno` | 
| `password` | `string` | Password to be used to connect to the database instance | `password` | 
| `schema` | `string` | Schema to be used inside the database (default: `public`) | `public` | 
| `ssl_mode` | `string` | [PostgreSQL SSL mode](https://www.postgresql.org/docs/9.1/libpq-ssl.html) to be used when connecting to the database. If not set, `disable` will be used. | `verify-ca` |
| `max_idle_connections` | `integer` | Max number of idle connections that should be kept open (default: `1`) | `10` |
| `max_open_connections` | `integer` | Max number of open connections at any time (default: `1`) | `15` | 

## `pruning`
This section contains the configuration about the pruning options of the database. Note that this will have effect only
if you add the `"pruning"` entry to the `modules` field of the [`cosmos` config](#cosmos).

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `interval` | `integer` | Number of blocks that should pass between one pruning and the other (default: prune every `10` blocks) | `100` | 
| `keep_every` | `integer` | Keep the state every `nth` block, even if it should have been pruned | `500` | 
| `keep_recent` | `integer` | Do not prune this amount of recent states | `100` |

## `logging`
This section allows to configure the logging details of Juno.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `format` | `string` | Format in which the logs should be output (either `json` or `text`) | `json` | 
| `level` | `string` | Level of the log (either `verbose`, `debug`, `info`, `warn` or `error`) | `error` | 

## `telemetry`
This section allows to configure the telemetry details of Juno.

| Attribute | Type | Description | Example |
| :-------: | :---: | :--------- | :------ |
| `enabled` | `bool` | Whether the telemetry should be enabled or not | `false` | 
| `port` | `uint` | Port on which the telemetry server will listen | `8000` | 

**Note**  
If the telemetry server is enabled, a new endpoint at the provided port and path `/metrics` will expose [Prometheus](https://prometheus.io/) data.