<<<<<<< HEAD
# azure-medallion
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
%python
# Retrieve widget values
fileName = dbutils.widgets.get('fileName')
tableSchema = dbutils.widgets.get('table_schema')
tableName = dbutils.widgets.get('table_name')

# Set the storage account key
spark.conf.set("fs.azure.account.key.medalionytazure.dfs.core.windows.net", "")

# Use the Databricks mount path
storage_path = f"abfss://bronze@medalionytazure.dfs.core.windows.net/{fileName}/{tableSchema}.{tableName}.parquet"

# Check if the directory exists
try:
    dbutils.fs.ls(f"abfss://bronze@medalionytazure.dfs.core.windows.net/{fileName}")
    # Create the table using the correct location
    spark.sql(f"""
        CREATE TABLE IF NOT EXISTS {tableSchema}.{tableName}
        USING PARQUET
        LOCATION '{storage_path}'
    """)
    print(f"Table {tableSchema}.{tableName} created successfully at {storage_path}")
except Exception as e:
    print(f"Directory {fileName} does not exist in the specified path. Error: {e}")
```

```
@concat(item().table_schema,'.',item().table_name,'.parquet')
```
```
@formatDateTime(utcNow(),'yyyyMMdd')
```
=======
Welcome to your new dbt project!

### Using the starter project

Try running the following commands:
- dbt run
- dbt test


### Resources:
- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](https://community.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
>>>>>>> 218a0a7 (Initial commit - added files)
