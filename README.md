# Creating an indepth EDA project using Pandas, Numpy, Seaborn, Matplotlib


## In this github repository, I will showcase how I went about creating the retail company EDA project. 

1- First step is to import the libraries such as Pandas, Numpy, Seaborn, Matplotlib in this case as well as read both of the csv files. 
  - I have also used the .head() method to run the first five rows for both of the csv files.

  ```
import pandas as pd 
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
                                         
data = pd.read_csv('sales/retail_sales_dataset.csv')

  ```
2- The second step was to rename the columns from the train csv file to make them more readable. 

  ```
data = data.rename(columns={'Customer ID': 'Customer','Product Category': 'Category','Price per Unit': 'Unit Price'})

  ```

3- The next step was to find out any missing values that exist in dataset. Luckily in this case, there were no missing values


  ```
data.isnull().sum()


  ```

<img width="118" alt="Screenshot 2025-05-12 at 17 16 06" src="https://github.com/user-attachments/assets/2ef23263-e38c-49b0-8b91-0eab39d13900" />


4- After finding out the amount of null values, as well as finding out the different values using .describe() we found out that the date was set to object, we need to change it to date. 

  ```
data['Date'] = pd.to_datetime(data['Date'], format='%Y-%m-%d') # Changed the format to '%Y-%m-%d' to match the actual date format in the 'Date' column. This format represents dates as 'YYYY-MM-DD'.


  ```
<img width="351" alt="Screenshot 2025-05-12 at 17 18 32" src="https://github.com/user-attachments/assets/435b8e23-ea85-4e6e-b1fe-785da3a133d1" />


5- Before doing any indepth analysis, my main goal was to go over the dataset and note down the key statements that I would like to go over:

 ```
What to analyse in this project
multiplot bar graph showcasing the amount of women and men in the database
spliting and showing the product category in a pie chart ( with percentages)
calculating the total amount of each product category in a bar graph
try and do a scatterplot showcasing the sales between a certain time period
line graph showing product category buying age
top 3 ages that spend the most money (boxplot)
category with with the age that spent most money in each.
age group split into the spending amount using a split graph
Average quality amount bought in each product category
try and do aa heatmap with all of the digits that are included in this dataset.

  ```





6- Let's create the first box plot using the following code, use palette colours to make the visuals more appealing:


 ```
plt.figure(figsize=(10, 6))
sns.boxplot(x='Gender', y='Age', data=data,palette=['lightcoral','mediumslateblue'])

plt.title('Age Distribution by Gender')
plt.show()


  ```


<img width="673" alt="Screenshot 2025-05-12 at 17 21 09" src="https://github.com/user-attachments/assets/539dbaf0-4d04-4c12-b4e5-0185cba85891" />


7 - The second boxplot was also created to find out the age distribution by product category. Very similar code was used as before:

 ```
plt.figure(figsize=(10,6))
sns.boxplot(x='Category',y='Age',data=data, palette = ['greenyellow','aquamarine','navajowhite'])
plt.title('Age Distribution by Product Category')
plt.show()
  ```
<img width="674" alt="Screenshot 2025-05-12 at 17 22 10" src="https://github.com/user-attachments/assets/48e200dd-1903-4425-bc8e-b4673005f91c" />


8 - The third more varied box plot was used to find out the quantity distribution by sales amount :

 ```
plt.figure(figsize=(10,6))
sns.boxplot(x='Quantity',y='Total Amount',data=data, palette = ['greenyellow','aquamarine','navajowhite'])
plt.title('Quantity Distribution by Sales Amount')
plt.show()


  ```
<img width="685" alt="Screenshot 2025-05-12 at 17 23 22" src="https://github.com/user-attachments/assets/661fb4d8-91d4-48ba-937a-ae60e6371ce1" />



9 - To not forget, make sure to create a pie chart in order to highlight the amount of males and females in the DataFrame:


 ```
male_count = data.loc[data.Gender == 'Male']['Gender'].count()
print(male_count)
female_count = data.loc[data.Gender == 'Female']['Gender'].count()
print(female_count)
total = data.loc[(data.Gender == 'Male') | (data.Gender == 'Female')]['Gender'].count() 
print(total)


plt.pie([male_count, female_count], labels = ['Male', 'Female'], autopct = '%1.1f%%')
plt.title('Gender Distribution')
plt.show()

  ```

<img width="311" alt="Screenshot 2025-05-12 at 17 24 35" src="https://github.com/user-attachments/assets/7f92377e-c57a-41d0-92eb-e3e2f6d6e834" />


 



10 - Make sure to always create another (bar) graph showcasing the value amount (not percentages) which you can list next to your pie chart:


 ```
plt.bar(['Women','Men','Total'], [female_count, male_count,total],color=['red','blue','violet'])
plt.title('Gender Distribution')
for i, value in enumerate([female_count,male_count,total]):
    plt.text(i, value + 10, str(value), ha='center', fontsize=10)  # Adjust +10 if needed for spacing

plt.ylim(0, max([female_count,male_count,total]) + 50)  # Optional: add space above bars
plt.show()

plt.show()
  ```


