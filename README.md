[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/ai-1st-deepview-mcp-badge.png)](https://mseep.ai/app/ai-1st-deepview-mcp)

# DeepView MCP

DeepView MCP is a Model Context Protocol server that enables IDEs like Cursor and Windsurf to analyze large codebases using Gemini's extensive context window.

[![PyPI version](https://badge.fury.io/py/deepview-mcp.svg)](https://badge.fury.io/py/deepview-mcp)
[![smithery badge](https://smithery.ai/badge/@ai-1st/deepview-mcp)](https://smithery.ai/server/@ai-1st/deepview-mcp)

## Features

- Load an entire codebase from a single text file (e.g., created with tools like repomix)
- Query the codebase using Gemini's large context window
- Connect to IDEs that support the MCP protocol, like Cursor and Windsurf
- Configurable Gemini model selection via command-line arguments

## Prerequisites

- Python 3.13+
- Gemini API key from [Google AI Studio](https://aistudio.google.com/)

## Installation

### Installing via Smithery

To install DeepView for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@ai-1st/deepview-mcp):

```bash
npx -y @smithery/cli install @ai-1st/deepview-mcp --client claude
```

### Using pip

```bash
pip install deepview-mcp
```

## Usage

### Starting the Server

Note: you don't need to start the server manually. These parameters are configured in your MCP setup in your IDE (see below).

```bash
# Basic usage with default settings
deepview-mcp [path/to/codebase.txt]

# Specify a different Gemini model
deepview-mcp [path/to/codebase.txt] --model gemini-2.0-pro

# Change log level
deepview-mcp [path/to/codebase.txt] --log-level DEBUG
```

The codebase file parameter is optional. If not provided, you'll need to specify it when making queries.

### Command-line Options

- `--model MODEL`: Specify the Gemini model to use (default: gemini-2.0-flash-lite)
- `--log-level {DEBUG,INFO,WARNING,ERROR,CRITICAL}`: Set the logging level (default: INFO)

### Using with an IDE (Cursor/Windsurf/...)

1. Open IDE settings
2. Navigate to the MCP configuration
3. Add a new MCP server with the following configuration:
   ```json
   {
     "mcpServers": {
       "deepview": {
         "command": "/path/to/deepview-mcp",
         "args": [],
         "env": {
           "GEMINI_API_KEY": "your_gemini_api_key"
         }
       }
     }
   }

Setting a codebase file is optional. If you are working with the same codebase, you can set the default codebase file using the following configuration:
  ```json
  {
     "mcpServers": {
       "deepview": {
         "command": "/path/to/deepview-mcp",
         "args": ["/path/to/codebase.txt"],
         "env": {
           "GEMINI_API_KEY": "your_gemini_api_key"
         }
       }
     }
   }
  ```

Here's how to specify the Gemini version to use:

```json
{
   "mcpServers": {
     "deepview": {
       "command": "/path/to/deepview-mcp",
       "args": ["--model", "gemini-2.5-pro-exp-03-25"],
       "env": {
         "GEMINI_API_KEY": "your_gemini_api_key"
       }
     }
   }
}
```

4. Reload MCP servers configuration


### Available Tools

The server provides one tool:

1. `deepview`: Ask a question about the codebase
   - Required parameter: `question` - The question to ask about the codebase
   - Optional parameter: `codebase_file` - Path to a codebase file to load before querying

## Preparing Your Codebase

DeepView MCP requires a single file containing your entire codebase. You can use [repomix](https://github.com/yamadashy/repomix) to prepare your codebase in an AI-friendly format.

### Using repomix

1. **Basic Usage**: Run repomix in your project directory to create a default output file:

```bash
# Make sure you're using Node.js 18.17.0 or higher
npx repomix
```

This will generate a `repomix-output.xml` file containing your codebase.

2. **Custom Configuration**: Create a configuration file to customize which files get packaged and the output format:

```bash
npx repomix --init
```

This creates a `repomix.config.json` file that you can edit to:
- Include/exclude specific files or directories
- Change the output format (XML, JSON, TXT)
- Set the output filename
- Configure other packaging options

### Example repomix Configuration

Here's an example `repomix.config.json` file:

```json
{
  "include": [
    "**/*.py",
    "**/*.js",
    "**/*.ts",
    "**/*.jsx",
    "**/*.tsx"
  ],
  "exclude": [
    "node_modules/**",
    "venv/**",
    "**/__pycache__/**",
    "**/test/**"
  ],
  "output": {
    "format": "xml",
    "filename": "my-codebase.xml"
  }
}
```

For more information on repomix, visit the [repomix GitHub repository](https://github.com/yamadashy/repomix).

## License

MIT

## Author

Dmitry Degtyarev (ddegtyarev@gmail.com)
