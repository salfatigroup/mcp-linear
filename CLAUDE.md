# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an MCP (Model Context Protocol) server implementation for the Linear GraphQL API. It enables AI assistants to interact with Linear project management systems through a standardized protocol.

## Architecture

The codebase follows a layered architecture:

1. **Entry Point**: `src/index.ts` - CLI executable that initializes the Linear client and MCP server
2. **MCP Server**: `src/mcp-server.ts` - Handles MCP protocol implementation and request routing
3. **Service Layer**: `src/services/linear-service.ts` - Contains all Linear API business logic (~40 methods)
4. **Tools Layer**: 
   - `src/tools/definitions/` - Tool metadata and JSON schemas
   - `src/tools/handlers/` - Tool implementations that call LinearService methods
   - `src/tools/type-guards.ts` - Runtime validation for tool arguments

## Development Commands

```bash
# Install dependencies
npm install

# Build the project (compiles TypeScript to dist/)
npm run build

# Run in development mode with auto-reload
npm run dev

# Run with MCP inspector for debugging
npm run inspect

# Lint the code
npm run lint

# Format code with Prettier
npm run format

# Run tests (currently no tests implemented)
npm test
```

## Testing a Single Tool

To test a specific tool during development:
1. Run `npm run dev` to start the development server
2. Use the MCP inspector (`npm run inspect`) to manually invoke specific tools
3. Or create a test script that uses the MCP SDK to call the tool directly

## Configuration

The server requires a Linear API token, which can be provided:
- Via command line: `--token YOUR_TOKEN`
- Via environment variable: `LINEAR_API_TOKEN`

## Adding New Tools

1. Define the tool schema in `src/tools/definitions/[category]-tools.ts`
2. Implement the handler in `src/tools/handlers/[category]-handler.ts`
3. Add the corresponding method in `src/services/linear-service.ts`
4. Export the new tool definition from `src/tools/definitions/index.ts`
5. Register the handler in `src/mcp-server.ts`

## Key Dependencies

- `@linear/sdk` - Official Linear SDK for API interactions
- `@modelcontextprotocol/sdk` - MCP protocol implementation
- `zod` - Schema validation (consider using for stronger input validation)

## Important Notes

- The project uses ES modules (type: "module" in package.json)
- TypeScript compiles to ES2022 targeting Node.js 20+
- No tests exist yet - implement tests when adding new features
- The Linear SDK handles authentication and GraphQL queries internally