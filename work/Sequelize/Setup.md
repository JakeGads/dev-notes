# Setup

## Connections

Sequelize uses a constructor, the constructor only requires a single string denoting where the db can be found

```js
const connection = new Sequelize('sqlite::memory:') // Example for sqlite
const sequelize = new Sequelize('postgres://user:pass@example.com:5432/dbname') // Example for postgres
```

there is also an extend route, the docs recommend keeping a json file, (use as a git secret as well)

```json
// contents of dbConfig.json
{
    HOST: "localhost",
    USER: "root",
    PASSWORD: "123456",
    DB: "testdb",
    dialect: "mysql",
    pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
}
```
  
we then can check the file contents with 

```js
const dbConfig = require('dbConfig.json');

const connection = new Sequelize(
    dbConfig.DB, dbConfig.USER, dbConfig.PASSWORD, 
    {
        host: dbConfig.HOST,
        dialect: dbConfig.dialect,
        operatorsAliases: false,
        logging: console.log,

        pool: {
            max: dbConfig.pool.max,
            min: dbConfig.pool.min,
            acquire: dbConfig.pool.acquire,
            idle: dbConfig.pool.idle
        }
    }
);
```

You can swap out the dialect of the config with any of the following `['mysql', 'mariadb', 'postgres', 'mssql']` as needed
You can add custom error logging in the `logger` argument of the options object (drill down 1)


```js
// async method to check connection
try {
  await connection.authenticate();
  console.log('Connection has been established successfully.');
} catch (error) {
  console.error('Unable to connect to the database:', error);
}
```

default behavior is to leave this connection open for the lifespan of the application to close the application call `connection.close()`