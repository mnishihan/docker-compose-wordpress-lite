# Light weight docker compose template for wordpress

## Features

- Built with [alpine linux](https://www.alpinelinux.org/) based images that supports multiple architecture (tested with amd64 and arm64v8). So, both Intel & Apple Silicon processors are supported and right image will be used by docker to enable maximum performance.

- Database management support with [Adminer](https://www.adminer.org/) is also included. After running `docker-compose up` command, navigate to http://localhost:8080/adminer.php (if the default `PORT` is not changed in `.env` file) to access adminer and use `db` as host and `root` as user along with the password set in `DB_ROOT_PASSWORD` in `.env` file.

- [WP-CLI](https://wp-cli.org/) is included, to enable easy management of wordpress site using command line.

## Requirements

- Make sure you have the latest versions of **[Docker Engine](https://docs.docker.com/get-docker/)** and **[Docker Compose](https://docs.docker.com/compose/install/)** or  **[Docker Desktop](https://docs.docker.com/desktop/)** (which includes both Docker Engine and Docker Compose) installed in your machine.

    Also, make sure to [add your user to the `docker` group](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user) when using Linux.

- Clone this repository or copy the files from this repository into a new folder.

    ```sh
    git clone https://github.com/mnishihan/docker-compose-wordpress-lite.git my_awesome_wp_site
    cd my_awesome_wp_site
    ```
## Configuration

1. Copy the example environment file into `.env`

    ```sh
    cp env.example .env
    ```

2. Edit the `.env` file to change the default IP address, MySQL root password and WordPress database name if needed.

## Project setup

1. In terminal execute following commands:

    ```sh
    docker-compose up --build
    ```

2. When every service has been initialized, open another terminal and execute following command to install wordpress and copy the generated password

    ```sh
    docker-compose run --rm wpcli wp core install --url=http://localhost:8080 --title="Hello Wordpress" --admin_user=admin --admin_email=admin@my_awesome_wp_site.com --admin_password=admin
    ```

    For convenience, you can add an alias for `wpcli` to your shell profile:

    ```sh
    alias wp="docker-compose run --rm wpcli wp"
    ```

    After adding the alias and restarting the terminal, you can use `wp` commands directly in your wordpress installation, when you are within the project folder. For example, the above `core install` command can be written as:

    ```sh
    wp core install --url=http://localhost:8080 --title="Hello Wordpress" --admin_user=admin --admin_email=admin@my_awesome_wp_site.com --admin_password=admin
    ```

3. Open http://localhost:8080/wp-login.php in a browser and login using `admin` as both user name & password fields.

## Development

1. If you want to develop a plugin, create a folder in `./src/plugins/` directory, uncomment following line in `volumes` section for `wp` service and replace `plugin-name` with the folder name.

    ```docker
    #- ./src/plugins/plugin-name/:/var/www/html/wp-content/plugins/plugin-name # Plugin development
    ```

2. If you want to develop a theme, create a folder in `./src/themes/` directory, uncomment following line in `volumes` section for `wp` service and replace `theme-name` with the folder name.

    ```docker
    #- ./src/plugins/plugin-name/:/var/www/html/wp-content/themes/theme-name # Theme development
    ```

3. Start needed services using following command in a terminal and do the development.

    ```sh
    docker-compose up # or docker-compose start
    ```

## Alternatives

https://github.com/TrafeX/docker-wordpress

## Credits

This project is heavily inspired by [WPDC - WordPress Docker Compose](https://github.com/nezhar/wordpress-docker-compose) project with necessary customization.

## License

This project is licensed under The MIT License (MIT). For further details, please review the `LICENSE` file included in this project.

