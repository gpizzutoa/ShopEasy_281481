# ShopEasy AI Model

## Team Members
- Gianfranco Pizzuto 281481
- Member 2
- Member 3


## Section 1: Introduction
Briefly describe your project here.

## Section 2: Methods
Describe your proposed ideas, such as features, algorithms, training overview, design choices, etc., and your environment here. Ensure that:
- A reader can understand why you made your design decisions.
- A reader should be able to recreate your environment (include commands like `conda list`, `conda env export`, etc.).
- Consider including a figure illustrating your ideas, like a flowchart of your machine learning system. (Place it in the "images" folder and link it here using `![Flowchart Description](images/flowchart.png)`).

## Section 3: Experimental Design
Describe the experiments conducted to validate the target contributions of your project:

### Purpose
1-2 sentence high-level explanation of each experiment.

### Baselines
Describe the methods used for comparison.

### Evaluation Metrics
List which metrics were used and why.

## Section 4: Results
Describe your main findings and conclusions drawn from the work:

- Main findings: report final results here.
- Include placeholder figures and tables to communicate findings (use Markdown image and table syntax).

Ensure that all figures containing results are generated from the code.

## Section 5: Conclusions
Concluding remarks:

- Summarize the key take-away from your work in one paragraph.
- Discuss any questions not fully answered by your work and potential future directions.

# main.ipynb

Ensure that your `main.ipynb` notebook includes:

- Alternating text and code cells from start to finish.
- Descriptions above each code cell explaining the intent and choice of the code below.
- Explanations of the outputs of each cell, especially figures, such that a reader understands what they are about to see.

# Additional Resources

## Images Folder
All figures displayed in the `README.md` should be stored in the `images` folder.


# Project Description: ShopEasy
Imagine a platform named ShopEasy, a leading e-commerce site that sells a variety of products, from 
books and gadgets to furniture and fashion. Over the years, they have amassed a vast amount of user 
data. This data is a gold mine of insights waiting to be discovered. ShopEasy aims to provide 
personalized user experiences, special promotions, and improved services. But to do this effectively, 
they first need to understand the buying habits and behaviors of their customers. By applying 
segmentation to this dataset, ShopEasy aims to uncover these hidden patterns and provide an 
enhanced, personalized shopping experience for its users.

## Dataset Features
- **personId:** Unique identifier for each user on the platform
- **accountTotal:** Total amount spent by the user on ShopEasy since their registration
- **frequencyIndex:** Reflects how frequently the user shops, with 1 being very frequent and values less than 1 being less frequent
- **itemCosts:** Total costs of items purchased by the user
- **singleItemCosts:** Costs of items that the user bought in a single purchase without opting for installments
- **multipleItemCosts:** Costs of items that the user decided to buy in installments
- **emergencyFunds:** Amount that the user decided to keep as a backup in their ShopEasy wallet for faster checkout or emergency purchases
- **itemBuyFrequency:** Frequency with which the user makes purchases
- **singleItemBuyFrequency:** How often the user makes single purchases without opting for installments
- **multipleItemBuyFrequency:** How often the user opts for installment-based purchases
- **emergencyUseFrequency:** How frequently the user taps into their emergency funds
- **emergencyCount:** Number of times the user has used their emergency funds
- **itemCount:** Total number of individual items purchased by the user
- **maxSpendLimit:** The maximum amount the user can spend in a single purchase, set by ShopEasy based on user's buying behavior and loyalty
- **monthlyPaid:** Total amount paid by the user every month
- **leastAmountPaid:** The least amount paid by the user in a single transaction
- **paymentCompletionRate:** Percentage of purchases where the user has paid the full amount
- **accountLifespan:** Duration for which the user has been registered on ShopEasy
- **location:** User's city or region
- **accountType:** The type of account held by the user. Regular for most users, Premium for those who have subscribed to ShopEasy premium services, and Student for users who have registered with a student ID
- **webUsage:** A metric (0-100) indicating the frequency with which the user shops on ShopEasy via web browsers. A higher number indicates more frequent web usage

## Assignment
- **Perform an Explanatory data analysis (EDA)** with visualization using the entire dataset.
- **Preprocess the dataset** (remove duplicates, encode categorical features with one hot encoding, not necessarily in this order).
- **Define the problem type** (regression, classification, or clustering), explain your choice, and design your model accordingly. Test at least 2 different models.
- **Identify the proper number of segments**, and evaluate different options.
- **Describe the properties of the segments** you have identified.
- **Describe the properties of the customers** belonging to each segment.
