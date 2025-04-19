# evandarwin/phpbb-docker

[![Docker Pulls](https://img.shields.io/docker/pulls/evandarwin/phpbb.svg)](https://hub.docker.com/r/evandarwin/phpbb)
[![Docker Image Size](https://img.shields.io/docker/image-size/evandarwin/phpbb/latest)](https://hub.docker.com/r/evandarwin/phpbb)
[![License](https://img.shields.io/github/license/evandarwin/docker-phpbb)](https://github.com/evandarwin/docker-phpbb/blob/main/LICENSE)
[![Latest Release](https://img.shields.io/github/v/tag/evandarwin/docker-phpbb?label=version)](https://github.com/evandarwin/docker-phpbb/releases)
[![Docker Stars](https://img.shields.io/docker/stars/evandarwin/phpbb.svg)](https://hub.docker.com/r/evandarwin/phpbb)

A modern Docker container for phpBB with improved security, multiple database support, and automatic
configuration.

## Supported Tags

- `latest` - Latest phpBB version
- `<version>` - Specific phpBB version (e.g., `3.3.15`)
- `<major>.<minor>` - Latest patch version of a minor release (e.g., `3.3`)
- `<major>` - Latest minor.patch version of a major release (e.g., `3`)

All images are built on Alpine Linux for minimal size and maximum security.

## Why This Project Exists

This project was created to maintain a modern version of the phpBB container after Bitnami/Broadcom
deprecated their official phpBB container, which had become extremely outdated. This container
ensures you can continue to run phpBB in Docker with current, supported versions and improved
security.

## Required Environment Variables

This container requires the following environment variables to be set for proper operation:

- `PHPBB_USERNAME`: Username for the administrative user (required for first installation)
- `PHPBB_PASSWORD`: Password for the administrative user (required for first installation)

These variables are mandatory for generating the database during first initialization.

## Key Features

- **Auto-configuration** through environment variables
- **Pre-configured with PHP opcache** for improved performance
- **Nginx over Apache** for improved performance and security
- **Preconfigured with security best practices**
- **Support for MySQL, PostgreSQL, and SQLite** database drivers
- **Non-root execution** for enhanced container security
- **Persistent data storage** with volume mounting options
- **Custom PHP configuration** through environment variables

## Quick Start

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Awesome Forum" \
  -e PHPBB_DATABASE_HOST="db" \
  -e PHPBB_DATABASE_NAME="phpbb" \
  -e PHPBB_DATABASE_USER="phpbb" \
  -e PHPBB_DATABASE_PASSWORD="secret" \
  evandarwin/phpbb:latest
```

## Environment Variables

The following environment variables can be used to configure the phpBB installation:

| Variable                                           | Description                                                      | Default                      |
| -------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------- |
| **Forum Configuration**                            |                                                                  |                              |
| `PHPBB_FORUM_NAME`                                 | Name of the forum                                                | "My phpBB Forum"             |
| `PHPBB_FORUM_DESCRIPTION`                          | Description of the forum                                         | "A phpBB Forum"              |
| `PHPBB_LANGUAGE`                                   | Language for the phpBB installation                              | "en"                         |
| **Admin User**                                     |                                                                  |                              |
| `PHPBB_USERNAME`                                   | Username for the administrative user (Required)                  | "admin"                      |
| `PHPBB_PASSWORD`                                   | Password for the admin user (Required)                           | "" (auto-generated if empty) |
| `PHPBB_FIRST_NAME`                                 | First name of the admin user                                     | "Admin"                      |
| `PHPBB_LAST_NAME`                                  | Last name of the admin user                                      | "User"                       |
| `PHPBB_EMAIL`                                      | Admin user email                                                 | "admin@example.com"          |
| **Database Configuration**                         |                                                                  |                              |
| `PHPBB_DATABASE_DRIVER`                            | Database driver type (mysqli, postgres, sqlite3)                 | "sqlite3"                    |
| `PHPBB_DATABASE_HOST`                              | Database host address                                            | "localhost"                  |
| `PHPBB_DATABASE_PORT`                              | Database port                                                    | "" (uses default port)       |
| `PHPBB_DATABASE_NAME`                              | Database name                                                    | "phpbb"                      |
| `PHPBB_DATABASE_USER` or `PHPBB_DATABASE_USERNAME` | Database username                                                | "phpbb_user"                 |
| `PHPBB_DATABASE_PASSWORD` or `PHPBB_DATABASE_PASS` | Database password                                                | ""                           |
| `PHPBB_DATABASE_SQLITE_PATH`                       | Full path for SQLite database file (used when driver is sqlite3) | "/opt/phpbb/phpbb.sqlite"    |
| `PHPBB_TABLE_PREFIX`                               | Prefix for database tables                                       | "phpbb\_"                    |
| **Email/SMTP Configuration**                       |                                                                  |                              |
| `SMTP_HOST`                                        | SMTP server address                                              | "" (disabled)                |
| `SMTP_PORT`                                        | SMTP server port                                                 | "25"                         |
| `SMTP_USER`                                        | SMTP username                                                    | ""                           |
| `SMTP_PASSWORD`                                    | SMTP password                                                    | ""                           |
| `SMTP_AUTH`                                        | SMTP authentication method                                       | ""                           |
| `SMTP_PROTOCOL`                                    | SMTP protocol                                                    | ""                           |
| **Server Configuration**                           |                                                                  |                              |
| `SERVER_PROTOCOL`                                  | Server protocol (http:// or https://)                            | "http://"                    |
| `SERVER_NAME`                                      | Server hostname                                                  | "localhost"                  |
| `SERVER_PORT`                                      | Server port                                                      | "80"                         |
| `SCRIPT_PATH`                                      | Base path for the phpBB installation                             | "/"                          |
| `COOKIE_SECURE`                                    | Whether to use secure cookies                                    | "false"                      |
| **PHP Configuration**                              |                                                                  |                              |
| `PHP_MEMORY_LIMIT`                                 | PHP memory limit                                                 | "128M"                       |
| `PHP_CUSTOM_INI`                                   | Custom PHP.ini directives (multiple lines)                       | ""                           |

## Data Persistence

There are two main approaches to persist your phpBB data:

### 1. Volume Mounting

You can mount a volume at `/opt/phpbb` to persist all phpBB files, which includes your forum's data,
themes, extensions, and configurations:

```bash
docker run -d \
  -p 8080:8080 \
  -v phpbb_data:/opt/phpbb \
  -e PHPBB_DATABASE_HOST="db" \
  -e PHPBB_DATABASE_NAME="phpbb" \
  -e PHPBB_DATABASE_USER="phpbb" \
  -e PHPBB_DATABASE_PASSWORD="secret" \
  evandarwin/phpbb:latest
```

This approach is recommended for most users as it persists all phpBB files.

### 2. Selectively Mount Important Directories

Alternatively, you can mount specific directories that contain important user data:

```bash
docker run -d \
  -p 8080:8080 \
  -v phpbb_config:/opt/phpbb/config \
  -v phpbb_store:/opt/phpbb/store \
  -v phpbb_files:/opt/phpbb/files \
  -v phpbb_images:/opt/phpbb/images \
  -v phpbb_ext:/opt/phpbb/ext \
  -e PHPBB_DATABASE_HOST="db" \
  -e PHPBB_DATABASE_NAME="phpbb" \
  -e PHPBB_DATABASE_USER="phpbb" \
  -e PHPBB_DATABASE_PASSWORD="secret" \
  evandarwin/phpbb:latest
```

This approach gives you more granular control over which parts of phpBB are persisted.

## Custom PHP Configuration

You can customize your PHP settings by providing PHP.ini directives through the `PHP_CUSTOM_INI`
environment variable:

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_DATABASE_HOST="mysql_container" \
  -e PHPBB_DATABASE_NAME="phpbb_db" \
  -e PHPBB_DATABASE_USER="phpbb_user" \
  -e PHPBB_DATABASE_PASSWORD="secure_password" \
  -e PHP_CUSTOM_INI="upload_max_filesize = 64M
post_max_size = 64M
memory_limit = 256M
max_execution_time = 60" \
  evandarwin/phpbb:latest
```

These directives will be appended to the PHP.ini file during container startup.

## Examples

### Basic Setup with MySQL

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_DATABASE_DRIVER="mysqli" \
  -e PHPBB_DATABASE_HOST="mysql_container" \
  -e PHPBB_DATABASE_NAME="phpbb_db" \
  -e PHPBB_DATABASE_USER="phpbb_user" \
  -e PHPBB_DATABASE_PASSWORD="secure_password" \
  evandarwin/phpbb:latest
```

### Setup with PostgreSQL

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_DATABASE_DRIVER="postgres" \
  -e PHPBB_DATABASE_HOST="postgres_container" \
  -e PHPBB_DATABASE_NAME="phpbb_db" \
  -e PHPBB_DATABASE_USER="phpbb_user" \
  -e PHPBB_DATABASE_PASSWORD="secure_password" \
  evandarwin/phpbb:latest
```

### Setup with SQLite and Data Persistence

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_DATABASE_DRIVER="sqlite3" \
  -e PHPBB_DATABASE_SQLITE_PATH="/opt/phpbb/data/phpbb.sqlite3" \
  -v phpbb_data:/opt/phpbb \
  evandarwin/phpbb:latest
```

### Setup with Email Configuration

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_EMAIL="admin@example.com" \
  -e SMTP_HOST="smtp.example.com" \
  -e SMTP_PORT="587" \
  -e SMTP_USER="smtp_user" \
  -e SMTP_PASSWORD="smtp_password" \
  -e PHPBB_DATABASE_HOST="mysql_container" \
  -e PHPBB_DATABASE_NAME="phpbb_db" \
  -e PHPBB_DATABASE_USER="phpbb_user" \
  -e PHPBB_DATABASE_PASSWORD="secure_password" \
  evandarwin/phpbb:latest
```

## Building Your Own Images

You can build your own phpBB Docker images using the provided build script:

```bash
# Build the latest phpBB with PHP 8.4
./scripts/build.sh

# Build a specific phpBB version
PHPBB_VERSION=3.3.10 ./scripts/build.sh
```

The build script automatically fetches the latest phpBB release version from GitHub if you don't
specify a version.

## Docker Compose Example

Here's a complete example using Docker Compose with MySQL:

```yaml
version: '3.8'

services:
  phpbb:
    image: evandarwin/phpbb:latest
    ports:
      - '8080:8080'
    environment:
      - PHPBB_FORUM_NAME=My Amazing Forum
      - PHPBB_FORUM_DESCRIPTION=Welcome to my phpBB forum
      - PHPBB_USERNAME=admin
      - PHPBB_PASSWORD=secure_password
      - PHPBB_EMAIL=admin@example.com
      - PHPBB_DATABASE_DRIVER=mysqli
      - PHPBB_DATABASE_HOST=mysql
      - PHPBB_DATABASE_NAME=phpbb
      - PHPBB_DATABASE_USER=phpbb
      - PHPBB_DATABASE_PASSWORD=mysql_password
      - SERVER_NAME=forums.example.com
      - COOKIE_SECURE=false
    volumes:
      - phpbb_data:/opt/phpbb
    depends_on:
      - mysql
    restart: unless-stopped
    healthcheck:
      test: ['CMD', 'curl', '-f', 'http://localhost:8080/']
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 30s

  mysql:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=root_password
      - MYSQL_DATABASE=phpbb
      - MYSQL_USER=phpbb
      - MYSQL_PASSWORD=mysql_password
    volumes:
      - mysql_data:/var/lib/mysql
    restart: unless-stopped
    healthcheck:
      test:
        ['CMD', 'mysqladmin', 'ping', '-h', 'localhost', '-u', 'root', '-p${MYSQL_ROOT_PASSWORD}']
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 30s

volumes:
  phpbb_data:
  mysql_data:
```

For PostgreSQL, replace the MySQL service with:

```yaml
postgres:
  image: postgres:15
  environment:
    - POSTGRES_PASSWORD=postgres_password
    - POSTGRES_USER=phpbb
    - POSTGRES_DB=phpbb
  volumes:
    - postgres_data:/var/lib/postgresql/data
  restart: unless-stopped
  healthcheck:
    test: ['CMD', 'pg_isready', '-U', 'phpbb']
    interval: 10s
    timeout: 5s
    retries: 3
    start_period: 30s
```

## Using Behind a Reverse Proxy

When using this container behind a reverse proxy like Traefik or Nginx:

1. Set the `SERVER_NAME` to your domain name
2. Set `SERVER_PROTOCOL` to `https://` if using SSL/TLS
3. Set `COOKIE_SECURE=true` for secure cookies

Example Docker Compose configuration with Traefik:

```yaml
services:
  phpbb:
    image: evandarwin/phpbb:latest
    environment:
      - SERVER_NAME=forums.example.com
      - SERVER_PROTOCOL=https://
      - COOKIE_SECURE=true
      # Other configuration...
    labels:
      - 'traefik.enable=true'
      - 'traefik.http.routers.phpbb.rule=Host(`forums.example.com`)'
      - 'traefik.http.routers.phpbb.entrypoints=websecure'
      - 'traefik.http.routers.phpbb.tls=true'
```

## Security

This Docker image runs phpBB as a non-root user for improved security and implements additional
security best practices.

## Security Features

This container implements several security best practices:

### Web Server Security

- **Non-root user**: The container runs as a non-privileged `phpbb` user
- **Rate limiting**: Login pages are protected against brute force attacks
- **Enhanced security headers**: Includes Content-Security-Policy, X-Frame-Options, and other
  security headers
- **Disabled PHP functions**: Dangerous PHP functions like `exec` and `shell_exec` are disabled
- **Secure file permissions**: Proper permission settings for all phpBB files and directories

### PHP Security

- PHP is configured with:
  - `open_basedir` restrictions to limit file access
  - Disabled exposure of PHP version information
  - Memory limits and execution time restrictions
  - File upload size limitations
  - PHP 8.3+ for latest security patches

### Nginx Configuration

- Protected sensitive directories and files from direct access
- Properly configured static file caching
- IP forwarding configured for proper logging behind proxies
- Secure TLS configuration options available

## SQLite Database Security

### IMPORTANT: Secure Storage of SQLite Databases

When using SQLite as your database driver, it's **critically important** to store your database file
outside of the publicly accessible web directory.

#### Security Risks

Storing SQLite database files in publicly accessible locations presents severe security risks:

- **Data Theft**: If an attacker can directly download your .sqlite or .db file, they gain access to
  all forum data including user credentials
- **Data Manipulation**: Unauthorized modification of your database could lead to account takeovers
- **Privacy Violations**: Personal user information could be exposed, potentially violating privacy
  laws

#### Recommendations

1. Store your SQLite database in a directory that is:

   - NOT accessible from the web
   - NOT inside the `/opt/phpbb` public directory structure
   - Properly permission-restricted

2. When using the `PHPBB_DATABASE_SQLITE_PATH` environment variable:

   - Use a path like `/var/lib/phpbb/data/phpbb.sqlite3`
   - NEVER use a path within the phpBB root directory
   - The container is configured to reject SQLite paths that contain the phpBB root directory

3. Use volume mounting to persist your SQLite database:

```bash
docker run -d \
  -p 8080:8080 \
  -e PHPBB_FORUM_NAME="My Community" \
  -e PHPBB_DATABASE_DRIVER="sqlite3" \
  -e PHPBB_DATABASE_SQLITE_PATH="/var/lib/phpbb/data/phpbb.sqlite3" \
  -v phpbb_sqlite_data:/var/lib/phpbb/data \
  -v phpbb_data:/opt/phpbb \
  evandarwin/phpbb:latest
```

#### Additional Protection

The nginx configuration in this container will automatically block access to any .sqlite or .db
files, but this should be considered a last line of defense rather than your primary security
measure.

## Container Health Monitoring

This Docker image includes a built-in health check that verifies the web server is responding
properly. The health check:

- Runs every 30 seconds after a 30-second startup period
- Verifies that the Nginx web server is running and responding to HTTP requests
- Will automatically mark the container as unhealthy if the web server stops responding

This is particularly useful when using the container with orchestration systems like Docker Swarm,
Kubernetes, or Docker Compose with health checks.

## Troubleshooting

### Common Issues

1. **Database Connection Errors**:

   - Verify your database credentials are correct
   - Ensure the database server is accessible from the phpBB container
   - For external databases, check network connectivity and firewall rules

2. **Permission Issues**:

   - If mounting volumes, ensure they have the correct ownership and permissions
   - The container uses a non-root user with UID/GID different from the host

3. **PHP Configuration**:

   - If you need to adjust PHP settings beyond what's available through environment variables, you
     can mount a custom php.ini file

4. **Nginx Logs**:
   - Container logs include nginx access and error logs
   - You can view them with `docker logs <container_name>`

### Accessing Logs

All logs are forwarded to the Docker logging system:

```bash
# View all logs
docker logs <container_name>

# View only recent logs
docker logs --tail 100 <container_name>

# Follow logs in real-time
docker logs -f <container_name>
```

## Automated Builds with GitHub Actions

This project uses GitHub Actions to automatically build and publish Docker images to Docker Hub. The
workflow includes:

- Building multi-architecture images (amd64, arm64)
- Signing images with Cosign for supply chain security
- Generating Software Bill of Materials (SBOM)
- Vulnerability scanning with Trivy
- Automated version detection from phpBB releases

### Setting up GitHub Secrets

To enable the automated build workflow, you need to set up the following GitHub repository secrets:

1. `DOCKER_HUB_USERNAME` - Your Docker Hub username
2. `DOCKER_HUB_TOKEN` - A Docker Hub access token (not your password)

You can create a Docker Hub access token by going to your Docker Hub account settings > Security >
New Access Token.

### Verifying Image Signatures

Images built by this workflow are automatically signed using Cosign. You can verify the signature of
an image using:

```bash
cosign verify --key cosign.pub evandarwin/phpbb:latest
```

## Contributing

Contributions to improve this Docker image are welcome! Please submit issues or pull requests to the
project repository.
