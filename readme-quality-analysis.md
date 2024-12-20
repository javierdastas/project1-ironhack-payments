# Project 1 - IronHack Payments

## Data Quality Analysis

This documentation presents only a summary of the data sets. To view more information about the data sets and their details visit the next link:

[View DataSets Details Documentation](./project_datasets/)

After knowing the previous information, we will describe in a little more detail the process carried out to study and analyze the data. For this, once the relevant data fields have been found and sorted, we perform a data quality validation to ensure the accuracy of the results. We perform this process by checking for empty values ​​or missing data.

To achieve this, we use the following code on the consolidated data:

```python
# Check for missing values in the DataFrame
missing_values = pd.isnull(data)

# Count missing values in each column
missing_counts = missing_values.sum()

# Count columns with missing values
columns_with_missing = missing_counts[missing_counts > 0].count()

# Check if all columns have missing values
all_columns_missing = missing_counts.all()

# Calculate the total number of missing values
total_missing_values = missing_counts.sum()

# Display the results
print("Missing Values in Each Column:\n", missing_counts)
print("\nNumber of Columns with Missing Values:", columns_with_missing)
print("All Columns Have Missing Values:", all_columns_missing)
print("\nTotal Missing Values in the DataFrame:", total_missing_values)
```

```console
Output:
Missing Values in Each Column:
```

| Column                  | Missing Values |
| ----------------------- | -------------- |
| fee_id                  | 0              |
| cash_request_id         | 0              |
| type                    | 0              |
| status                  | 0              |
| category                | 18,861         |
| total_amount            | 0              |
| reason                  | 0              |
| fee_created_at          | 0              |
| updated_at              | 0              |
| paid_at                 | 5,526          |
| from_date               | 13,291         |
| to_date                 | 13,291         |
| charge_moment           | 0              |
| cash_request_id.1       | 0              |
| cash_request_created_at | 0              |
| cash_request_amount     | 0              |

```console
Number of Columns with Missing Values: 4
All Columns Have Missing Values: False
Total Missing Values in the DataFrame: 50969
```

### Check Relation Between Datasets

- Identify possible relations between "cash request" & "fees"
- Check null and empty values
- Check duplicate values (groups)

```python
# Merge to relate the Datasets
fees_with_cash_requests = fees_df.merge(cash_requests_df, left_on='cash_request_id', right_on='id', how='left')

# Identify the nulls and duplicates values
missing_cash_requests = fees_with_cash_requests['id_y'].isnull().sum()
duplicates_in_fees = fees_df['cash_request_id'].duplicated().sum()

# Relation Summary between Datasets
relationship_summary = {
    "Total fees records": len(fees_df),
    "Total cash requests records": len(cash_requests_df),
    "Matched fees with cash requests": len(fees_with_cash_requests) - missing_cash_requests,
    "Unmatched fees (missing cash requests)": missing_cash_requests,
    "Duplicate cash_request_id in fees": duplicates_in_fees,
}

# Display the summary results
print("\nRELATIONS AND DATA PROBLEMS SUMMARY:")
for key, value in relationship_summary.items():
    print(f"  {key}: {value}")
```

```console
RELATIONS AND DATA PROBLEMS SUMMARY:
Total fees records: 21061
Total cash requests records: 23970
Matched fees with cash requests: 21057
Unmatched fees (missing cash requests): 4
Duplicate cash_request_id in fees: 8127
```
