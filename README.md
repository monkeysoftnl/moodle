# Moodle Docker Image

This repository contains a lightweight Docker setup for running Moodle (the open-source learning management system). It provides a Dockerfile and a compose configuration to build and run Moodle in containers for local development or small deployments.

**Quick Highlights**
- **Purpose:** Run Moodle in Docker for development, testing, or demo environments.
- **Includes:** `Dockerfile` and a `compose.yml` example to orchestrate services.

**Requirements**
- **Docker:** Install Docker Engine (Desktop or Engine) on your machine.
- **Compose:** `docker compose` or `docker-compose` for multi-container runs.

**Quick Start**
- Build the image locally:

	`docker build -t moodleapp .`

- Start services with the provided compose file:

	`docker compose -f compose.yml up -d`

- Visit `http://localhost` (or the host/port you configured) to complete the Moodle web installer.

**Configuration**
- **Database:** Provide a database for Moodle (MySQL/MariaDB or PostgreSQL). Configure DB host, user, password and database name via environment variables or your compose file.
- **Persistent Storage:** Mount a volume for Moodle data (moodledata) and for the database data directory to keep uploads and DB data across restarts.
- **Ports:** Expose the webserver port (commonly `80` or `8080`) to access the site from the host.

Example minimal `docker compose` snippet (adapt to your DB choice):

```
services:
	moodleapp:
		build: .
		ports:
			- "80:80"
		volumes:
			- moodledata:/var/moodledata
		environment:
			- MOODLE_DB_HOST=db
			- MOODLE_DB_USER=moodle
			- MOODLE_DB_PASSWORD=secret
			- MOODLE_DB_NAME=moodle

	db:
		image: mysql:8
		environment:
			- MYSQL_ROOT_PASSWORD=secret
			- MYSQL_DATABASE=moodle
			- MYSQL_USER=moodle
			- MYSQL_PASSWORD=secret
		volumes:
			- dbdata:/var/lib/mysql

volumes:
	moodledata:
	dbdata:
```

**Files of Interest**
- Dockerfile: [Dockerfile](Dockerfile)
- Compose example: [compose.yml](compose.yml)

**Troubleshooting**
- If the web installer cannot connect to the database, verify the database container is healthy and the host/credentials match the Moodle environment variables.
- Check container logs for errors:

	`docker compose -f compose.yml logs -f`

- For permission errors on uploaded files, ensure the `moodledata` volume is writable by the webserver process user inside the container.

**Credits & References**
- Original guidance and reference material: https://seunayolu.hashnode.dev/run-moodle-lms-as-a-container-with-docker-compose

**License**
- This repository follows the LICENSE file included in the project root.