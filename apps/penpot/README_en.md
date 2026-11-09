# Penpot

Penpot is an open-source design and prototyping platform for product teams, with vector editing, interactive prototypes, design systems, and real-time collaboration.

This app uses the official **2.18.6** images and includes the frontend, backend, exporter, admin console, MCP, PostgreSQL 15, and Valkey 8.1. Both amd64 and arm64 are supported.

## Installation

1. Refresh the local app list in 1Panel and install **Penpot**.
2. The default HTTP port is `9001`. Set the public URL to the full address used in your browser, such as `http://192.168.1.100:9001` or `https://penpot.example.com`. Do not use an internal container address. Update this URL whenever the port or domain changes.
3. Keep the generated database password and provide an application secret key. Generate a key on the 1Panel server with:

   ```bash
   python3 -c 'import secrets; print(secrets.token_urlsafe(64))'
   ```

4. Wait for database initialization and migrations to finish, then open the configured public URL and register your first account. There are no default login credentials.

## Feature flags and email

The default flags allow registration without email verification and support HTTP login. The admin console, MCP, and administrative command interface are enabled. The command interface is reachable only within the app's dedicated network.

SMTP is disabled by default, so registration and login work without a mail server. To invite new users or send password reset emails, configure the SMTP host, port, username, password, and sender address, then append `enable-smtp` to the feature flags. Port `587` typically uses STARTTLS; port `465` typically uses SSL. Follow your mail provider's settings.

To require email verification, configure working SMTP and remove `disable-email-verification`. To disable public registration, append `disable-registration`.

## Domain and HTTPS

Create a reverse proxy in 1Panel pointing to the app's HTTP port and enable WebSocket forwarding. When using HTTPS, set the public URL to the corresponding `https://` address and remove `disable-secure-session-cookies` from the feature flags.

For large imports, set the proxy request body limit to at least `350 MiB` (`367001600` bytes).

## Data and upgrades

- PostgreSQL data is stored in the Compose project's `penpot_postgres_v15` named volume.
- Uploaded images, fonts, and design assets are stored in the project's `penpot_assets` named volume, shared by the frontend and backend.
- Before upgrading, back up both the database and assets volume, and save the app's environment configuration. Backing up only the app installation directory does not include the named volumes. Follow the [official backup instructions](https://help.penpot.app/technical-guide/getting-started/docker/#backup-penpot).
- Keep the database password and secret key unchanged, and follow the official instructions when upgrading between versions.

## Official links

- [GitHub repository](https://github.com/penpot/penpot)
- [Docker installation guide](https://help.penpot.app/technical-guide/getting-started/docker/)
- [Configuration reference](https://help.penpot.app/technical-guide/configuration/)
