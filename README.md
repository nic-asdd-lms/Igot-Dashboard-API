# iGOT Dashboard API
API to fetch analytics data of child organisations

## Prerequisites
1. Java version 17
2. Maven version 3.8.8

## Run

1. Create tables `ehrms_users` and `ehrms_logs` in Postgres:
```
CREATE TABLE IF NOT EXISTS public.ehrms_users
(
    id uuid NOT NULL,
    password text COLLATE pg_catalog."default" NOT NULL,
    org text COLLATE pg_catalog."default",
    CONSTRAINT ehrms_users_pkey PRIMARY KEY (id)
)
```

```
CREATE TABLE IF NOT EXISTS public.ehrms_logs
(
    id serial NOT NULL ,
    user_id character varying(50) COLLATE pg_catalog."default" NOT NULL,
    org_id character varying(50) COLLATE pg_catalog."default",
    action text COLLATE pg_catalog."default",
    "timestamp" time without time zone,
    CONSTRAINT ehrms_logs_pkey PRIMARY KEY (id)
)
```

2. `mvn clean install`

3. `java -jar target/ehrms-0.0.1-SNAPSHOT.jar --config=<path to config file> --response=<path to file generated by https://github.com/nic-asdd-lms/Igot-Dashboard-Script> --db.ip=<postgres ip> --db.port=<postgres port> --db.name=<db> --db.user=<postgres username> --db.password=<postgres password>`

4. Create user for a department: 
```
curl --location --request POST 'localhost:8000/ehrmsservice/apis/igot/dashboard/user/create/<mapId>' 
```

5. Generate token for the created user:
```
curl --location --request POST 'localhost:8000/ehrmsservice/apis/igot/dashboard/authenticate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "password": <password>,
    "id": <id>
}' 
```

6. Call the API to get the metrics
```
curl --location --request GET 'localhost:8000/ehrmsservice/apis/igot/analytics/<mapId>' \
--header 'id: <id>' \
--header 'Authorization: <Bearer token>' \
```
