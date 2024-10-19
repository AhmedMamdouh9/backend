## GitHub Action: Deploy Laravel App

This GitHub Action workflow automates the deployment process for the Laravel application. It is triggered on pushes and pull requests to the `main` branch, as well as manually via the workflow dispatch event.

## Table of Contents

- [GitHub Action: Deploy Laravel App](#github-action-deploy-laravel-app)
- [Table of Contents](#table-of-contents)
  - [Workflow File](#workflow-file)
  - [Triggers](#triggers)
  - [Jobs](#jobs)
    - [Build](#build)
      - [Steps](#steps)
  - [Environment Variables](#environment-variables)
  - [Example Usage](#example-usage)
- [Nginx Configuration: Default Server](#nginx-configuration-default-server)
  - [Configuration File](#configuration-file)
  - [Server Block](#server-block)
  - [Location Blocks](#location-blocks)
    - [Root Location](#root-location)
    - [PHP Files](#php-files)
    - [Hidden Files](#hidden-files)
  - [Example Usage](#example-usage-1)
### Workflow File

The workflow is defined in `.github/workflows/deploy_backend.yml`.

### Triggers

- **Push**: Triggered on pushes to the `main` branch.
- **Pull Request**: Triggered on pull requests to the `main` branch.
- **Manual Dispatch**: Can be manually triggered via the GitHub Actions interface.

### Jobs

#### Build

- **Runs on**: `ubuntu-latest`

##### Steps

1. **Checkout code**: Uses the `actions/checkout@v2` action to check out the repository code.
2. **Create .env file**: Copies `.env.example` to `.env` and appends environment variables from GitHub secrets.
3. **Set up Docker Buildx**: Uses the `docker/setup-buildx-action@v2` action to set up Docker Buildx.
4. **Log in to DockerHub**: Uses the `docker/login-action@v2` action to log in to DockerHub using credentials stored in GitHub secrets.
5. **Build Docker image**: Builds the Docker image for the Laravel application.

### Environment Variables

The following environment variables are set using GitHub secrets:

- `APP_KEY`
- `MYSQL_HOST`
- `MYSQL_DATABASE`
- `MYSQL_USER`
- `MYSQL_PASSWORD`
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`

### Example Usage

To trigger this workflow, push changes to the `main` branch or create a pull request targeting the `main` branch. You can also manually trigger the workflow from the GitHub Actions interface.

![Screenshot_20241019_213552](https://github.com/user-attachments/assets/afe6dcb9-e7fd-41df-a195-4805f111db03)

## Nginx Configuration: Default Server

This Nginx configuration file sets up a default server to serve a Laravel application. It listens on port 80 and serves files from the `/var/www/html/public` directory.

### Configuration File

The configuration is defined in `default.conf`.

### Server Block

- **Listen**: The server listens on port 80.
- **Server Name**: The server name is set to `localhost`.
- **Root**: The root directory is set to `/var/www/html/public`.
- **Index**: The index files are `index.php`, `index.html`, and `index.htm`.

### Location Blocks

#### Root Location

- **Path**: `/`
- **Try Files**: Tries to serve the requested URI, then the URI with a trailing slash, and finally falls back to `index.php` with the query string.

#### PHP Files

- **Path**: `~ \.php$`
- **Include**: Includes the `snippets/fastcgi-php.conf` configuration.
- **FastCGI Pass**: Passes PHP requests to FastCGI at `127.0.0.1:9000`.
- **FastCGI Param**: Sets the `SCRIPT_FILENAME` parameter to the document root and script name.
- **Include**: Includes the `fastcgi_params` configuration.

#### Hidden Files

- **Path**: `~ /\.ht`
- **Deny**: Denies access to all `.ht` files.

### Example Usage

To use this configuration, place it in your Nginx configuration directory (e.g., `/etc/nginx/sites-available/`) and create a symbolic link to it in the `sites-enabled` directory. Then, restart Nginx to apply the changes.

