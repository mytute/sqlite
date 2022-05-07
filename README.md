# sqlite


to install sqlite3    

```bash
$ npm install sqlite3 --save
```

## command line sqlite operations  

to open database   

```bash
$ sqlite3 <database_name>.sqlite
```

to show list of tables in dabase     

```bash
$ sqlite> .tables
```

to show list of tables in dabase     

```bash
$ sqlite> PRAGMA table_info(employer);

# result >>>>
# 0|employerid|INTEGER|0|1|1
# 1|nic|CHAR|0|null|0
# 2|name|CHAR|0|null|0
# 3|level|CHAR|0|null|0
# 4|username|CHAR|0|null|0
# 5|password|CHAR|0|null|0

```

## use of "seqelize" package for sqlite3

```bash
$ npm i sequelize --save
```

mode configuration

```javascript
class Bar extends Model {}
Bar.init({ /* bla */ }, {
  // The name of the model. The model will be stored in `sequelize.models` under this name.
  // This defaults to class name i.e. Bar in this case. This will control name of auto-generated
  // foreignKey and association naming
  modelName: 'bar',

  // don't add the timestamp attributes (updatedAt, createdAt)
  timestamps: false,

  // don't delete database entries but set the newly added attribute deletedAt
  // to the current date (when deletion was done). paranoid will only work if
  // timestamps are enabled
  paranoid: true,

  // Will automatically set field option for all attributes to snake cased name.
  // Does not override attribute with field option already defined
  underscored: true,

  // disable the modification of table names; By default, sequelize will automatically
  // transform all passed model names (first parameter of define) into plural.
  // if you don't want that, set the following
  freezeTableName: true,

  // define the table's name
  tableName: 'my_very_custom_table_name',

  // Enable optimistic locking.  When enabled, sequelize will add a version count attribute
  // to the model and throw an OptimisticLockingError error when stale instances are saved.
  // Set to true or a string with the attribute name you want to use to enable.
  version: true,

  // Sequelize instance
  sequelize,
})
```

##  "seqelize" data types

```bash  

Sequelize.STRING                      // VARCHAR(255)
Sequelize.STRING(1234)                // VARCHAR(1234)
Sequelize.STRING.BINARY               // VARCHAR BINARY
Sequelize.TEXT                        // TEXT
Sequelize.TEXT('tiny')                // TINYTEXT
Sequelize.CITEXT                      // CITEXT      PostgreSQL and SQLite only.

Sequelize.INTEGER                     // INTEGER
Sequelize.BIGINT                      // BIGINT
Sequelize.BIGINT(11)                  // BIGINT(11)

Sequelize.FLOAT                       // FLOAT
Sequelize.FLOAT(11)                   // FLOAT(11)
Sequelize.FLOAT(11, 10)               // FLOAT(11,10)

Sequelize.REAL                        // REAL        PostgreSQL only.
Sequelize.REAL(11)                    // REAL(11)    PostgreSQL only.
Sequelize.REAL(11, 12)                // REAL(11,12) PostgreSQL only.

Sequelize.DOUBLE                      // DOUBLE
Sequelize.DOUBLE(11)                  // DOUBLE(11)
Sequelize.DOUBLE(11, 10)              // DOUBLE(11,10)

Sequelize.DECIMAL                     // DECIMAL
Sequelize.DECIMAL(10, 2)              // DECIMAL(10,2)

Sequelize.DATE                        // DATETIME for mysql / sqlite, TIMESTAMP WITH TIME ZONE for postgres
Sequelize.DATE(6)                     // DATETIME(6) for mysql 5.6.4+. Fractional seconds support with up to 6 digits of precision
Sequelize.DATEONLY                    // DATE without time.
Sequelize.BOOLEAN                     // TINYINT(1)

Sequelize.ENUM('value 1', 'value 2')  // An ENUM with allowed values 'value 1' and 'value 2'
Sequelize.ARRAY(Sequelize.TEXT)       // Defines an array. PostgreSQL only.
Sequelize.ARRAY(Sequelize.ENUM)       // Defines an array of ENUM. PostgreSQL only.

Sequelize.JSON                        // JSON column. PostgreSQL, SQLite and MySQL only.
Sequelize.JSONB                       // JSONB column. PostgreSQL only.

Sequelize.BLOB                        // BLOB (bytea for PostgreSQL)
Sequelize.BLOB('tiny')                // TINYBLOB (bytea for PostgreSQL. Other options are medium and long)

Sequelize.UUID                        // UUID datatype for PostgreSQL and SQLite, CHAR(36) BINARY for MySQL (use defaultValue: Sequelize.UUIDV1 or Sequelize.UUIDV4 to make sequelize generate the ids automatically)

Sequelize.CIDR                        // CIDR datatype for PostgreSQL
Sequelize.INET                        // INET datatype for PostgreSQL
Sequelize.MACADDR                     // MACADDR datatype for PostgreSQL

Sequelize.RANGE(Sequelize.INTEGER)    // Defines int4range range. PostgreSQL only.
Sequelize.RANGE(Sequelize.BIGINT)     // Defined int8range range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DATE)       // Defines tstzrange range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DATEONLY)   // Defines daterange range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DECIMAL)    // Defines numrange range. PostgreSQL only.

Sequelize.ARRAY(Sequelize.RANGE(Sequelize.DATE)) // Defines array of tstzrange ranges. PostgreSQL only.

Sequelize.GEOMETRY                    // Spatial column.  PostgreSQL (with PostGIS) or MySQL only.
Sequelize.GEOMETRY('POINT')           // Spatial column with geometry type. PostgreSQL (with PostGIS) or MySQL only.
Sequelize.GEOMETRY('POINT', 4326)     // Spatial column with geometry type and SRID.  PostgreSQL (with PostGIS) or MySQL only.

```

