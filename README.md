# data_masker_labs

## Prerequisites
To run this lab, you will need:
1. A clean SQL Server Instance where you have permission to create/drop databases, and insert/update/delete data. (SQL Server Developer edition is suitable and free.) This should not be a production SQL Server instance, and it should not contain any important databases or data.
2. Data Masker for SQL Server by Redgate. (A 14 day free trial is available here: https://www.red-gate.com/products/data-masker/. If you need a trial extension, contact sales@red-gate.com.) 

## LAB 1 - Build and run your first masking set
1. Execute the script DropAndCreateDmDatabase.sql on your SQL Server instance. This will drop and recreate a sample databas called DMDatabase. ***(WARNING: If you already have a database called DMDatabase, this script will drop your existing database! If you value that database, use a different SQL Server instance to run these labs!)*** Examine the data in the DM_CUSTOMER and DM_CUSTOMER_NOTES tables. Consider which data is sensitive and which columns to mask.
 This script is rerunnable, so you can run it as frequently as you like to 
2. Open Data Masker for SQL Server and create a new Masking Set.
3. Connect to SQL Server, and the DMDatabase database.
4. Go to the "Tables" tab. Expand the DM_CUSTOMER and DM_CUSTOMER_NOTES tables. Set appropriate Sensitivity labels for each column in each of the two tables.
5. Select all the sensitive columns in the DM_CUSTOMER table (SHIFT or CONTROL + left-click), select Add Rule / Susbstitution.
6. For each column, select an appropriate data set. Remember that for address data you can use a correlated data set.
7. Repeat steps 4-6 for the DM_CUSTOMER_NOTES table.
8. Click "Run Masking Set". Once completed, take another look at the data in the DM_CUSTOMER and DM_CUSTOMER_NOTES tables. You can reset the data by rerunning DropAndCreateDmDatabase.sql, and you can rerun the masking set by clicking the "Run Masking Set" button in the Data Masker for SQL Server UI.
9. Optional: Review the command line documentation, and attempt to run your maskign set via the command line: https://documentation.red-gate.com/dms/data-masker-help/general-topics/about-command-line-automation

## LAB 2 - Making a data set more "Truthy"
Hopefully, the masking set we create in LAB 1 masked the original data sufficiently well to meet our compliance requirements. However, it may not feel very "truthy". Consider the following questions:
1. Are all the names in the DM_CUSTOMER table gender appropriate?
2. Do the email addresses in the DM_CUSTOMER table reflect the names of the cusomters?
3. In the original data, the names in the DM_CUSTOMER and DM_CUSTOMER_NOTES matched, when joining on the customer_id column. (Re-run DropAndCreateDmDatabase.sql to verify this.) Do the names still match after running your masking set?

To solve this issues, complete the following tasks:
1. Ensure that the Substitution rule for the DM_CUSTOMER table uses a female names data set for first names, thn adda second Substitution rule, dependent on the first, that goes back over the first name column and uses a male dataset, with an appropriate WHERE clause.
2. Create a Row Internal rule to generate truthy email addresses, by incorporating the data in the name columns. Ensure that this rule runs *after* any rules that mask the name column have completed.
3. Create a Table-to-Table rule to sync the data in the DM_CUSTOMER table with the DM_CUSTOEMR_NOTES table. Ensure that this rule is set to run after the data in the DM_CUSTOMER and DM_CUSTOMER_NOTES tables have already been completely masked.
4. Re-run your masking set, and review the data in the tables.
5. Observe ExampleMaskSet.DMSMaskSet. This is an example masking set that appropriately masks the DM_CUSTOMER and DM_CUSTOMER_NOTES tables. Compare it with your own masking set, and consider any differences.
