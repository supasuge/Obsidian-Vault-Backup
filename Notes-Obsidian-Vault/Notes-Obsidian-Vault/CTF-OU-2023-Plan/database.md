## Table of Contents

    - [Script: init_db.sh](#Script:\init_db.sh)
- [Default Database file](#default\database\file)
- [Parse flags](#parse\flags)
- [Initialize SQLite Database](#initialize\sqlite\database)
    - [Script: insert_values.sh](#Script:\insert_values.sh)
- [Database file](#database\file)
- [Insert data based on flags provided](#insert\data\based\on\flags\provided)
- [Check if data was inserted](#check\if\data\was\inserted)
    - [S](#S)

### Script: init_db.sh
```bash
#!/bin/bash

# Default Database file
DB_FILE="database.db"

# Parse flags
while getopts ":o:" opt; do
  case $opt in
    o) DB_FILE=$OPTARG ;;
    \?) echo "Invalid option -$OPTARG" >&2; exit 1 ;;
  esac
done

# Initialize SQLite Database
sqlite3 $DB_FILE <<EOF
CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, username TEXT, password TEXT);
CREATE TABLE IF NOT EXISTS flag (id INTEGER PRIMARY KEY, text TEXT);
EOF

echo "Database initialized with tables: users, flag at $DB_FILE"

```
Usage: 
```bash
chmod +x init_db.sh && ./init_db.sh -o /path/to/your_database_name.db
```

### Script: insert_values.sh
```bash
#!/bin/bash
# Database file
DB_FILE="database.db"
inser_user() {
    sqlite3 $DB_FILE "INSERT INTO flag (id, text) VALUES (1, '$1');"
    echo "Inserted flag: $1"
}

if [[ $# -eq 0 ]]; then
    echo "No args provided. Usage: -u 'user' -p 'pass', -f for flag."
    exit 1
fi

while getopts ":u:p:f:" opt; do
    case $opt in
        u) USERNAME=$OPTARG ;;
        p) PASSWORD=$OPTARG ;;
        f) FLAG=$OPTARG ;;
        \?) echo "Invalid option -$OPTARG" >2& ;;
    esac
done

# Insert data based on flags provided
[ ! -z "$USERNAME" ] && [ ! -z "$PASSWORD" ] && insert_user "$USERNAME" "$PASSWORD"
[ ! -z "$FLAG" ] && insert_flag "$FLAG"

# Check if data was inserted
if [ -z "$USERNAME" ] && [ -z "$FLAG" ]; then
    echo "No data inserted. Use -u and -p to insert a user or -f to insert a flag."
fi

```



Sure, I can help you with that. We'll create two Bash scripts: one for initializing the SQLite database with the necessary tables (`init_db.sh`), and another for inserting values into the database (`insert_values.sh`).

### S

- To initialize the database:
    `./init_db.sh`
    
- To insert a user:
    `./insert_values.sh -u "username" -p "password"`
    
- To insert a flag:
    `./insert_values.sh -f "ctf{example}"`