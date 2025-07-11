---
description: 'Guidelines for building F# applications'
applyTo: '**/*.fs'
---

# F# Development

## F# Instructions

- Target the latest stable F# release (currently F# 7).
- Use the `task { … }` computation expression for asynchronous workflows.
- Index arrays and slices with `myArray[0]` rather than the legacy `.[ ]` syntax.
- Write XML-style triple-slash comments (`///`) on all public APIs, including `<summary>`, `<param>`, and `<returns>` where appropriate.
- Favor expression-oriented, point-free style.
- Use pipeline (`|>`) breaks at function boundaries for clarity.

## General Instructions

- Provide only high-confidence suggestions when reviewing code changes
- Annotate non-obvious design decisions in comments.
- Handle edge cases explicitly.
- Prefer `Option` over `null` and use pattern matching for exhaustive checks.
- Use discriminated unions to model domain data and avoid ambiguous representations.

## Naming Conventions

- **Types**, **modules**, and **namespaces**: PascalCase (e.g., `CustomerOrder`, `MyModule`).
- **Functions** and **values**: camelCase without underscores (e.g., `calculateTotal`, `orderDate`).
- **Discriminated union cases**: PascalCase (e.g., `Pending`, `Completed`).
- **Interfaces**: prefix with `I` only when interop with other .NET languages is expected (e.g., `IService`).

## Formatting

- Use **Fantomas** (via `dotnet tool install fantomas`) configured in your `.editorconfig` to enforce consistent formatting.
- Follow Microsoft’s F# formatting guidelines: indent 4 spaces, no trailing whitespace.
- Group `open` statements at the top of the file, just underneath the `namespace` declaration.
- Place the pipeline operator (`|>`) at the start of continued lines for readability.

## Project Setup and Structure

- Initialize with `dotnet new console -lang F#` or `dotnet new template-name` (e.g., Giraffe) and explain each generated file in README.
- Organize code by feature folders (e.g., `Features/Orders`) or domain-driven structure: separate business logic, data access, and web layers.
- Use `global.json` to pin SDK versions for team consistency.

## API Versioning and Documentation

- Guide users through implementing and explaining API versioning strategies.
- Demonstrate OpenAPI implementation with proper documentation.
- Show how to document endpoints, parameters, responses, and authentication.
- Guide users on creating meaningful API documentation that helps consumers.

## Model Context Protocol (MCP) Tools

- Guide users to expose functionality in the app in the form of tools, resources, prompts and other constructs from the Model Context Protocol specification.
- Help users to think through the right tools to expose, and how to describe them for LLM's to consume.
- The MCP tools should do more than just exposing the existing API; they should be designed for LLM's to use well.

## Performance Optimization

- Guide users on implementing caching strategies (in-memory, distributed, response caching).
- Explain asynchronous programming patterns and why they matter for API performance.
- Demonstrate pagination, filtering, and sorting for large data sets.
- Show how to implement compression and other performance optimizations.
- Explain how to measure and benchmark API performance.

## Testing

- Always include test cases for critical paths of the application.
- Guide users through creating unit tests.
- Do not emit "Act", "Arrange" or "Assert" comments.
- Copy existing style in nearby files for test method names and capitalization.
- Explain integration testing approaches for API endpoints.
- Demonstrate how to mock dependencies for effective testing.
- Show how to test authentication and authorization logic.
- Explain test-driven development principles as applied to API development.
- Recommend property-based tests where possible; use FsCheck to implement them.
- If possible, use `Microsoft.Testing.Platform` with one of well-known testing frameworks.

## Logging and Monitoring

- Emit structured logs (`log.Information`, `log.Debug`, etc.).
- Configure **OpenTelemetry** exporters for distributed tracing in cloud deployments.

## Deployment and DevOps

- Containerize with `dotnet publish --os linux --arch x64 -p:PublishProfile=DefaultContainer` and optimize images with multi-stage Dockerfiles.
- Define CI/CD workflows in GitHub Actions or Azure Pipelines, include linting (Fantomas), testing, and security scans.
- Include health checks via ASP.NET Core middleware.
