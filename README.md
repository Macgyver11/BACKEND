# BACKEND

$.ajax({
url: 'http://',
tipe: 'get',//get,post,put,delete
data:{},
beforesend:function(){

},
success:function(){

}
})




import json
import pymysql

# Convert the .txt file to a JSON file
with open('data.txt', 'r') as f:
    data = f.read().splitlines()
with open('data.json', 'w') as f:
    json.dump(data, f)

# Read the JSON file to get the table structure information
with open('table_structure.json', 'r') as f:
    table_structure = json.load(f)

# Connect to the MariaDB database
conn = pymysql.connect(host='localhost', user='root', password='password', db='mydb')
cursor = conn.cursor()

# Create a table in the database using the table structure information
create_table_query = f"CREATE TABLE {table_structure['table_name']} ({table_structure['columns']})"
cursor.execute(create_table_query)

# Load the data from the JSON file into the table
load_data_query = f"LOAD DATA INFILE 'data.json' INTO TABLE {table_structure['table_name']} FIELDS TERMINATED BY ','"
cursor.execute(load_data_query)

# Commit the changes and close the connection
conn.commit()
conn.close()