<img width="446" alt="Screenshot 2025-05-12 at 17 25 40" src="https://github.com/user-attachments/assets/a387e240-b0d3-425d-9ae7-8db4326907ca" />


11 - For a more indepth analysis, we can also find out the amount of product sold by percentages in each category - pie chart:


 ```
category = data['Category'].unique()
print(category)

beauty = data.loc[data.Category == 'Beauty']['Category'].count()
print(beauty)

clothing = data.loc[data.Category == 'Clothing']['Category'].count()
print(clothing)

electronics = data.loc[data.Category == 'Electronics']['Category'].count()
print(electronics)


total_amount = data['Category'].count()
print(total_amount)

beauty_percentage = (beauty/total_amount)*100
print(beauty_percentage)

clothing_percentage = (clothing/total_amount)*100
print(clothing_percentage)

electronics_percentage = (electronics/total_amount)*100
print(electronics_percentage)

total_percentage = beauty_percentage,clothing_percentage,electronics_percentage
print(total_percentage)



plt.pie(total_percentage,labels = ['Beauty','Clothing','Electronics'],autopct='%1.1f%%', colors = ['salmon','lightskyblue','plum'])
plt.title('Product Amount in each Category')
plt.show()
  ```
<img width="336" alt="Screenshot 2025-05-12 at 17 27 12" src="https://github.com/user-attachments/assets/20c44737-09ae-4917-9a68-b37dfaeafa2e" />


12 - Like before, creating a value graph next to the percentage graph is important to show both scales next to eachother:



 ```
plt.bar(['Beauty','Clothing','Electronics'],[beauty,clothing,electronics],color=['salmon','lightskyblue','plum'])
plt.title('Total Amount of each Product Category')
  ```

<img width="438" alt="Screenshot 2025-05-12 at 17 28 22" src="https://github.com/user-attachments/assets/fe8b9dac-639f-4641-9dbe-69aea0da2666" />

13 - Due to this not being our first analysis project, we can also include more advanced techniques which will be listed below such as lineplot created in seaborn. 


 ```
plt.bar(['Beauty','Clothing','Electronics'],[beauty,clothing,electronics],color=['salmon','lightskyblue','plum'])
plt.title('Total Amount of each Product Category')

  ```
<img width="674" alt="Screenshot 2025-05-12 at 17 29 37" src="https://github.com/user-attachments/assets/36440a96-00d2-4091-9c28-fb1facbaafb0" />

14 - The next stage consisted of finding out the top 3 ages that spent most amount of money in this shop, and later plot everything in 3 values per age (intermedite level):

 ```
present_age = data['Age'].unique()
print(present_age)

total_amount_age = data.groupby('Age')['Total Amount'].sum().sort_values(ascending=False).head(3)
print(total_amount_age)


age34_category_amount = data.loc[data.Age == 34].groupby('Category')['Total Amount'].sum()
print(age34_category_amount)


age43_category_amount = data.loc[data.Age == 43].groupby('Category')['Total Amount'].sum()
print(age43_category_amount)

age51_category_amount = data.loc[data.Age == 51].groupby('Category')['Total Amount'].sum()
print(age51_category_amount)


total_ages = ['34','43','51']
bar_width = 0.2
x = range(len(total_ages))




beauty_values = [age34_category_amount[0],age43_category_amount[0],age51_category_amount[0]]
clothing_values = [age34_category_amount[1],age43_category_amount[1],age51_category_amount[1]]
electronics_values = [age34_category_amount[2],age43_category_amount[2],age51_category_amount[2]]



plt.bar(x,beauty_values,width=bar_width,label='Beauty',color='skyblue')
plt.bar([p + bar_width for p in x], clothing_values, width=bar_width, label='Clothing', color='green')
plt.bar([p + 2 * bar_width for p in x], electronics_values, width=bar_width, label='Electronics', color='red')
plt.title('Top 3 ages with the most sales')
plt.xlabel('Age')
plt.ylabel('Amount')
plt.xticks(ticks=[p + bar_width for p in x], labels=total_ages)
plt.legend()
plt.show()


  ```
<img width="464" alt="Screenshot 2025-05-12 at 17 31 43" src="https://github.com/user-attachments/assets/ec62dd6d-8239-4801-9a6f-50d37d7b056b" />



15 - Continuing, a lineplot has been done to label the most amount of money spent by customers over the half year period. This is important for clear presentation skills.

 ```
ata['Month'] = data['Date'].dt.month  
data = data[data['Month'] <= 6]       


monthly_sales = data.groupby(['Month', 'Category'])['Total Amount'].sum().reset_index()


plt.figure(figsize=(20,10))
sns.lineplot(data=monthly_sales, x='Month', y='Total Amount', hue='Category', marker='o')
plt.title('Amount of Sales Made per Month (Jan–Jun 2023)')
plt.xlabel('Month')
plt.ylabel('Total Sales Amount')
plt.xticks(range(1, 7), ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'])
plt.grid(True)
plt.show()

  ```
