import os
import pandas as pd

path = r"C:\Users\.../"
master_file = "test"
file_type = ".xlsx"
output_path = path
result_file = "test_output"
result_file_type = ".xlsx"

df = pd.read_excel(path + master_file + file_type, sheet_name = "Sheet1", header = None)

#Abover, you can give usecols argument if you knowwhat exact columns you are going to parse. Let's assume that we don't know the columns that we want to work on our dataframe.
input_number = len(df.columns)
def arange_size(x, y):
    x = x.iloc[:, 0:y]
    x = x.T
    x.columns = x.iloc[0]
    x = x[1:]
    return x
df = arange_size(df, input_number)

total = df[['page_no','question_no_in_page']]
df.drop(["page_no" ,"question_no_in_page", "person"], axis = 1, inplace = True)

new_df = pd.DataFrame()
for i in range(0, len(df.columns)):
    keep_answers = df.iloc[:, i]
    consolide = pd.concat([total, keep_answers], axis = 1)
    consolide = consolide.T.reset_index(drop = True).T
    new_df = new_df.append(consolide)
new_df.reset_index(drop = True, inplace = True)
new_df.rename(columns = {0: 'page_no', 1: 'question_no_in_page', 2: 'answers'}, inplace = True)

persons = df.columns
repeat_num = len(df)
raw_persons = pd.DataFrame(persons.repeat(repeat_num))
raw_persons.rename(columns = {0: "email"}, inplace = True)
raw_data = pd.concat([raw_persons, new_df], axis = 1)

#Here is optional.

def user_option(subtraction):
    while True:
        user_input = int(input("If you want to reduce by the given number, please enter 1. If not, please enter 0: "))
        if user_input not in (0, 1):
            print('Invalid given. Please do not enter a value other than 0 or 1!')
            continue
        elif user_input == 1:
            raw_data['answers'] = raw_data['answers'] - subtraction
            raw_data.dropna(subset = ['answers'], axis = 0, inplace = True)
            print('Answers has reduced by {subtraction}!')
            break
        elif user_input == 0:
            print('Answers did not change!')
            break
        else:
            continue
user_option(1)
raw_data.to_excel(os.path.join(os.path.dirname(output_path), result_file + result_file_type), 
              header = True, index = False, sheet_name = "raw_data")
