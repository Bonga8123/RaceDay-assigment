# RaceDay API (ASP.NET Core)

Starter implementation of the RaceDay event management API using **C# / ASP.NET Core Minimal APIs** and **SQLite**.

## Requirements
- .NET 8 SDK
- Visual Studio Code
- C# Dev Kit extension (recommended)

## Run in VS Code terminal
1. Extract this folder.
2. Open the `RaceDayApi` folder in VS Code.
3. Open Terminal → New Terminal.
4. Run:
   ```bash
   dotnet restore
   dotnet run
   ```
5. Use the HTTPS/HTTP URL printed in the terminal and add `/swagger`, for example `https://localhost:7000/swagger`.

The SQLite file `raceday.db` is created automatically on first run.

## Important role note
Public registration intentionally creates a `Participant` account. For this starter, organiser-only endpoints require a JWT with the `EventOrganiser` role. Add a secure admin-only process to promote approved accounts before real deployment; do not allow public registration to choose their own role.

## Main endpoints
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/users/me`
- `PUT /api/users/me`
- `PATCH /api/users/me/password`
- `GET /api/users/me/performance`
- `GET /api/users/me/enrolments`
- `GET /api/categories`
- `POST /api/categories` (EventOrganiser)
- `GET /api/events`
- `GET /api/events/{id}`
- `POST /api/events` (EventOrganiser)
- `POST /api/events/{eventId}/enrolments` (Participant)
- `GET /api/events/{eventId}/enrolments` (EventOrganiser)
- `GET /api/events/{eventId}/results`
- `POST /api/events/{eventId}/results` (EventOrganiser)
- `GET /api/events/{eventId}/weather`
- `GET /api/events/{eventId}/route`
- `GET /api/health`

## Test registration and login
Register:
```json
{
  "name": "Test Participant",
  "email": "participant@example.com",
  "password": "ExamplePass123!"
}
```

Login:
```json
{
  "email": "participant@example.com",
  "password": "ExamplePass123!"
}
```

Copy `accessToken` from the login response and click **Authorize** in Swagger. Enter:
`Bearer YOUR_ACCESS_TOKEN`

## Create an organiser account for development
This starter does not provide a public role-change endpoint for security reasons. To test organiser-only routes, promote a test user's Role to `EventOrganiser` in the SQLite database using a database viewer, then log in again to obtain a token with the updated role. Never expose this process in production.

## Database choice
SQLite is selected because it is simple to run locally and does not require a separate database server. For a larger cloud deployment, consider PostgreSQL or SQL Server and use EF Core migrations.

## Limitations / next steps
- Weather endpoint is a placeholder; connect a weather API to return real forecasts.
- Route endpoint returns route information saved on the event; add a map/route provider if the requirements demand it.
- Add EF Core migrations, automated tests, stronger validation, secrets management, logging, and deployment configuration before production.
- JWT key in `appsettings.json` is for local development only. Store a strong secret in user-secrets/environment variables in deployment.
