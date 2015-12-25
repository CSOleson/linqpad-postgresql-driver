# LINQPad PostgreSQL Driver

This is a driver for [LINQPad](https://www.linqpad.net) to add support for [PostgreSQL](http://www.postgresql.org) databases. This driver uses [LINQ to DB](https://github.com/linq2db/linq2db) to execute the LINQ queries and [Npgsql](http://www.npgsql.org) for the database access. In addition [Dapper](https://github.com/StackExchange/dapper-dot-net) is used for a more convenient database access during the model creation.

This driver has been tested with PostgreSQL 9.4.4 but should work with all versions supported by [LINQ to DB](https://github.com/linq2db/linq2db) v1.0.7.3 and [Npgsql](http://www.npgsql.org) v3.0.4. It can be used with [LINQPad](https://www.linqpad.net) versions 4 and 5.

### Modifying data

Unfortunately, editing data directly in the results grid is not supported for third-party drivers. You can, however, use the [LINQ to DB](https://github.com/linq2db/linq2db) `DataConnection` to modify your data. The `DataConnection` can be accessed using the `this` keyword in your query. In the following some samples to insert, update and delete records are provided. You can get more detailed information on the [LINQ to DB](https://github.com/linq2db/linq2db) website.

#### [Insert](https://github.com/linq2db/linq2db#insert)

If you want to provide an ID yourself:

```csharp
var user = new User();
user.Id = 42;
user.Name = "John Doe";
user.Emailaddress = "john.doe@xyz.com";
user.Password = "1234";

this.Insert(user);
```

If the ID is generated by the database:

```csharp
var user = new User();
user.Name = "John Doe";
user.Emailaddress = "john.doe@xyz.com";
user.Password = "1234";

var userId = this.InsertWithIdentity(user);
```

#### [Update](https://github.com/linq2db/linq2db#update)

If you have an instance of the object you want to update:

```csharp
var user = Users.Single(u => u.Id == 42);
user.Emailaddress = "john.doe@abc.com";

this.Update(user);
```

If you want to update all records matching certain criteria:

```csharp
Users.Where(u => u.Id == 42)
	 .Set(u => u.Emailaddress, "john.doe@abc.com")
	 .Update();
```

#### [Delete](https://github.com/linq2db/linq2db#delete)

If you have an instance of the object you want to delete:

```csharp
var user = Users.Single(u => u.Id == 8);
this.Delete(user);
```

If you want to delete all records matching certain criteria:

```csharp
Users.Delete(u => u.Id == 42);
```