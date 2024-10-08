# Golem Cloud example templates

This repository contains all the *examples* available for the `golem` CLI tool using via the `golem new` command.

See the example templates section on the [Golem CLI page](https://www.golem.cloud/learn/golem-cli).

## Structure

The examples are organized to directories per **guest languages**. Each guest language directory contains an `INSTRUCTIONS` text file, which is a template itself and gets printed as a result of the `golem new` command.

Each subdirectory of the guest languages is a template where the directory's name becomes the template's name.

Each **example** consists of arbitrary number of files and subdirectories and a `metadata.json` file.

The `golem new` command applies the below defined **template rules** for each file's and directory's name, and for each file's contents.

The metadata file contains required information and also allows some additional project generation steps to be enabled.

### Metadata JSON
The following fields are required:

- `description` is a free-text description of the example

The following fields are optional:

- `requiresAdapter` is a boolean, defaults to **true**. If true, the appropriate version of the WASI Preview2 to Preview1 adapter is copied into the generated project (based on the guest language) to an `adapters` directory.
- `requiresGolemHostWIT` is a boolean, defaults to **false**. If true, the Golem specific WIT interface gets copied into `wit/deps`.
- `requiresWASI` is a boolean, defaults to **false**. If true, the WASI Preview2 WIT interfaces which are compatible with Golem Cloud get copied into `wit/deps`.
- `exclude` is a list of sub-paths and works as a simplified `.gitignore` file. It's primary purpose is to help the development loop of working on examples and in the future it will likely be dropped in favor of just using `.gitignore` files.

### Template rules

Golem examples are currently simple and not using any known template language, in order to keep the examples **compilable** as they are - this makes it very convenient to work on existing ones and add new examples as you can immediately verify that it can be compiled into a _Golem template_.

When calling `golem-new` the user specifies a **template name**. The provided component name must use either `PascalCase`, `snake_case` or `kebab-case`.

There is an optional parameter for defining a **package name**, which defaults to `golem:component`. It has to be in the `pack:name` format.

The following occurrences get replaced to the provided component name, applying the casing used in the template:
- `component-name`
- `ComponentName`
- `component_name`
- `pack::name`
- `pack:name`
- `pack_name`
- `pack-name`
- `pack/name`
- `PackName`

### Testing the examples
The example generation and instructions can be tested with a test [cli app](/src/test/main.rs).
The app also accepts a filter argument, which matches for the example name, eg. to test the go examples use:

```shell
cargo run --bin golem-examples-test-cli -- -f go
```

The necessary tooling for the specific language is expected to be available.

The test app will instantiate examples and then execute the instructions (all lines starting with `  `).

The examples are generated in the `/examples-test` directory.
