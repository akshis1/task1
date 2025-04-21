import pandas as pd

# Step 1: Load the data
df = pd.read_csv(r"C:\Users\vimal\Desktop\netflix.csv")

# Step 2: View basic info
print(df.info())
print(df.head())

# Step 3: Check for missing value1
missing_values = df.isnull().sum()
print("Missing values:\n", missing_values)

# Step 4: Fill or drop missing values
# Fill missing 'country' with 'Unknown'
df['country'] = df['country'].fillna('Unknown')
# Fill missing 'director' with 'Not Specified'
df['director'] = df['director'].fillna('Not Specified')



# Drop rows where 'title' or 'type' is missing (critical info)
df.dropna(subset=['title', 'type'], inplace=True)

# Step 5: Convert 'date_added' to datetime

df['date_added'] = pd.to_datetime(df['date_added'].astype(str).str.strip(), format='mixed', errors='coerce')

# Drop garbage column
df.drop(columns=['Unnamed: 11'], inplace=True)


# Step 6: Trim whitespace from string columns
string_cols = df.select_dtypes(include='object').columns
df[string_cols] = df[string_cols].apply(lambda x: x.str.strip())

# Step 7: Split duration into numeric + unit
df[['duration_int', 'duration_unit']] = df['duration'].str.extract(r'(\d+)\s*(\w+)')
df['duration_int'] = pd.to_numeric(df['duration_int'])

# Step 8: Normalize column names (optional)
df.columns = df.columns.str.lower().str.replace(' ', '_')

# Step 9: Remove duplicates
df.drop_duplicates(inplace=True)

# Step 10: Final check
print(df.info())
print(df.head())
