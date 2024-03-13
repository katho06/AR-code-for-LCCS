# AR-code-for-LCCS
#EXAM NUMBER - 205626
# Import Libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

# -------AR1---------

data = pd.read_csv('arr.csv')

parameters = data[['Hours_Studied', 'Absence_Percentage', 'Hours_Slept']]
happiness = data['Happiness_Index']

parameters_train, parameters_test, happiness_train, happiness_test = train_test_split(parameters, happiness, test_size=0.2, random_state=0)

model = LinearRegression()

model.fit(parameters_train, happiness_train)

hapiness_pred = model.predict(parameters_test)

print("Model Completed")

def predict_happiness(hours_studied, absence_percentage, hours_slept):
    df = pd.DataFrame([[hours_studied, absence_percentage, hours_slept]],
                      columns=['Hours_Studied', 'Absence_Percentage', 'Hours_Slept'])
    return model.predict(df)[0]

print("")
print("Hello! Today I will predict your happiness based on the your percentage of absence hours you study and slept ")
hours_studied = int(input("Please tell me the amount of hours you study day! Can be any number from 1-9: "))
hours_slept = int(input("Please tell me the amount of hours you sleep. Any number between 4-10: "))
absence_percentage = float(input("What's your absence percentage? Please enter any number down to the decimal from 1-100: "))

predicted_happiness = round(predict_happiness(hours_studied, absence_percentage, hours_slept), 4)  
print("\n The Predicted Happiness Index based on numbers given: ", predicted_happiness)

# ---------AR2----------

# i have hardcoded the variables for ALL what if questions
# What If Q1 
print("------------------------------------------------------------------------------------------------------------------------------------------")
print("What if .... ? (1)")
print("If the hours a student studies is double that of the national average, would doubling the days missing from 20.2 to 40.4 make them happy? ")

# keeping number of hours slept to the average (7)
# national average of hours studied is 3, so 3*2 = 6
hours = 6
missing = 40.4
slept = 7

happiness_what_if_q1 = round(predict_happiness(hours, missing, slept) , 4) 
print("Happiness based on parameters of What If Question 1: ", happiness_what_if_q1)

if happiness_what_if_q1 > 5.5:
    print("Doubling the percentage of days missing for a student who studies double that of the national average increases happiness")
else:
    print("Doubling the percentage of days missing for a student who studies double that of the national average doesn't increase happiness")
    
# What If q2
    
print("------------------------------------------------------------------------------------------------------------------------------------------")
print("What if .... ? (2)")
print("If a students hours slept is kept at a constant (average), \nbut their percentage of days missing exceeds 60%, and hours they study is above the national average, are they happy?")

# percentage of days missing is hardcoded to 0.6 more than 60
# I've interpreted "above" as one our more than average

hours = 4
missing = 60.6
slept = 7

happiness_what_if_q2 = round(predict_happiness(hours, missing, slept) , 4) 
print("Happiness based on parameters of What If Question 2: ", happiness_what_if_q2)

if happiness_what_if_q2 > 5.5:
    print("Student is happy ")
else:
    print("Student is still not happy")
    
# --------AR3----------

import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D


variables = ['Happiness of student who studied double \nav yet increase days missing', 'Happiness of student who studies above average\n but has an absence of above 60%',]
what_ifs = [happiness_what_if_q1, happiness_what_if_q2]

plt.bar(variables, what_ifs)

plt.xlabel('Parameters used')
plt.ylabel('Happiness Index (1-10)')

plt.title('Outcomes of What If Questions')

plt.show()
