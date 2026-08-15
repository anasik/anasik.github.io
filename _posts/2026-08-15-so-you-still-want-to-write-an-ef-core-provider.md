In 2016, [Arthur Vickers wrote a post](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/)
called *"So you want to write an EF Core provider."* The post says right at the bottom that it's *"up-to-date as of
November 11th, 2016."* It was written against EF Core 1.1, and almost none of the specific types it names still exist.

Last year, when I wanted to write a provider for Azure Database Explorer AKA Kusto, I found myself reading that post
and struggling to fill in the gaps. So when I actually finished writing [my provider](https://github.com/anasik/EFCore.Kusto) and [the introductory blog post about
it](https://anasismail.com/i-wrote-an-ef-core-provider/), I made a note to write a spiritual successor to Arthur's post, explaining what has changed in the last ten years. This is that post.

This post tries to follow the roadmap of the orignal post as closely as possible but it deviates where necessary to better match the natural order of implementation today. This post also highlights the differences
between the original post and the current state of EF Core, so you can see what has changed and what has stayed the same
but it doesn't require you to read the original post to understand this one.

## Step 1: Implement services

### Implement the services

The 2016 post has you subclass one big `DatabaseProviderServices` class. Today, each service is its own class, registered individually. Let's start with `IDatabaseProvider`, our "Hello World!" moment. This interface was mentioned in the old post but was
used very differently.

```csharp
public sealed class MyProviderDatabaseProvider : IDatabaseProvider
{
    public string Name => "MyProvider";

    public bool IsConfigured(IDbContextOptions options)
        => options.FindExtension<MyProviderOptionsExtension>() != null;
}
```

Then we have `IRelationalTypeMappingSource` which maps CLR types to your database's column types.

```csharp
public sealed class MyProviderTypeMappingSource(
    TypeMappingSourceDependencies dependencies,
    RelationalTypeMappingSourceDependencies relationalDependencies)
    : RelationalTypeMappingSource(dependencies, relationalDependencies)
{
    protected override RelationalTypeMapping? FindMapping(in RelationalTypeMappingInfo mappingInfo)
    {
        if (mappingInfo.ClrType == typeof(string)) return new StringTypeMapping("TEXT", DbType.String);
        if (mappingInfo.ClrType == typeof(int)) return new IntTypeMapping("INTEGER", DbType.Int32);
        return base.FindMapping(mappingInfo);
    }
}
```

`IProviderConventionSetBuilder` and `LoggingDefinitions` both come with default implementations but they're abstract
classes. We just need to subclass them without any overrides.

```csharp
public sealed class MyProviderConventionSetBuilder(
    ProviderConventionSetBuilderDependencies dependencies,
    RelationalConventionSetBuilderDependencies relationalDependencies)
    : RelationalConventionSetBuilder(dependencies, relationalDependencies) { }

public sealed class MyProviderLoggingDefinitions : RelationalLoggingDefinitions { }
```

`IModificationCommandBatchFactory` needs to return a `ModificationCommandBatch` object. EF Core has a built-in
`SingularModificationCommandBatch` that we can use. It doesn't really do batching, but it will do for now.

```csharp
public sealed class MyProviderModificationCommandBatchFactory(ModificationCommandBatchFactoryDependencies dependencies)
    : IModificationCommandBatchFactory
{
    public ModificationCommandBatch Create() => new SingularModificationCommandBatch(dependencies);
}
```
`IUpdateSqlGenerator` is pretty self-explanatory. This is where you generate the SQL for inserts, updates, and deletes.
For now, we can just throw `NotImplementedException` for all of the methods.

```csharp
public sealed class MyProviderUpdateSqlGenerator : IUpdateSqlGenerator
{
    public ResultSetMapping AppendInsertOperation(
        StringBuilder commandStringBuilder, IReadOnlyModificationCommand command, int commandPosition, out bool requiresTransaction)
        => throw new NotImplementedException();

    // The other nine members follow the same pattern:
    // GenerateNextSequenceValueOperation
    // AppendNextSequenceValueOperation
    // GenerateObtainNextSequenceValueOperation
    // AppendObtainNextSequenceValueOperation
    // AppendBatchHeader
    // PrependEnsureAutocommit
    // AppendDeleteOperation
    // AppendUpdateOperation
    // AppendStoredProcedureCall
}
```

Lastly, `IRelationalConnection` is what wraps your ADO.NET connection, if your database has one.

```csharp
public sealed class MyProviderConnection(RelationalConnectionDependencies dependencies) : RelationalConnection(dependencies)
{
    protected override DbConnection CreateDbConnection() => new SqliteConnection(ConnectionString);
}
```

<details markdown="1">
<summary>Click here to expand detailed instructions if your database's client SDK doesn't use ADO.NET at all.</summary>

```csharp
public sealed class MyProviderConnection(RelationalConnectionDependencies dependencies) : RelationalConnection(dependencies)
{
    protected override DbConnection CreateDbConnection() => new MyProviderDbConnection(ConnectionString);

    private sealed class MyProviderDbConnection(string connectionString) : DbConnection
    {
        public override string ConnectionString { get; set; } = connectionString;
        public override string Database => "";
        public override string DataSource => "";
        public override string ServerVersion => "MyProvider";
        public override ConnectionState State => ConnectionState.Open;

        public override void Open() { }
        public override void Close() { }
        public override void ChangeDatabase(string databaseName) { }

        protected override DbTransaction BeginDbTransaction(IsolationLevel isolationLevel)
            => throw new NotSupportedException();

        protected override DbCommand CreateDbCommand() => new MyProviderDbCommand { Connection = this };
    }

    private sealed class MyProviderDbCommand : DbCommand
    {
        public override string CommandText { get; set; } = "";
        public override int CommandTimeout { get; set; }
        public override CommandType CommandType { get; set; } = CommandType.Text;
        public override bool DesignTimeVisible { get; set; }
        public override UpdateRowSource UpdatedRowSource { get; set; }
        protected override DbConnection? DbConnection { get; set; }
        protected override DbParameterCollection DbParameterCollection { get; } = new MyProviderDbParameterCollection();
        protected override DbTransaction? DbTransaction { get; set; }

        public override void Cancel() { }
        public override void Prepare() { }
        protected override DbParameter CreateDbParameter() => new MyProviderDbParameter();
        public override int ExecuteNonQuery() => throw new NotSupportedException();
        public override object? ExecuteScalar() => throw new NotSupportedException();

        protected override DbDataReader ExecuteDbDataReader(CommandBehavior behavior)
        {
            var rows = YourRealClient.Execute(CommandText, Parameters); // This is where you actually call your client SDK
            return new MyProviderDbDataReader(rows);
        }
    }
    
    private sealed class MyProviderDbParameter : DbParameter
    {
        public override DbType DbType { get; set; }
        public override ParameterDirection Direction { get; set; } = ParameterDirection.Input;
        public override bool IsNullable { get; set; }
        public override string ParameterName { get; set; } = "";
        public override int Size { get; set; }
        public override string SourceColumn { get; set; } = "";
        public override bool SourceColumnNullMapping { get; set; }
        public override object? Value { get; set; }
        public override void ResetDbType() { }
    }
    
    // DbParameterCollection is an abstract class that implements IList. We can just wrap a List<T> and implement the abstract members.
    private sealed class MyProviderDbParameterCollection : DbParameterCollection
    {
        private readonly List<MyProviderDbParameter> _parameters = new();
        public override int Count => _parameters.Count;
        public override object SyncRoot => _parameters;
    
        public override int Add(object value)
        {
            _parameters.Add((MyProviderDbParameter)value);
            return _parameters.Count - 1;
        }
    
        public override void AddRange(Array values)
        {
            foreach (var value in values) Add(value!);
        }
    
        public override void Clear() => _parameters.Clear();
        public override bool Contains(object value) => _parameters.Contains((MyProviderDbParameter)value);
        public override bool Contains(string value) => IndexOf(value) >= 0;
        public override void CopyTo(Array array, int index) => ((IList)_parameters).CopyTo(array, index);
        public override IEnumerator GetEnumerator() => _parameters.GetEnumerator();
        public override int IndexOf(object value) => _parameters.IndexOf((MyProviderDbParameter)value);
        public override int IndexOf(string parameterName) => _parameters.FindIndex(p => p.ParameterName == parameterName);
        public override void Insert(int index, object value) => _parameters.Insert(index, (MyProviderDbParameter)value);
        public override void Remove(object value) => _parameters.Remove((MyProviderDbParameter)value);
        public override void RemoveAt(int index) => _parameters.RemoveAt(index);
        public override void RemoveAt(string parameterName) => RemoveAt(IndexOf(parameterName));
    
        protected override DbParameter GetParameter(int index) => _parameters[index];
        protected override DbParameter GetParameter(string parameterName) => _parameters[IndexOf(parameterName)];
        protected override void SetParameter(int index, DbParameter value) => _parameters[index] = (MyProviderDbParameter)value;
        protected override void SetParameter(string parameterName, DbParameter value) => _parameters[IndexOf(parameterName)] = (MyProviderDbParameter)value;
    }
}
```

`MyProviderDbDataReader` is what actually reads the rows returned by your client SDK. Here, we assume that the client
SDK returns a `List<Dictionary<string, object?>>`, one dictionary per row. Each dictionary maps column names to values.
Your actual client dictates this: if it gives you positional arrays instead, take those; if it gives you its own row
type, take that. Shape the constructor around whatever your SDK actually returns, not around this example.

```csharp
public sealed class MyProviderDbDataReader(List<Dictionary<string, object?>> rows) : DbDataReader
{
    private readonly string[] _columns = rows.Count > 0 ? rows[0].Keys.ToArray() : [];
    private int _index = -1;
    private bool _isClosed;

    public override int FieldCount => _columns.Length;
    public override int Depth => 0;
    public override bool HasRows => rows.Count > 0;
    public override bool IsClosed => _isClosed;
    public override int RecordsAffected => -1;

    public override bool Read() => ++_index < rows.Count;
    public override bool NextResult() => false;
    public override void Close() => _isClosed = true;

    public override string GetName(int ordinal) => _columns[ordinal];
    public override int GetOrdinal(string name) => Array.IndexOf(_columns, name);
    public override object GetValue(int ordinal) => rows[_index][_columns[ordinal]] ?? DBNull.Value;
    public override bool IsDBNull(int ordinal) => GetValue(ordinal) is DBNull;
    public override IEnumerator GetEnumerator() => new DbEnumerator(this);

    public override bool GetBoolean(int ordinal) => (bool)GetValue(ordinal);
    public override int GetInt32(int ordinal) => (int)GetValue(ordinal);
    public override long GetInt64(int ordinal) => (long)GetValue(ordinal);
    public override double GetDouble(int ordinal) => (double)GetValue(ordinal);
    public override decimal GetDecimal(int ordinal) => (decimal)GetValue(ordinal);
    public override string GetString(int ordinal) => (string)GetValue(ordinal);
    public override DateTime GetDateTime(int ordinal) => (DateTime)GetValue(ordinal);
    public override Guid GetGuid(int ordinal) => (Guid)GetValue(ordinal);
    public override byte GetByte(int ordinal) => (byte)GetValue(ordinal);
    public override short GetInt16(int ordinal) => (short)GetValue(ordinal);
    public override float GetFloat(int ordinal) => (float)GetValue(ordinal);
    public override char GetChar(int ordinal) => (char)GetValue(ordinal);

    public override object this[int ordinal] => GetValue(ordinal);
    public override object this[string name] => GetValue(GetOrdinal(name));

    public override int GetValues(object[] values) => throw new NotSupportedException();
    public override long GetBytes(int ordinal, long dataOffset, byte[]? buffer, int bufferOffset, int length) => throw new NotSupportedException();
    public override long GetChars(int ordinal, long dataOffset, char[]? buffer, int bufferOffset, int length) => throw new NotSupportedException();
    public override Type GetFieldType(int ordinal) => throw new NotSupportedException();
    public override string GetDataTypeName(int ordinal) => throw new NotSupportedException();
}
```

</details>

### Create an "AddEntityFramework..." extension method

Same name and purpose as 2016, different services. This is where we register all of the services we defined and then call `TryAddCoreServices()` at the end to fill in EF's default implementations for the remaining services that we didn't override. e.g. `IQuerySqlGeneratorFactory`.

```csharp
public static class MyProviderServiceCollectionExtensions
{
    public static IServiceCollection AddEntityFrameworkMyProvider(this IServiceCollection services)
    {
        new EntityFrameworkRelationalServicesBuilder(services)
            .TryAdd<IDatabaseProvider, MyProviderDatabaseProvider>()
            .TryAdd<IRelationalTypeMappingSource, MyProviderTypeMappingSource>()
            .TryAdd<IRelationalConnection, MyProviderConnection>()
            .TryAdd<ISqlGenerationHelper, RelationalSqlGenerationHelper>()
            .TryAdd<LoggingDefinitions, MyProviderLoggingDefinitions>()
            .TryAdd<IProviderConventionSetBuilder, MyProviderConventionSetBuilder>()
            .TryAdd<IModificationCommandBatchFactory, MyProviderModificationCommandBatchFactory>()
            .TryAdd<IUpdateSqlGenerator, MyProviderUpdateSqlGenerator>()
            .TryAddCoreServices();
    
        return services;
    }
}
```
You'll notice that we also injected `ISqlGenerationHelper` which we never defined. That's because EF Core has a default
concrete implementation that we can just use. You don't need to subclass it unless your database
has a different SQL syntax for identifiers, parameters, or comments.

## Step 2: Implement a `Use...` method

### Create an options extension

Fairly unchanged from the 2016 post. We still subclass `RelationalOptionsExtension` and it still carries an `ApplyServices` method to call the `AddEntityFramework...`method above, but the rest of the members are different.

By implementing `IDbContextOptionsExtension` we can have provider-specific configuration surface. EF Core will call
`ApplyServices` to register our services. There's a clone method to replace the old copy constructor pattern.

```csharp
public sealed class MyProviderOptionsExtension : RelationalOptionsExtension
{
    private DbContextOptionsExtensionInfo? _info;

    public MyProviderOptionsExtension() { }
    private MyProviderOptionsExtension(MyProviderOptionsExtension copyFrom) : base(copyFrom) { }

    protected override RelationalOptionsExtension Clone() => new MyProviderOptionsExtension(this);

    public override void ApplyServices(IServiceCollection services)
        => services.AddEntityFrameworkMyProvider();

    public override DbContextOptionsExtensionInfo Info
        => _info ??= new ExtensionInfo(this);

    private sealed class ExtensionInfo(MyProviderOptionsExtension extension) : RelationalExtensionInfo(extension)
    {
        public override void PopulateDebugInfo(IDictionary<string, string> debugInfo) { }
    }
}
```

### Create a 'Use...' method

This part is just convention rather than a service implementation, so it's largely unchanged from the 2016 post. Users call this method in their `DbContext.OnConfiguring`. There can and should be multiple overloads of it. The simplest one takes a connection string.

```csharp
public static class MyProviderDbContextOptionsBuilderExtensions
{
    public static DbContextOptionsBuilder UseMyProvider(
        this DbContextOptionsBuilder builder, string connectionString)
    {
        var extension = builder.Options.FindExtension<MyProviderOptionsExtension>()
            ?? new MyProviderOptionsExtension();
    
        extension = (MyProviderOptionsExtension)extension.WithConnectionString(connectionString);
        ((IDbContextOptionsBuilderInfrastructure)builder).AddOrUpdateExtension(extension);
    
        return builder;
    }
}
```

## Step 3: Create metadata extension methods

### Define annotations

This one is only similar to the 2016 post in spirit. We still need to create a prefix name for our provider's
annotations, but the rest of the implementation is vastly simpler. All we need is a `IRelationalAnnotationProvider`
implementation that we can achieve by subclassing `RelationalAnnotationProvider`. Of course, if you do not need any
annotations, you can skip this step entirely.

In this example, we define a `MyProvider:StorageEngine` annotation that can be set on an entity type to specify which
storage engine to use for that table.

```csharp
public sealed class MyProviderAnnotationProvider(RelationalAnnotationProviderDependencies dependencies)
    : RelationalAnnotationProvider(dependencies)
{
    public override IEnumerable<IAnnotation> For(ITable table, bool designTime)
    {
        var entityType = (IEntityType)table.EntityTypeMappings.First().TypeBase;
        var storageEngine = entityType.FindAnnotation("MyProvider:StorageEngine");

        if (storageEngine is not null)
        {
            yield return new Annotation("MyProvider:StorageEngine", storageEngine.Value);
        }
    }
}
```

If you do choose to add your own annotations, make sure to add this line to `AddEntityFrameworkMyProvider()` before the
`TryAddCoreServices()` call:
`.TryAdd<IRelationalAnnotationProvider, MyProviderAnnotationProvider>()`.

### Create fluent API extensions

To actually make use of the annotation we just defined, we need to create a fluent API extension method that sets it on
an entity type.

```csharp
public static class MyProviderEntityTypeBuilderExtensions
{
    public static EntityTypeBuilder HasMyProviderStorageEngine(
        this EntityTypeBuilder entityTypeBuilder, string storageEngine)
    {
        entityTypeBuilder.Metadata.SetAnnotation("MyProvider:StorageEngine", storageEngine);
        return entityTypeBuilder;
    }
}
```

## Not covered here

- **Real batching.** `SingularModificationCommandBatch` sends one row per round trip. Batching multiple rows into one
  command means subclassing `AffectedCountModificationCommandBatch` instead.
- **Migrations.** `IMigrationsSqlGenerator` and `IHistoryRepository` are what `dotnet ef migrations add` and
  `Database.Migrate()` need.

  When I wrote my provider, I wanted to quickly get to a point where it can succesfully read rows from the database. That's where my idea of a minimal provider, that this post promised, comes from. Nevertheless, the entry points for implementing mutations and batching are shown clearly in the post. Migrations, however were never in the scope for this post.
