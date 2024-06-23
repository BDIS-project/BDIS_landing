## Subprojects are connected by submodules

To connect them, you need to enter the git submodule init and git submodule update commands.

To switch to the correct branch for tests, go to the subproject folder and run git checkout <branch name>

## Configuration
The project requires an `.env` file with the following fields:
    ```
    POSTGRES_DATABASE=zlagoda
    POSTGRES_USER=django-admin
    POSTGRES_PASSWORD=<django-db-password>
    POSTGRES_ROOT_PASSWORD=<db-root-password>
    ```
## Start and stop the project

To work, you need to be in the root directory

## Start the project:
    ```bash
    docker compose -f docker-compose-local.yml up --build -d
    ```
    *Wait a **minute** before the server starts completely.
*Stop the project:
    ````bash
    docker compose -f docker-compose-local.yml stop
    ```
* Delete containers:
    ```bash
    docker compose -f docker-compose-local.yml rm
    ```
### Database dump

***Only when DB containser is running***

* Create database:
    ```bash
    echo "CREATE DATABASE $POSTGRES_DATABASE;" | \
    docker exec -i bdis_landing_back-postgres-1 psql \
    -U $POSTGRES_USER -d postgres
    ```

* Create database tables from `sql_script.sql`:
    ```bash
    cat sql_script.sql | docker exec -i bdis_landing_back-postgres-1 psql \
    -U $POSTGRES_USER -d $POSTGRES_DATABASE -a
    ```

* Load data from `test_data.sql` to database:
    ```bash
    cat test_data.sql | docker exec -i bdis_landing_back-postgres-1 psql \
    -U $POSTGRES_USER -d $POSTGRES_DATABASE -a