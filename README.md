#Apriori Algorithm Implementation

This repository contains Python scripts for applying the Apriori algorithm to generate association rules from transaction data. The scripts are designed to process a dataset containing various items purchased in each transaction, with the goal of identifying frequent itemsets and deriving rules that predict item associations within transactions.

Overview
The Apriori algorithm is a classic algorithm used in data mining for extracting frequent itemsets with applications in market basket analysis, such as understanding customer purchase behavior. This project applies the Apriori algorithm to a dataset (testarules.csv) to discover such patterns. The repository includes scripts that handle data loading, preprocessing, parameter tuning, and the calculation of association rules.

Data Description
The dataset testarules.csv includes transactional data where each row represents a transaction and each column represents an item purchased in that transaction. The script preprocesses this data to convert it into a list of transactions, each being a list of items.


1. Importing Libraries
   
The script begins by importing necessary Python libraries:

pandas for data manipulation and analysis.

apyori, a Python library specifically designed to perform Apriori algorithm for extracting frequent itemsets and association rules.


import pandas as pd
from apyori import apriori

2. Data Loading
   
The dataset is loaded into a pandas DataFrame. This step involves reading a CSV file, which is expected to contain transactional data where each row represents a transaction and each column can potentially represent an item bought in that transaction.


data = pd.read_csv('/path/to/testarules.csv')

The script checks the first few rows of the DataFrame to understand its structure and ensure that data loading is successful:


print(data.head())


3. Data Preprocessing
   
This step is crucial as it transforms raw data into an appropriate format for the Apriori algorithm. The script converts the DataFrame into a list of transactions:

Each transaction is represented as a list of items.

NaN values (representing missing items in some transactions) are removed.


transactions = data.apply(lambda row: [item for item in row if pd.notna(item)], axis=1).tolist()

This lambda function iterates over each row, collecting all non-null items into a list, effectively cleaning the data by ignoring blank entries.

4. Applying the Apriori Algorithm
   
The Apriori algorithm is applied using the apyori.apriori method, which requires the list of transactions as input and parameters such as min_support, min_confidence, and min_lift:


results = list(apriori(transactions, min_support=0.0045, min_confidence=0.2, min_lift=3, min_length=2))

min_support determines how frequently a set of items must appear in the dataset to be considered significant.

min_confidence assesses the reliability of the inference made by the rule.

min_lift measures how much more often the antecedent and consequent of the rule occur together than expected if they were statistically independent.

min_length specifies the minimum number of items that must be present in a rule.


5. Outputting Results
   
After calculating the rules, the script iterates over the results to print out each rule along with its support, confidence, and lift:


for rule in results:

    antecedents = tuple(rule[0])
    
    support = rule[1]
    
    for detail in rule[2]:
    
        confidence = detail[2]
        
        lift = detail[3]
        
        print(f"Rule: {antecedents} -> {tuple(detail[1])}, Support: {support}, Confidence: {confidence}, Lift: {lift}")
        
        
This loop extracts and displays detailed information about each rule, helping users understand which items frequently co-occur in the dataset.

Apriori Algorithm Application

With transactions prepared, the script uses the apyori library to apply the Apriori algorithm. Parameters such as min_support, min_confidence, and min_lift are set to find meaningful rules without overwhelming the user with too many or irrelevant rules.

Output

The output includes the association rules found, displaying each rule's antecedents, consequents, support, confidence, and lift. These metrics help in understanding the strength and reliability of each rule derived from the dataset.
