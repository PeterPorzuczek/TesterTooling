# Tester Tooling 2023

This repository, serves as a dedicated location for the storage and management of docker configuration files necessary for the deployment of TestLink and Mantis docker images.

The primary purpose of the repository is to allow users to efficiently set up and run TestLink and Mantis within docker containers. This streamlined setup facilitates a smoother and faster implementation of these tools, ensuring less downtime and quicker start times for testing efforts.

## Testlink

TestLink https://testlink.org/ is an open-source test management tool that provides support for test cases, test suites, test plans, and user management. It is often used by Quality Assurance (QA) teams to manage, track, and organize their testing efforts.

### Build & Run

Run commands inside testlink folder.

```bash
cd testlink
docker build -t testlink-app .
docker-compose up -d
```

### Setup

Testlink has the web based installer. Run the docker container and access to `localhost:3000/testlink` endpoint in the web browser. Use user `root` and `MYSQL_ROOT_PASSWORD` from `doceker-compose.yml`, the Database host is `db` as the name of container defined in a file.

Configuration:
  * Debian 10
  * testlink 1.9.20
  * apache
  * dumb-init 1.2.0
  * EXPOSED @ 3000 port
  * ENTRYPOINT: /usr/bin/dumb-init
  * ENV in Dockerfile

### Screenshots

<div><img src="https://i.imgur.com/XCdL6Wa.png" width="50%"></div>

<div><img src="https://i.imgur.com/PWdsYqo.png" width="50%"></div>

<div><img src="https://i.imgur.com/9U566lB.png" width="50%"></div>

## Mantis

Mantis Bug Tracker https://www.mantisbt.org/ also known as MantisBT, is a free, open-source bug tracking software that helps teams in tracking bugs, issues, and tasks throughout their software development process. It's a web-based application that is implemented in PHP and is typically used for project management, bug tracking, and issue tracking.

### Build & Run

Run commands inside mantis folder.

```bash
cd mantis
docker build -t mantis-app .
docker-compose up -d
```

### Setup

* Navigate to `localhost:8989/admin/install.php` in your browser and follow the installation guide. The default settings should be appropriate for most setups.  Faster solution is to setup user to mantis with password mantis.
* During installation, you may encounter a warning labeled Config File Exists but Database does not. This can be overlooked as you continue with the installation.
* After the installation is complete, log in with the default credentials as `administrator/root`. You can then adjust the settings according to your needs, typically, you'd create a new admin account and deactivate the built-in "administrator" account.
* Ensure to evaluate the MantisBT's internal checks located at `localhost:8989/admin/`. It's worth noting that some warnings are expected due to existing issues in MantisBT, such as the magic quotes warning ([#26964](https://www.mantisbt.org/bugs/view.php?id=26964)) and the "folder outside of web root" warning ([#21584](https://mantisbt.org/bugs/view.php?id=21584)).
* Once your system is ready for production, you can disable the `MANTIS_ENABLE_ADMIN` environment variable or set its value to 0. This action will eliminate the "admin" folder from the installation.
* For other details go to [official documentation](https://www.mantisbt.org/docs/master/en-US/Admin_Guide/html-desktop/#admin.install.new)
* If you require further customization in the configuration, create a `config_inc_addon.php` file. This file should be mounted to `/var/www/html/config/config_inc_addon.php` within the container. This additional file will be appended to the default config_inc.php. By mounting the file, it allows for immediate visibility of changes without the necessity to rebuild or restart the container.
* If you want to incorporate your own custom plugins into the image, there are two methods. Firstly, you could create your personal Dockerfile and copy additional plugins to the `/var/www/html/plugins/` directory. Alternatively, in your docker-compose, you could establish a volume to mount the extra plugin directly within the existing image using the path `./custom_plugin/:/var/www/html/plugins/custom_plugin/`.

Configuration:
  * Latest MantisBT version
  * PHP version 7.4
  * Customization of the config files
  * ENV in Dockerfile

### Screenshots

<div><img src="https://i.imgur.com/GEUyp30.png" width="50%"></div>

<div><img src="https://i.imgur.com/q6yi0H5.png" width="50%"></div>

<div><img src="https://i.imgur.com/82xOKAd.pngsss" width="50%"></div>
