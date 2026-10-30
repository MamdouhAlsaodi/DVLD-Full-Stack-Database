# DVLD System Overview — Text Companion

This document summarizes the DVLD (Driver & Vehicle License Department)
desktop application. It accompanies `DVLD-System-Overview.pptx`.

## What it is

DVLD is a Windows Forms (desktop) application that models the workflow of a
driving license department: registering people, opening license
applications, scheduling and taking vision/theory/practical tests, issuing,
renewing, or replacing licenses, and detaining/releasing licenses.

## Solution structure

Single solution: `DVLD-Forms/DVLD.sln` — three projects, all
**.NET Framework 4.8**:

1. **DVLD (WinExe, WinForms)** — `DVLD-Forms/` — all UI forms and user
   controls, organized by feature (People, Users, Applications, Licenses,
   Tests, Drivers, Login, Global Classes).
2. **DVLD_Buisness (class library)** — `DVLD_Buisness/` — domain classes
   such as `clsPerson`, `clsUser`, `clsApplication`,
   `clsApplicationType`, `clsLocalDrivingLicenseApplication`, `clsLicense`,
   `clsLicenseClass`, `clsTest`, `clsTestAppointment`, `clsTestType`,
   `clsDriver`, `clsDetainedLicense`, `clsInternationalLicense`,
   `clsCountry`.
3. **DVLD_DataAccess (class library)** — `DVLD_DataAccess/` — one data
   class per entity, using raw **ADO.NET** (`SqlConnection`,
   `SqlCommand`, `SqlDataAdapter`, `DataTable`) against SQL Server.
   Connection string is defined in `clsDataAccessSettings.cs` and must be
   edited to match your local SQL Server instance.

## Domain model (entities inferred from classes/forms)

- **Person** — base personal record (with `ctrlPersonCard` and
  add/update/find forms).
- **User** — application account linked to a person; login, list,
  add/update, change password.
- **Application / ApplicationType** — generic application record and its
  fee-bearing types.
- **LicenseClass** — categories of driving license.
- **LocalDrivingLicenseApplication** — an application to obtain a license
  of a given class; drives the test pipeline.
- **TestType / TestAppointment / Test** — the three-stage testing
  workflow (scheduling, taking, recording pass/fail).
- **License / InternationalLicense** — issued licenses, including
  renewal and replacement (lost/damaged) flows.
- **Driver / DetainedLicense** — driver registry and license
  detention/release.

## Typical workflow

1. Log in.
2. Register or find a **person**.
3. Create a **local driving-license application** for a license class.
4. For each **test type**, schedule an appointment, take the test, and
   record the result; the next stage unlocks on passing.
5. After passing all tests, **issue the first-time driver license**.
6. Later: **renew**, **replace** (lost/damaged), **detain/release**, or
   **issue an international license**; view full **license history**.

## Setup summary

- Windows + Visual Studio + .NET Framework 4.8 targeting pack
- SQL Server instance hosting the `DVLD` database (restore from
  `DVLD.bak` — contents unverified — or create a matching schema)
- Update the connection string in
  `DVLD_DataAccess/clsDataAccessSettings.cs` (use placeholders/your own
  local credentials; do not commit real secrets)

## Honest limitations

- No automated tests are included.
- `DVLD.bak` contents unreviewed; treat as untrusted and potentially sensitive.
- Connection string is hard-coded in source.
- No evidence of password hashing in source; no security hardening
  claimed.
- Project name contains a typo ("Buisness") that is part of the assembly
  name and is kept as-is.
