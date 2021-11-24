# Sequelize Model

## Making Model

Models are created extending the `Sequelize.Model` class

```js
class User extends Model {}

User.init({
  // Model attributes are defined here
  firstName: {
    type: DataTypes.STRING,
    allowNull: false
  },
  lastName: {
    type: DataTypes.STRING
    // allowNull defaults to true
  }
}, {
  // Other model options go here
  connection, // We need to pass the connection instance
  modelName: 'User' // We need to choose the model name
});

// the defined model is the class itself
console.log(User === connection.models.User); // true
```

By default sequelize auto handles models names based on the class name however if you want to enforce it add the `tableName` property to the `.init` functions options object

Because these objects are also classes you can include regular js class functions to them that will allow you to call during your operations

To save this model to the db you need to call the sync method, there are 3 ways of handling the sync method 

- `User.sync()` - This creates the table if it doesn't exist (and does nothing if it already exists)
- `User.sync({ force: true })` - This creates the table, dropping it first if it already existed
- `User.sync({ alter: true })` - This checks what is the current state of the table in the database (which columns it has, what are their data types, etc), and then performs the necessary changes in the table to make it match the model.

```js
try{
    await User.sync({ alter: true });
    console.log("The table for the User model was just (re)created!");
} catch(err){
    console.log("something went wrong", err)
}
```

you can also similarly sync the entire database together

```js
try{
    await connection.sync({ alter: true });
    console.log("The table for the User model was just (re)created!");
} catch(err){
    console.log("something went wrong", err)
}
```

## Dropping tables

To drop the table related to a model:

```js
await User.drop();
console.log("User table dropped!");
```
To drop all tables:

```js
await sequelize.drop();
console.log("All tables dropped!");
```

### DataTypes

Data Types
Every column you define in your model must have a data type. Sequelize provides a lot of built-in data types. To access a built-in data type, you must import DataTypes:

```js
const { DataTypes } = require("sequelize"); // Import the built-in data types
```

#### Strings
```js
DataTypes.STRING             // VARCHAR(255)
DataTypes.STRING(1234)       // VARCHAR(1234)
DataTypes.STRING.BINARY      // VARCHAR BINARY

DataTypes.TEXT               // TEXT
DataTypes.TEXT('tiny')       // TINYTEXT
DataTypes.CITEXT             // CITEXT          PostgreSQL and SQLite only.
DataTypes.TSVECTOR           // TSVECTOR        PostgreSQL only.
```

#### Boolean
```js
DataTypes.BOOLEAN            // TINYINT(1)
```


#### Numbers
```js
DataTypes.INTEGER            // INTEGER
DataTypes.BIGINT             // BIGINT
DataTypes.BIGINT(11)         // BIGINT(11)

DataTypes.FLOAT              // FLOAT
DataTypes.FLOAT(11)          // FLOAT(11)
DataTypes.FLOAT(11, 10)      // FLOAT(11,10)

DataTypes.REAL               // REAL            PostgreSQL only.
DataTypes.REAL(11)           // REAL(11)        PostgreSQL only.
DataTypes.REAL(11, 12)       // REAL(11,12)     PostgreSQL only.

DataTypes.DOUBLE             // DOUBLE
DataTypes.DOUBLE(11)         // DOUBLE(11)
DataTypes.DOUBLE(11, 10)     // DOUBLE(11,10)

DataTypes.DECIMAL            // DECIMAL
DataTypes.DECIMAL(10, 2)     // DECIMAL(10,2)
```

#### Unsigned & Zerofilled Ints

```js
// MySQL/MariaDB only


DataTypes.INTEGER.UNSIGNED
DataTypes.INTEGER.ZEROFILL
DataTypes.INTEGER.UNSIGNED.ZEROFILL
// You can also specify the size i.e. INTEGER(10) instead of simply INTEGER
// Same for BIGINT, FLOAT and DOUBLE
```

#### UUIDs

```js
{
  type: DataTypes.UUID,
  defaultValue: DataTypes.UUIDV4 // Or DataTypes.UUIDV1
}
````

## NEXT

