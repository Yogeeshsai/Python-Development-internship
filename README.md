# Python-Development-internship
#It is a internship assignment
import os
import pandas as pd
import docx2txt
import re

# Function to extract email addresses from text
def extract_emails(text):
    emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
    return emails

# Function to extract phone numbers from text
def extract_phone_numbers(text):
    phone_numbers = re.findall(r'\b(?:0|\+?91)?[789]\d{9}\b', text)
    return phone_numbers

# Function to process CVs and extract information
def process_cv(cv_dir):
    cv_data = []
    for filename in os.listdir(cv_dir):
        if filename.endswith(".docx"):
            text = docx2txt.process(os.path.join(cv_dir, filename))
            email = extract_emails(text)
            phone_number = extract_phone_numbers(text)
            cv_data.append({
                'Filename': filename,
                'Email': ', '.join(email),
                'Phone Number': ', '.join(phone_number),
                'Text': text
            })
    return cv_data

# Directory containing CVs
cv_directory = "path_to_your_cv_directory"

# Process CVs and extract information
cv_info = process_cv(cv_directory)

# Create dataframe
df = pd.DataFrame(cv_info)

# Export dataframe to Excel
df.to_excel("cv_information.xlsx", index=False)
