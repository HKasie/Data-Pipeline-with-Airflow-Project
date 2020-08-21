# Data-Pipeline-with-Airflow-Project
Create high grade data pipelines that are dynamic and built from reusable tasks, can be monitored, and allow easy backfills.

## Objective of the Airflow Pipeline
to create custom operators namely **stage, fact, dimension and data quality operators** to be implemented into functional pieces of a **data pipeline**, to perform tasks such as filling the data warehouse with date and running checks on the data as the final step. The operators, also contain a **set of tasks** that need to be linked to achieve a coherent and sensible data flow within the pipeline. The custom operators execute a helper class that contains all the **SQL transformations.**

### Stage Operator
The following are the uses of of the stage operator:
   * stage operator would be used to load JSON formatted files from S3 to Amazon Redshift. 
   * it would be used to create and run a SQL COPY statement based on the parameters provided. 
   * The operator's parameters would be used to specify where in S3 the file is loaded and what is the target table.
   * The parameters would be used to distinguish between JSON file. 
   * Finally the stage operator would contain a templated field that allows it to load timestamped files from S3 based on the execution time and run backfills.

### Fact and Dimension Operators
The dimension and fact operators can be utilized as follows:
   * to run data transformations using the provided SQL helper class. Most of the logic is within the SQL transformations.
   * operator takes as input the SQL statement and target database on which to run the query against. A target table is defined that to store the results.
   * Dimension loads are often done with the truncate-insert pattern where the target table is emptied before the load. Thus, you could also have a parameter that allows switching between insert modes when loading dimensions.
   * Fact tables are usually so massive that they should only allow append type functionality.

### Data Quality Operator
The final operator to create is the data quality operator which are used as follows:
* to run checks on the data itself.
* main functionality is to receive one or more SQL based test cases along with the expected results and execute the tests. For each the test, the test result and expected result needs to be checked and if there is no match, the operator should raise an exception and the task should retry and fail eventually.

For example one test could be a SQL statement that checks if certain column contains NULL values by counting all the rows that have NULL in the column. We do not want to have any NULLs so expected result would be 0 and the test would compare the SQL statement's outcome to the expected result.
