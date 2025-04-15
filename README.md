# py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
personal_expenses= pd.read_excel(r"personal expenses.xlsx")
personal_expenses.shape
personal_expenses.describe()
personal_expenses.isna().sum()
personal_expenses = personal_expenses.drop(columns=['Conversion rate from source/accounting currency to ils',
    'comments',
    'Labeling',
    'Discount_club',
    'Discount_key'])
    personal_expenses["Debit_currency"].unique
    personal_expenses["Original_transaction_currency"].unique
    personal_expenses.transaction_type.unique()
    personal_expenses.drop(columns='Original_transaction_currency', inplace=True)
    personal_expenses[personal_expenses["debit_amount"]!=personal_expenses["Original_transaction_amount"]]
    personal_expenses.loc[82, 'debit_amount'] = personal_expenses.loc[82, 'Original_transaction_amount']
    personal_expenses.drop(columns='Original_transaction_amount', inplace=True)
    personal_expenses.The_way_the_transaction_is_carried_out.unique()
    personal_expenses.transaction_date = pd.to_datetime(personal_expenses.transaction_date, format='mixed')
# sorting personal_expenses by transaction_date
personal_expenses=personal_expenses.sort_values(by="transaction_date")
#resting the index numbers
personal_expenses.reset_index(drop=True, inplace=True)
personal_expenses['Name_of_the_business'].unique()
ersonal_expenses['Name_of_the_business'].replace("Transfer in BIT BIP", "Transfer in BIT", inplace=True)
debit_amount_by_category = personal_expenses.debit_amount.groupby(personal_expenses.category).sum().sort_values()
sns.set_style("whitegrid")

# Plot the bar graph
plt.figure(figsize=(10, 6))
ax = debit_amount_by_category.plot(kind='bar', color='skyblue')

# Adding title, labels, and xticks settings
plt.title('Money Spent per Category')
plt.xlabel('Category')
plt.ylabel('Money Spent')
plt.xticks(rotation=45, ha='right')

# Adding value annotations
for p in ax.patches:
    # Get the height of the bar
    height = p.get_height()
    # Add the text annotation
    ax.text(p.get_x() + p.get_width() / 2, height, f'₪{height:.2f}', 
            ha='center', va='bottom')

# Show the plot
plt.show()


debit_amount_by_month =personal_expenses.debit_amount.groupby(personal_expenses.Billing_date.dt.month).sum()

plt.figure(figsize=(10, 6))
plt.title("Money spent over time")
plt.xticks(ticks=personal_expenses.Billing_date.dt.month.unique(), labels=personal_expenses.Billing_date.dt.month_name().unique())
plt.xlabel('Month')
plt.ylabel('Total money spent')
plt.plot(debit_amount_by_month, 'o--c')
payment_choice_distribution = personal_expenses['The_way_the_transaction_is_carried_out'].value_counts()

# Plot
plt.figure(figsize=(10, 7))
plt.pie(payment_choice_distribution, 
        labels=payment_choice_distribution.index, 
        autopct='%1.1f%%', 
        startangle=140, 
        colors=plt.cm.Paired(range(len(payment_choice_distribution))))
plt.title('Distribution of Payment Choices')
plt.show()
