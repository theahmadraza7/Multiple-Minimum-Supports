Multiple Minimum Supports
Concept:
Mining with Multiple Minimum Supports:
Traditional association rule mining assumes that all items have the same nature or
frequency, which often isn’t true. Items may have vastly different frequencies.
● Example: In supermarkets, bread and milk are bought far more frequently than
cooking pans.
● Rare Item Problem:
○ High minsup might overlook rare items.
○ Low minsup risks combinatorial explosion, producing too many rules involving
frequent items.
Approach to Multiple Minimum Supports
● Assign individual Minimum Item Support (MIS) values to each item.
● Introduce a support difference constraint (φ) to avoid combining very frequent and
rare items in a single itemset.
Minsup of a Rule:
● The minsup is determined by the minimum MIS value of items in the rule.
○ Example:
■ MIS(bread) = 2%, MIS(shoes) = 0.1%, MIS(clothes) = 0.2%.
■ Rule "clothes ⟶ bread" does not satisfy minsup.
■ Rule "clothes ⟶ shoes" satisfies minsup.
Challenges and Solutions
● Downward Closure Property Invalidity:
○ Example: For items 1, 2, 3, 4 with MIS values ranging from 5%-20%, {1, 2}
might be infrequent, but {1, 2, 3} could be frequent.
● Solution:
○ Sort items by MIS and generate candidates using ordered itemsets.
Steps in the Algorithm
1. Frequent Itemset Generation:
○ Sort items by MIS.
○ Construct the seed list (L) to begin generating candidate itemsets.
○ Candidate generation and pruning follow a specific order based on MIS
values.
2. Rule Generation:
○ Generate rules from frequent itemsets.
○ Handle the Head-item Problem: Some rules may not have sufficient support
recorded, requiring careful item elimination to avoid re-scanning the data.
Task:
Objective
This manual provides step-by-step instructions for understanding and implementing
Multiple Minimum Support (MIS) in the context of association rule mining using Python.
Task Overview
Students will:
1. Generate synthetic transaction data.
2. Define and apply multiple minimum support values.
3. Calculate the support of each item.
4. Generate frequent itemsets based on MIS values.
5. Derive association rules from frequent itemsets.
Step-by-Step Instructions
Step 0: Imports
import itertools
import pandas as pd
Step 1: Generate Synthetic Transaction Data
Create a list of transactions. Each transaction is represented as a set of items.
Code Snippet:
transactions = [
{'Milk', 'Bread', 'Shoes'},
{'Milk', 'Bread', 'Clothes'},
{'Milk', 'Shoes'},
{'Clothes', 'Bread'},
{'Milk', 'Clothes', 'Shoes', 'Bread'},
{'Bread', 'Shoes'},
{'Clothes', 'Milk'},
]
Explanation:
● Each set represents a transaction.
Step 2: Define Minimum Item Support (MIS)
Define minimum support values for each item.
Code Snippet:
MIS = {
'Milk': 0.3, # 30%
'Bread': 0.2, # 20%
'Shoes': 0.1, # 10%
'Clothes': 0.15 # 15%
}
Explanation:
● Assign a minimum support threshold for each item.
Step 3: Calculate Support of Each Item
Create a function to compute the support for each item.
Code Snippet:
def calculate_support(transactions):
item_counts = {}
total_transactions = len(transactions)
for txn in transactions:
for item in txn:
item_counts[item] = item_counts.get(item, 0) + 1
support = {item: count / total_transactions for item, count in
item_counts.items()}
return support
support = calculate_support(transactions)
Explanation:
● Count the number of occurrences of each item.
● Calculate support as the ratio of item count to the total number of transactions.
Step 4: Generate Frequent Itemsets Based on MIS
Generate frequent itemsets that satisfy the MIS values.
Code Snippet:
def generate_frequent_itemsets(transactions, MIS, support):
L = [item for item in support if support[item] >= MIS[item]]
L.sort(key=lambda item: MIS[item])
frequent_itemsets = []
for k in range(1, len(L) + 1):
candidates = list(itertools.combinations(L, k))
valid_itemsets = []
for itemset in candidates:
min_mis = min(MIS[item] for item in itemset)
itemset_support = sum(1 for txn in transactions if
set(itemset).issubset(txn)) / len(transactions)
if itemset_support >= min_mis:
valid_itemsets.append(itemset)
if valid_itemsets:
frequent_itemsets.extend(valid_itemsets)
return frequent_itemsets
frequent_itemsets = generate_frequent_itemsets(transactions, MIS,
support)
Explanation:
● Generate all item combinations.
● Check if each combination's support meets its minimum MIS value.
Step 5: Generate Association Rules
Generate association rules from frequent itemsets based on a minimum confidence
threshold.
Code Snippet:
def generate_rules(frequent_itemsets, transactions, min_conf=0.6):
rules = []
for itemset in frequent_itemsets:
if len(itemset) > 1:
for i in range(1, len(itemset)):
for antecedent in itertools.combinations(itemset,
i):
consequent = tuple(set(itemset) -
set(antecedent))
support_antecedent = sum(1 for txn in
transactions if set(antecedent).issubset(txn)) / len(transactions)
support_itemset = sum(1 for txn in
transactions if set(itemset).issubset(txn)) / len(transactions)
confidence = support_itemset /
support_antecedent if support_antecedent > 0 else 0
if confidence >= min_conf:
rules.append((antecedent, consequent,
confidence))
return rules
rules = generate_rules(frequent_itemsets, transactions)
Explanation:
● For each frequent itemset, generate possible antecedents and consequents.
● Calculate confidence for each rule and filter based on the threshold.
Step 6: Display Results
Print the frequent itemsets and the generated rules.
Code Snippet:
print("Frequent Itemsets:")
for itemset in frequent_itemsets:
print(itemset)
print("\nAssociation Rules:")
for rule in rules:
print(f"{rule[0]} -> {rule[1]} (Confidence: {rule[2]:.2f})")
Expected Output:
● Frequent itemsets based on MIS.
● Association rules with confidence values.
For Practice:
● Implement this code on groceries_final.csv dataset used in lab 4
Conclusion
Students should now be able to:
1. Understand the concept of Multiple Minimum Support.
2. Calculate item support and identify frequent itemsets.
3. Generate and interpret association rules.
Libraries:
● Pandas
● Numpy
● Intertools
itertools is a built-in Python module that provides a collection of fast,
memory-efficient tools for creating and handling iterators. It is often used to perform
combinatorial operations, infinite iterations, and other advanced iteration patterns.
