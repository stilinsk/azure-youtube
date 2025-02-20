# azure-youtube

```
SELECT * 
FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_SCHEMA = 'SalesLT' 
AND TABLE_TYPE = 'BASE TABLE';
```
```
dbutils.fs.mount(
    source='wasbs://@medalionytazure.blob.core.windows.net',
    mount_point='/mnt/bronze',
    extra_configs = {'fs.azure.account.key.medalionytazure.blob.core.windows.net': dbutils.secrets.get(scope = "data", key = "dataa")}
)

dbutils.fs.mount(
    source='wasbs://@medalionytazure.blob.core.windows.net',
    mount_point='/mnt/silver',
    extra_configs = {'fs.azure.account.key.medalionytazure.blob.core.windows.net': dbutils.secrets.get(scope = "data", key = "dataa")}
)

dbutils.fs.mount(
    source='wasbs://@medalionytazure.blob.core.windows.net',
    mount_point='/mnt/gold',
    extra_configs = {'fs.azure.account.key.medalionytazure.blob.core.windows.net': dbutils.secrets.get(scope = "data", key = "dataa")}
)
```
```
# Retrieve widget values
fileName = dbutils.widgets.get('fileName')
tableSchema = dbutils.widgets.get('table_schema')
tableName = dbutils.widgets.get('table_name')

# Create the database if it does not exist
spark.sql(f'CREATE DATABASE IF NOT EXISTS {tableSchema}')

# Create the table if it does not exist
spark.sql(f"""
    CREATE TABLE IF NOT EXISTS {tableSchema}.{tableName}
    USING PARQUET
    LOCATION '/mnt/bronze/{fileName}/{tableSchema}.{tableName}.parquet'
""")
```
