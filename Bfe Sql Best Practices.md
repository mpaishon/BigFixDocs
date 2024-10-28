# SQL Server Best Practices Checklist

## Performance Optimization

- **SQL Memory Limitation Configuration**:
  - Ensure SQL memory is capped to allow at least 8 GB for the operating system. This cap is configured in SQL Server properties. Typically, the 80/20 rule applies.

- **SQL Logging**:
  - Set logging to `Simple` mode in the `BFEnterprise` database for optimal performance.

- **Database/Logs/TempDB**:
  - Place each on different drives to optimize I/O performance.

- **Max Degree of Parallelism (MAXDOP)**:
  - Configure according to the Capacity and Planning Guide.
  - BF Server installer manages this as of version 10, but manual configuration is optional. Refer to documentation (5.19 PRF-INF-19).

- **Cost Threshold for Parallelism (CFTP)**:
  - Set to 50 for balanced performance.
  - Managed by the server installer as of version 10, but can be customized. Refer to documentation (5.19 PRF-INF-20).

- **Growth Settings**:
  - Configure database growth incrementally to avoid the "Woodpecker Effect." Ensure growth happens with purpose to minimize fragmentation. For example, grow by at least 2GB or 5% with each auto-growth. Adjusting from default settings helps manage large databases more efficiently.

- **Dedicated TempDB I/O Channel**:
  - TempDB is heavily used by the BigFix Enterprise DB when using snapshot isolation. TempDB is a shared instance-level resource. Be mindful of competing workloads. Isolate TempDB from other workloads when possible to improve performance.

- **Antivirus Exclusion for SQL Files**:
  - Exclude the SQL file storage location (including all data file sets) from virus scanning to improve performance and prevent file locking. Refer to your antivirus software's instructions for setting exclusion rules.

- **Avoid File Indexing and Compression on SQL Data Files**:
  - Exclude SQL data files from file indexing or compression, as it may lock files that SQL Server needs, potentially causing performance issues.

- **Configure SQL Maintenance Jobs to Update Statistics**:
  - Updating statistics in SQL Server helps optimize query performance by ensuring that the database engine has accurate information about the distribution of data within tables and indexes.
  - SQL Server's query optimizer relies on these statistics to create efficient query plans. Regular updates, depending on data change frequency, improve performance.

---

## Additional Performance Enhancements

- **Top 20 Largest Property Results**:
  - Use a SQL query to return the top largest properties. Refer to the [Largest Properties Query](https://github.com/mpaishon/BFSqlEntQueries/blob/main/BFE%20Largest%20Properties.md).

- **Database Fragmentation Levels**:
  - Run a SQL query to check for fragmentation levels regularly. Refer to the [Table and Index Stats Query](https://github.com/mpaishon/BFSqlEntQueries/blob/main/BFE%20Table%20and%20Index%20Stats.md).

---
