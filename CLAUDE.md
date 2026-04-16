# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Restore dependencies
dotnet restore

# Build
dotnet build

# Run the API (available at http://localhost:5000/swagger)
dotnet run --project API/API.csproj

# Add a new EF Core migration
dotnet ef migrations add <MigrationName> --project API

# Apply migrations manually
dotnet ef database update --project API
```

There are no test projects in this solution.

## Architecture

This is an **ASP.NET Core 5.0** REST API using a simple layered structure:

- `API/Controllers/` — HTTP request handlers
- `API/Entities/` — Domain models (currently only `Product`)
- `API/Data/` — `StoreContext` (EF Core DbContext), migrations, and `DbInitializer` for seed data

**Startup flow:**
1. `Program.cs` runs pending EF migrations and calls `DbInitializer.Initialize()` to seed 18 sample products if the database is empty.
2. `Startup.cs` registers `StoreContext` as a scoped service and configures Swagger (Development only).

**Database:** SQLite (`store.db` in the API project root), Code-First with EF Core. Connection string is in `appsettings.Development.json`. The database file is auto-created and seeded on first run.

**Swagger UI** is auto-launched at `/swagger` in Development and is the primary way to explore/test the API.
