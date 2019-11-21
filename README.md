# Example Laravel on Docker

I can haz Laravel? When you just want to start working on a Laravel 6 project with Docker.

Here's an example of Laravel running inside Docker with:

- SSL (locally)
- XDebug
- MySQL database files left outside of the container (in order to persist data)
- PHPUnit???

## Requirements

- `composer` installed
- Docker installed
- Nothing already running on port 443 or port 3307 on your localhost

## Installation

1. Clone this repository
1. `cd` into the `laravel-docker` directory
1. `composer create-project --prefer-dist laravel/laravel`
1. `sed -i -e 's!DB_PASSWORD=!DB_PASSWORD=laravel!g' ./laravel/.env && sed -i -e 's!DB_HOST=127.0.0.1!DB_HOST=laravel-db!g' ./laravel/.env && rm ./laravel/.env-e`
1. If it's not already running, start Docker
1. `docker-compose build` (Docker will complain about a few non-important things such as the `ServerName` domain name not being set)
1. `docker-compose up`
1. Open a new tab in your shell and `cd` to the project root - you can now run your development commands (such as `php artisan`) from here (see below)
1. `docker-compose exec laravel php artisan migrate` to migrate your database and confirm connectivity with your Docker database (`laravel-db`)

Great, from now on **to run the application**, just use `docker-compose up`

### PHPStorm Setup (XDebug)

1.

### Logs

Logs such as Apache error logs can be found at: `./docker/logs`

**TODO:** Add the MySQL logs here too!

## Running commands inside the Docker containers

You'll probably want to run commands such as `php artisan` and others inside your Laravel container.

You can either run the commands directly from the host command line, or you can SSH into the Docker container and run them there.

To run from the host: `docker-compose exec laravel <shell command to run here>`

Or, SSH to your Laravel container: `docker exec -u 0 -it laravel bash` and run all the commands your heart desires.

---

In order to SSH to the database container, use: `docker exec -u 0 -it laravel-db bash`
