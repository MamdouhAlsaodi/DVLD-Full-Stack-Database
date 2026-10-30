# DVLD — Driver & Vehicle License Department (Desktop Application)

DVLD is a Windows desktop application for managing a driving license
department: people, applications, license classes, tests and appointments,
license issuing/renewal/replacement, and license detention — built as a
training/portfolio project with a classic three-layer .NET architecture.

> **Note:** This documentation describes the source as reviewed; deployment
> status is unverified. Functional claims below are derived from the repository,
> not from executed test runs.

## Features (as implemented in source)

- **Login & user management** — login screen, user list, add/update users,
  change password (see `DVLD-Forms/Login`, `DVLD-Forms/User`).
- **People management** — list, search, add/update/delete person records
  (`DVLD-Forms/People`).
- **Applications** — application types, local driving-license applications,
  international licenses, renewal, replace lost/damaged license, release
  detained license (`DVLD-Forms/Applications`).
- **Licenses** — issue first-time driver license, license history, license
  info viewer, detain license, international licenses (`DVLD-Forms/Licenses`).
- **Tests** — test types, schedule tests, take tests, test appointments
  (`DVLD-Forms/Tests`).
- **Drivers** — list of drivers (`DVLD-Forms/Drivers`).

## Architecture

Three projects in a single Visual Studio solution (`DVLD-Forms/DVLD.sln`),
all targeting **.NET Framework 4.8**:

| Project | Type | Role |
|---|---|---|
| `DVLD-Forms` (DVLD) | WinExe | Windows Forms UI layer |
| `DVLD_Buisness` | Library | Business logic / domain classes |
| `DVLD_DataAccess` | Library | Data access via **ADO.NET** (`SqlConnection`, parameterized commands) against SQL Server |

Data access is direct ADO.NET (no ORM). The connection string is defined in
`DVLD_DataAccess/clsDataAccessSettings.cs`.

## Prerequisites

- **Windows** (WinForms desktop app)
- **Visual Studio** (solution built with VS 2022 format) with the
  **.NET Framework 4.8** developer pack / targeting pack
- **SQL Server** (LocalDB, Express, or full edition) — to host the database

## Database setup

1. The repository includes `DVLD.bak`, a SQL Server backup file.
   **Its contents have not been reviewed or verified by this documentation.**
   Inspect and restore it at your own discretion, e.g. via
   SQL Server Management Studio: *Restore Database* from device.
   Alternatively, create a database schema matching the data-access code.
2. Configure the connection string in
   `DVLD_DataAccess/clsDataAccessSettings.cs` to point at **your local**
   SQL Server instance and database.

> **Security note:** Do not hard-code real credentials, and never commit
> passwords or server details. The source currently contains hard-coded
> connection details whose validity is unknown; replace them for local use.
> If any credentials were real, rotate them and review repository history.

## Build & run

1. Open `DVLD-Forms/DVLD.sln` in Visual Studio.
2. Restore/attach the database (see above).
3. Set the connection string (see above).
4. Build and run the `DVLD` (WinForms) project.

## Known limitations & scope

- **Not tested here:** the application was not built, run, or verified in
  preparing this documentation.
- **Database contents unknown:** `DVLD.bak` is tracked in the repository,
  but its contents and sensitivity have not been verified — treat it as
  untrusted and potentially sensitive.
- **Hard-coded configuration:** the connection string lives in source code
  rather than a config file.
- **No automated tests** and no CI in the repository.
- **Password handling:** no evidence of password hashing was found in the
  source; no security hardening is claimed.
- The repository includes `bin/` and `obj/` build artifacts in source
  control, which are not required to build from the `.sln`.

## License

Released under the [MIT License](LICENSE).
