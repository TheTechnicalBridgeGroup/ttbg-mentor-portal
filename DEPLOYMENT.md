# TTBG Portal Deployment: Render + PlanetScale Postgres

Use PlanetScale **Postgres**, not Vitess/MySQL.

## PlanetScale

1. Create a Postgres database named `ttbg_mentor_portal`.
2. Choose a region close to the Render service.
3. Choose the smallest suitable Single Node configuration.
4. Create a user-defined role named `ttbg_app`.
5. Enable `pg_read_all_data` and `pg_write_all_data`.
6. In PlanetScale's SQL web console, grant initial schema creation permission:

```sql
GRANT CREATE ON SCHEMA public TO "ttbg_app";
```

Use the exact role name PlanetScale displays if it differs.

From **Connect**, choose the `main` branch and `ttbg_app` role. Copy the
pooled/PgBouncer PostgreSQL URL, normally using port `6432`. Keep the SSL
parameters in the URL.

Example shape:

```text
postgresql://USER:PASSWORD@HOST:6432/postgres?sslmode=verify-full&sslrootcert=system
```

Do not commit this value to GitHub.

## GitHub

Create a blank private repository named `ttbg-mentor-portal`.

```cmd
git init
git add .
git status
git commit -m "Deploy TTBG mentor portal"
git branch -M main
git remote add origin YOUR_PRIVATE_GITHUB_REPOSITORY_URL
git push -u origin main
```

Confirm `.env`, `.venv`, `instance`, and local database files are not staged.

## Render

1. Select **New > Blueprint**.
2. Connect the private GitHub repository.
3. Render detects `render.yaml`.
4. Enter:

```text
DATABASE_URL=<PlanetScale pooled PostgreSQL URL>
ADMIN_EMAIL=<portal administrator email>
ADMIN_PASSWORD=<new strong portal password>
```

5. Deploy.

## Test

Test:

```text
/healthz
/apply
/login
/admin
```

Then submit a fake application, review it, update its status and notes, sign
out, sign back in, and confirm the data remains.

Do not replace the live Squarespace form until this online test passes.
