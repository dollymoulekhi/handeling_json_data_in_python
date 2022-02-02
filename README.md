# Json columns Normalization Python:

## pd.read_json(): Read json file.


## json.load() : It is used to read JSON encoded data from a file and convert it into a Python dictionary.


## json.loads(): It can be used to parse a valid JSON string and convert it into a Python Dictionary.


## pandas.json_normalize: Normalize semi-structured JSON data into a flat table.


## For normalizing a json column having missing values:

1) Replace missing values with {}

df['column'].apply(lambda x: {} if pd.isna(x) else x)

2) 




`data = [
    {"id": 1, "name": {"first": "Coleen", "last": "Volk"}},
    {"name": {"given": "Mark", "family": "Regner"}},
    {"id": 2, "name": "Faye Raker"},
]`

`pd.json_normalize(data)`

## If you have exported the data from mongo DB and a column is nested having missing values, steps to follow for normalizing it are:
1) replace all the missing values with `'[]'`
2) Apply literal_eval on that column
3) Extract the zeroth index item from it, i.e data but not the list
4) Use json loads for loading that data
5) Use json_normalize for normalizing that data.

Code example:

```

merged_profile_details.loc[(merged_profile_details['preferredLocation'].isnull()),'preferredLocation']='[]'
merged_profile_details['preferredLocation']=merged_profile_details.preferredLocation.apply(lambda x: literal_eval(str(x)))
merged_profile_details['preferredLocation']=merged_profile_details['preferredLocation'].str[0]
json_struct = json.loads(merged_profile_details.to_json(orient="records"))
df_flat = pd.io.json.json_normalize(json_struct)
df_flat

```

You will get a properly expanded column :)
