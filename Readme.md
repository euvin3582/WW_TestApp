## Creating Migration Code First ##
# To enable code first db migration
1. Enable-Migrations

# Add an initial create script to an exisiting db i.e cmd name -params
Add-Migration InitialCreate -IgnoreChanges

# The ignorechanges command will create an empty db script to create a base db run same command without ignore
Add-Migration InitialCreate

# update script in db
update-database

#This creates a table with all the change scripts going forward

## Adding a new table to DB ##

# Add a new class file with table info i.e. NewTableModel
# Create a class under the model folder or wherver you keep you models at
# Make sure you create a primary key column and add the [Key] attribute to the column i.e.
#	[Key]
#   public int id { get; set; }
# Make sure that you make the primary key column public
# use  [StringLength(100)] for varchars and to specify the length for varchars
# add table to your model OnModelCreating function
# i.e.
#	modelBuilder.Entity<NewTableModel>().ToTable("New");
# Note: The Entity<ModelName> is the class that was created for the model and ToTable is the name of the table being created.
Add-Migration AddingNewModel
Update-Database

# Setting default Date Value use: 'defaultValueSql: "GETDATE()"' i.e.
ModifiedDate = c.DateTime(nullable: false, defaultValueSql: "GETDATE()")
#or
Name = c.String(nullable: false, defaultValueSql: "Jose")

# Data Motion : No native support for data motion yet but you can run SQL commands i.e.
# add this to the Up() override
Sql("UPDATE dbo.MyDBTable SET ColumnName = LEFT(Content, 100) WHERE ColumnName IS NULL");
Sql("INSERT INTO MyNewTable(NyColumnName) Values('Test')");

# Automatic DB migration: Update DB to latest on startup i.e. 
# This can be added to the Main() function for a Console app or to the protected void Application_Start()
# in the Global.asax.cs in MVC
 Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

 # Downgrading: This will run the Down() scripts  Run the Update-Database –TargetMigration: MigrationName i.e.
Update-Database –TargetMigration: StudentTable 

# To roll back to an empty DB
Update-Database –TargetMigration: $InitialDatabase