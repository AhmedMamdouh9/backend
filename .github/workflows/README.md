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

1. **Checkout code**  
   Checks out the repository code using `actions/checkout@v2`.

2. **Create .env file**  
   Copies `.env.example` to `.env` and adds environment variables from GitHub secrets.

3. **Set up Docker Buildx**  
   Sets up Docker Buildx for efficient image building.

4. **Log in to DockerHub**  
   Logs in to DockerHub with credentials from GitHub secrets.

5. **Build Docker image**  
   Builds the Docker image for the Laravel application, tagged as `my-laravel-app:latest`.

6. **Set version**  
   Sets a unique version for the image based on the current date and time.

7. **Tag Docker image with version**  
   Tags the image with the generated version to prevent overwriting.

8. **Push Docker image to DockerHub**  
   Pushes the tagged image to DockerHub for deployment.



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

![Screenshot from 2024-10-20 14-17-04](https://github.com/user-attachments/assets/558a506a-ca15-40f4-9605-144f268ca24c)

**Push Docker image to DockerHub**

![Screenshot from 2024-10-20 14-15-29](https://github.com/user-attachments/assets/8481b6a4-a943-49be-ad5b-23c1c38b903b)


## Nginx Configuration: Default Server

This Nginx configuration file sets up a default server to serve a Laravel application. It listens on port 80 and serves files from the `/var/www/html/public` directory.

### Configuration File

The configuration is defined in `default.conf`.

### Server Block

- **Listen**: The server listens on port 80.
- **Server Name**: The server name is set to `localhost`.
- **Root**: The root directory is set to `/var/www/html/public`.
- **Index**: The index files are `index.php`, `index.html`, and `index.htm`.