## DATEONLY format    

to save "DATEONLY" format your then good to have iso date format.     
ex :      
2021-10-10 > correct     
2021/10/10 > make some waring      


## put ENUM with seqelize     

```javascript
var Model = sequelize.define('model', {
  states: {
    type:   Sequelize.ENUM,
    values: ['active', 'pending', 'deleted']
  }
});
```



Model synchronization

User.sync() - This creates the table if it doesn't exist (and does nothing if it already exists)
User.sync({ force: true }) - This creates the table, dropping it first if it already existed
User.sync({ alter: true }) - This checks what is the current state of the table in the database (which columns it has, what are their data types, etc), and then performs the necessary changes in the table to make it match the model.


# Query


## findOne

```javascript
User.findOne({
                where: {username:userName},
                //attributes: ['username', 'password', 'nic'],
                //order: [ [ 'createdAt', 'DESC' ]],
                raw:true
             })
     .then(function(operator){
         console.log("operator >>", operator);
         if(operator){
             callback(null, operator);
         }else{
             callback({message:"user credentials not matched"}); // user not found
         }

     },err =>{
         console.log("err >>", err);
         callback(err);
     });
```

## findOne

```javascript
User.findOne({
                where: {username:userName},
                //attributes: ['username', 'password', 'nic'],
                //order: [ [ 'createdAt', 'DESC' ]],
                raw:true
             })
     .then(function(operator){
         console.log("operator >>", operator);
         if(operator){
             callback(null, operator);
         }else{
             callback({message:"user credentials not matched"}); // user not found
         }

     },err =>{
         console.log("err >>", err);
         callback(err);
     });
```


## update
```javascript
const operatorID = operator.employerid;
User.update({

    lastLoggedIn: moment().utc().toISOString()

}, { where: { employerid: operatorID }})
.then(function (result) {
    callback(null, operator);
})
.catch(err => {
    callback(err);
});
```

find and update

```javascript
const job = await Job.findOne({where: {id, ownerId: req.user.id}});

if (!job) {
    throw Error(`Job not updated. id: ${id}`);
}

job.name = input.name;
job.payload = input.payload;
await job.save();
```

```javascript
async function upCustomer(){
    const customer = await Customer.findOne({where: { id: customerID }});
    //console.log('@@@@', customer);
    customer.completedPawnCount += 1 ;
    customer.runningPawnCount   -= 1 ;

    await customer.save();
}

upCustomer().then(function(){
    callback(null, pawn);
})
```

```javascript
Customer.findOne(
    {where: { id: customerID }
}).then(customer=>{

    customer.completedPawnCount += 1 ;
    customer.runningPawnCount   -= 1 ;

    customer.save().then(function(re){
        callback(null, pawn);
    });

})
```

## insert / create

to insert new data to

```javascript
User.create({
               data:data
             })
     .then(function(operator){
         console.log("operator >>", operator.dataValues); // return inserted values
         if(operator){
             callback(null, operator);
         }else{
             callback({message:"user credentials not matched"}); // user not found
         }

     },err =>{
         console.log("err >>", err);
         callback(err);
     });
```


## use of "knex" package for sqlite3





# mysql tutorial
