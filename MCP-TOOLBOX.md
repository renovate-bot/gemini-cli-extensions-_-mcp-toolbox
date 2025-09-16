# MCP Toolbox: Configuring Custom Tools

This document provides instructions on how to configure custom tools for the MCP Toolbox using the `tools.yaml` file.

## `tools.yaml` Configuration

The primary way to configure the MCP Toolbox is through the `tools.yaml` file. This file allows you to define data sources, custom tools, and toolsets.

### Using Environment Variables

To avoid hardcoding secrets like passwords, usernames, or API keys in your `tools.yaml` file, you can use environment variables with the format `${ENV_NAME}`.

```yaml
user: ${USER_NAME}
password: ${PASSWORD}
```

You can also provide a default value like this: `${ENV_NAME:default}`.

```yaml
port: ${DB_PORT:3306}
```

### Defining Sources

The `sources` section of your `tools.yaml` defines the data sources that your tools can connect to. Most tools will require a source to execute against.

Here is an example of a `postgres` source:

```yaml
sources:
  my-pg-source:
    kind: postgres
    host: 127.0.0.1
    port: 5432
    database: toolbox_db
    user: ${USER_NAME}
    password: ${PASSWORD}
```

For more details on configuring different types of sources, refer to the official documentation.

### Defining Tools

The `tools` section of your `tools.yaml` is where you define the custom actions that your agent can take. For each tool, you specify its kind, the source it uses, a description, and the parameters it accepts.

Here is an example of a `postgres-sql` tool:

```yaml
tools:
  search-hotels-by-name:
    kind: postgres-sql
    source: my-pg-source
    description: Search for hotels based on name.
    parameters:
      - name: name
        type: string
        description: The name of the hotel.
    statement: SELECT * FROM hotels WHERE name ILIKE '%' || $1 || '%';
```

For more details on configuring different types of tools, refer to the official documentation.

### Defining Toolsets

The `toolsets` section allows you to group tools together. This is useful for creating different sets of tools for different agents or applications.

```yaml
toolsets:
  my_first_toolset:
    - my_first_tool
    - my_second_tool
  my_second_toolset:
    - my_second_tool
    - my_third_tool
```

You can then load a specific toolset in your application by its name.
