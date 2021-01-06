The environment variables to be defined as 

POSTGRES_USER_FILE: /run/secrets/pg_user 

POSTGRES_PASSWORD_FILE: /run/secrets/pg_password 

POSTGRES_DB_FILE: /run/secrets/pg_database


specified here the name of the secrets which are being used by the service:

secrets:

- pg_password

- pg_user

- pg_database

Indicates that the secrets are external:

secrets:

 pg_user:

   external: true

 pg_password: 

   external: true

 pg_database: 

   external: true
