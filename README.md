# README

## Table of Contents
- [Table of Contents](#table-of-contents)
- [File structure](#file-structure)
- [Run Docker containers](#run-docker-containers)
- [Database credentials](#database-credentials)
- [Import OSM data](#import-osm-data)
- [Register Server](#register-server)
- [Make SQL Queries](#make-sql-queries)
- [Known Issues and How to fix them](#known-issues-and-how-to-fix-them)

## File structure
```bash
Project/
│
├── .devcontainer/
│   └── devcontainer.json
|   └── servers.json
│
├── SQL/
├── SQL_EXTENDED/
├── default.style
├── docker-compose.yml
├── Dockerfile
├── postgresql_postgis.ipynb
├── README.md
├── requirements.txt
└── ...
```

## Run Docker containers
```bash
VS Code -> left Menu -> search file 'docker-compose.yml' -> right click -> Compose Up
```

## Database credentials
```bash
Host: db
Port: 5432
Maintenance database: postgres
Username: pgadmin
Password: geheim
```

## Import OSM data

In VS Code -> Terminal, use the following Docker commands to insert OpenStreetMap data into the PostgreSQL database.

```bash
# Show running Docker containers
docker ps

# Open bash and run osm2pgsql commands to fill up OpenStreetMap tables
docker exec -it postgis_container bash

# Run the following code in bash (change user name and password if required)
PGPASSWORD=geheim osm2pgsql -c -d osm_switzerland -U pgadmin -H db -P 5432 -S /usr/bin/default.style /tmp/zurich-latest.osm.pbf

# Exit bash
exit

# Show available tables in the database 'osm_switzerland'
docker exec -it postgis_container psql -U pgadmin -d osm_switzerland -c "\dt;"

# quit psql
q
```

## Register Server
The pgAdmin tool can be used to manage the databases. Therefore, a server must be registered.

In VS Code, open a new Terminal. In the upper part of the Terminal, click on 'PORTS'.

Search for the port 5050 and click on 'Open in Browser' on the right side of the link.

Log into the pgAdmin tool (Email: pgadmin@gmail.com, Password: geheim)

Inside pgAdmin, before you can see the databases and tables, you must register a server.

How to register (use the credentials shown above): https://youtu.be/MYhPNS2Ivtw?si=o0niuELbzuSCMJ3c&t=20

After registration, tables can be found under: 

```bash
Servers -> db -> Databases -> osm_switzerland -> Schemas -> public -> Tables
```
**Tip: If registration fails, reload the web page and log in again!**

## Make SQL Queries
In order to make SQL queries, right click on the 'osm_switzerland' database name -> Query Tool.

Use the example queries from the SQL folder like:

```bash
SELECT
    p.osm_id,
    p."addr:street",
    p."addr:housenumber",
    p."addr:city",
    p."addr:postcode",
    p.building,
    st_transform(p.way, 4326) AS geom
FROM
    public.planet_osm_polygon AS p
WHERE 
    p."addr:street" IS NOT NULL
    AND p."addr:city" = 'Zürich'
    AND p."addr:postcode" IN ('8001')
```

**Tip: If you click on the flag symbol in the 'geom' column of the results table, you can visualize the spatial data in pgAdmin.**

## Known Issues and How to fix them
```bash
1.) Maps are not displayed in Jupyter Notebooks.
- This is a known bug related to the rendering of Jupyter Notebook content.
- You should save, close, and reopen your Jupyter Notebook, then it should work.

2.) SQL queries are empty.
- Check that your SQL query is correct, e.g., using pgadmin4.
- Make sure that the OpenStreetMap data has been imported into the database (see README file).
```
