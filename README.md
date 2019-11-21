# Example Laravel on Docker

I can haz Laravel? When you just want to start working on a Laravel 6 project with Docker.

Here's an example of Laravel running inside Docker with:

- SSL (locally)
- XDebug
- MySQL database files left outside of the container (in order to persist data when the container is shutdown)
- PHPUnit???

## Requirements

- `composer` installed
- Docker installed and running
- Nothing already running on port 443 or port 3307 on your localhost
- The Xdebug browser extension installed

## Installation

Instructions are for the Mac OS.

1. Clone this repository: `git clone git@github.com:jonathanbell/laravel-docker.git`
1. `cd` into the `laravel-docker` project directory
1. `composer create-project --prefer-dist laravel/laravel`
1. `sed -i -e 's!DB_PASSWORD=!DB_PASSWORD=laravel!g' ./laravel/.env && sed -i -e 's!DB_HOST=127.0.0.1!DB_HOST=laravel-db!g' ./laravel/.env && rm ./laravel/.env-e`
1. If it's not already running, start Docker
1. `docker-compose build` (Docker will complain about a few non-important things such as the `ServerName` domain name not being set)
1. `docker-compose up`
1. Open a new tab in your shell and `cd` to the project root - you can now run your development commands (such as `php artisan`) from here (see below)
1. In the new tab: `docker-compose exec laravel php artisan migrate` in order to migrate your database and confirm connectivity with your Docker database (`laravel-db`)
1. (optional) `rm -rf ./.git` in order to remove the laravel-docker repository - you can now initialize a new Git repository inside the `laravel` folder

Great, from now on **to run the application**, just use: `docker-compose up`

You can now visit: <https://localhost> (accept the security warning)

To kill the application, just press `cntrl + C` from the tab running the Docker containers.

### Setup PHPStorm (XDebug)

1. Open the `laravel` folder in PHPStorm
1. Go to: `Preferences -> Languages & Frameworks -> PHP` and set a new CLI Interpreter using `From Docker, Vagrant...`
1. Choose the `Docker` from the radio buttons
1. Select `New...` and configure a new Docker server
1. Choose `Connect to Docker daemon` with `Docker for Mac`
1. PHPStorm checks the PHP installation on Docker and finds the debugger
1. Enable the Xdebug browser extension and visit <https://localhost>
1. The first time you attempt to connect PHPStorm and Xdebug you will be asked to map the project to the server path - the defaults should workout just fine (just click OK)
1. You've got debugging working in PHPStorm via Docker!

### Logs

Logs such as Apache error logs can be found at: `./docker/logs`

**TODO:** Add the MySQL logs here too!

## Running commands inside the Docker containers

You'll probably want to run commands such as `php artisan` and others inside your Laravel container.

You can either run the commands directly from the host command line, or you can SSH into the Docker container and run them there.

To run from the host: `docker-compose exec laravel <shell command to run here>`

Or, SSH to your Laravel container (as root): `docker exec -u 0 -it laravel bash` and run all the commands your heart desires there.
