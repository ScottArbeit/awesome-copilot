---
description: 'Guidelines for building F# applications'
applyTo: '**/*.fs'
---

This document outlines best practices and personal conventions for F# codebases, leveraging the latest language features (e.g., `task {}` CE, array indexing syntax `myArray[0]`) and community tools (Fantomas, Giraffe, SAFE Stack) to ensure maintainability, readability, and consistency across projects.  

## F# Development

### F# Instructions
- Target the latest stable F# release (currently F# 7); use the `task { … }` computation expression for asynchronous workflows.
- Index arrays and slices with `myArray[0]` rather than the legacy `.[ ]` syntax.
- Write XML-style triple-slash comments (`///`) on all public APIs, including `<summary>`, `<param>`, and `<returns>` where appropriate.
- Favor expression-oriented, point-free style; use pipeline (`|>`) breaks at function boundaries for clarity.

## General Instructions
- Provide only high-confidence suggestions when reviewing code changes; annotate non-obvious design decisions in comments.
- Handle edge cases explicitly; prefer `Option` over `null` and use pattern matching for exhaustive checks\.
- Use discriminated unions to model domain data and avoid ambiguous representations.

## Naming Conventions
- **Types**, **modules**, and **namespaces**: PascalCase (e.g., `CustomerOrder`, `MyModule`).
- **Functions** and **values**: camelCase without underscores (e.g., `calculateTotal`, `orderDate`).
- **Discriminated union cases**: PascalCase (e.g., `Pending`, `Completed`).
- **Interfaces**: prefix with `I` only when interop with other .NET languages is expected (e.g., `IService`).

## Formatting
- Use **Fantomas** (via `dotnet tool install fantomas`) configured in your `.editorconfig` to enforce consistent formatting.
- Follow Microsoft’s F# formatting guidelines: indent 4 spaces, no trailing whitespace, group `open` statements at the top.
- Place the pipeline operator (`|>`) at the start of continued lines for readability.

## Project Setup and Structure
- Initialize with `dotnet new console -lang F#` or `dotnet new template-name` (e.g., Giraffe) and explain each generated file in README.
- Organize code by feature folders (e.g., `Features/Orders`) or domain-driven structure: separate business logic, data access, and web layers.
- Use `global.json` to pin SDK versions for team consistency.

## Web Development
- **Giraffe**: build on ASP.NET Core with functional HttpHandlers; structure handlers, routers, and dependency injection clearly.
- **SAFE Stack**: combine Saturn, Azure, Fable, and Elmish for full-stack F# apps; follow “Safe from Scratch” tutorial for toolchain setup.
- Consider **Saturn** for opinionated MVC-style apps and **Suave/Falco** for lightweight alternatives.

## Dependency Management & Build Automation
- Use **Paket** for precise NuGet dependency control; commit `paket.dependencies` and `paket.lock`.
- Automate tasks with **FAKE** scripts (`build.fsx`) for consistency in CI pipelines.

## Testing
- Use **Expecto** as the primary test framework; tests are async and parallel by default, define fixtures for Giraffe integration tests.
- Place all tests in a separate `*.Tests.fsproj` project; include `Expecto.FsCheck` and benchmarking integrations as needed.
- Write property-based tests using FsCheck and leverage Hopac for concurrency scenarios.

## Logging and Monitoring
- Integrate **Serilog** with sinks for console and files; emit structured logs (`Log.Information`).
- Configure **OpenTelemetry** exporters for distributed tracing in cloud deployments.

## Deployment and DevOps
- Containerize with `dotnet publish --os linux --arch x64 -p:PublishProfile=DefaultContainer` and optimize images with multi-stage Dockerfiles.
- Define CI/CD workflows in GitHub Actions or Azure Pipelines, include linting (Fantomas), testing (Expecto), and security scans.
- Include health checks via ASP.NET Core middleware.