<img width="502" alt="Screenshot 2025-05-04 at 11 58 59" src="https://github.com/user-attachments/assets/1fba2269-4308-4378-8bfd-3c3df5422df1" />



16 - One of the most interesting information that I have learnt are the pallette colours use in seaborn and in matplotlib. Using them effectively clearly helps in making the visualisation more eye-appealing. I had to make the graph larger in order for the customer ( retail shop owner) to read the information more clearly. 

 ```
values = [one_person_family,two_people_family,three_people_family,four_people_family,five_people_family]

siblings = ['0 siblings','1 sibling','2 siblings','3 siblings','4 siblings']

colors = ['mediumspringgreen','springgreen','limegreen','green','darkgreen']

plt.figure(figsize=(10, 6))
bars = plt.bar(siblings,values,color = colors)
plt.title('Amount of people with different number of siblings')
plt.xlabel('Amount')
plt.ylabel('Number of siblings')
plt.show()

  ```
<img width="1101" alt="Screenshot 2025-05-12 at 17 35 04" src="https://github.com/user-attachments/assets/9026bd20-9154-4260-8f76-fe723a82bf46" />

17 - To find out the same information but over the year period, I have used a bar graph to showcase a side to side comparison. Very easy to read. 

 ```
data['YearMonth'] = data['Date'].dt.to_period('M')

# Group by Year-Month and sum Total Amount
monthly_sales = data.groupby('YearMonth')['Total Amount'].sum().reset_index()

# Plot
plt.figure(figsize=(10, 6))
plt.bar(monthly_sales['YearMonth'].astype(str), monthly_sales['Total Amount'], color='skyblue')
plt.xticks(rotation=45)
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.title('Monthly Sales')
plt.tight_layout()
plt.show()


  ```
<img width="791" alt="Screenshot 2025-05-12 at 17 36 13" src="https://github.com/user-attachments/assets/c433ba4c-397d-4cfc-a26e-7fda79d47b84" />

18 - To further analyse, I have created a bar graph focusing on the main sales month which was in this case May. We could then analyse which category sold the most in this month. 

 ```
cat = ['Beauty','Clothing','Electronics']
May = data.loc[data.YearMonth == '2023-05']
May_cat = May.groupby('Category')['Total Amount'].sum()


plt.figure(figsize=(10,8))
plt.bar(cat,May_cat,color =['olive','teal','plum'])
plt.title('May 2023 Sales')
plt.show()


  ```
<img width="678" alt="Screenshot 2025-05-12 at 17 37 11" src="https://github.com/user-attachments/assets/992368b7-846e-4002-a3ef-de2448621b75" />

19 - For the final graph, I have decided to use a box plot in order to show the average amount spent per category. .sum() and .count() has been used in order to find out the total amount of sales than we divided them together to find the average. Use different colours to make the plot more visual and eye-appealing:
 ```
beauty_sales = data[data['Category'] == 'Beauty']

# Sum the 'Total Amount' column
total_beauty_sales = beauty_sales['Total Amount'].sum()

total_beauty_count = beauty_sales['Total Amount'].count()

average_beauty_sales = total_beauty_sales / total_beauty_count

sum_beauty = print(f"This is the total amount for beauty sales: {total_beauty_sales}")


clothing_sales = data[data['Category'] == 'Clothing']

total_clothing_sales = clothing_sales['Total Amount'].sum()

total_clothing_count = clothing_sales['Total Amount'].count()

average_clothing_sales = total_clothing_sales / total_clothing_count


sum_beauty = print(f"This is the total amount for clothing sales: {total_clothing_sales}")


electronics_sales = data[data['Category'] == 'Electronics']

total_electronic_sales = electronics_sales['Total Amount'].sum()

total_electronic_count = electronics_sales['Total Amount'].count()

average_electronics_sales = total_electronic_sales / total_electronic_count



sum_electronics = print(f"This is the total amount for electronics sales: {total_clothing_sales}")




total_average = [average_beauty_sales,average_clothing_sales,average_electronics_sales]

average_label = ['Average Beauty Sales','Average Clothing Sales','Average Electronics Sales']


plt.figure(figsize=(10,6))
plt.bar(average_label,total_average,color=['darkorange','greenyellow','crimson']) # Changed code to remove unnecessary indexing.
plt.title('Average Quality Amount Bought in each Product Category')
plt.xlabel('Product Category')
plt.ylabel('Average Amount in $')

for i, value in enumerate(total_average):
    plt.text(i, value + 5, str(value), ha='center', fontsize=10)  # Adjust +10 if needed for spacing
plt.show()
  ```
<img width="680" alt="Screenshot 2025-05-12 at 17 40 11" src="https://github.com/user-attachments/assets/04edc1d4-4d04-462e-90fe-097e664ff029" />




## 22- This was my first ETA project but I am looking to expand my skills and analyse more in the nearest future. My main skills which I learnt in this project was the ability to split the date into seperate months as well as change the object format to date format. Furthemore, using a variety of graphs has helped me to make the presentation more clear for the customer. For any recommendations, please let me know! Thank you for reading.
