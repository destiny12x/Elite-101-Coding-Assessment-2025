# I did this code with codehs.com which is a website that allows me to write the code

# I added the sample restaurant table data structure to kind of give me a good understanding
restaurant_tables = [
    [0, 'T1(2)', 'T2(4)', 'T3(2)', 'T4(6)', 'T5(4)', 'T6(2)'],  # this shows us the table labels and their capacities
    [1, 'o', 'o', 'o', 'o', 'o', 'o'],  # slot 1 (all tables are free)
    [2, 'o', 'x', 'o', 'x', 'o', 'o'],  # slot 2 (some tables are occupied)
    [3, 'x', 'o', 'o', 'x', 'x', 'o'],  # slot 3 (some tables are occupied)
    # more rows can be added for other eslots...
]

# This will be Level 1 List All Free Tables for a Timeslot
def list_free_tables(restaurant_tables, timeslot):
    # Check if timeslot is within valid range
    if timeslot < 1 or timeslot >= len(restaurant_tables):2
    print("Invalid timeslot")
    return []

    free_tables = []  # List to store free table IDs

    # Loop through each table and check if it's free ('o')
    for col in range(1, len(restaurant_tables[0])):
        if restaurant_tables[timeslot][col] == 'o':  # If the table is free
            free_tables.append(restaurant_tables[0][col])  # Add the table to the list
    return free_tables  


# this will be my Level 2 Find One Table for a Party Size
def find_table_for_party(restaurant_tables, party_size, timeslot):
    # this loop will go through each table to find one that fits the party size and is free
    for col in range(1, len(restaurant_tables[0])):
        table_label = restaurant_tables[0][col]
        capacity = int(table_label.split('(')[1].split(')')[0])  # this can remove the seating capacity

        if capacity >= party_size and restaurant_tables[timeslot][col] == 'o':  # will tell me if the table is free
            return table_label  # Return the table label
    return None  # Return None if no table is found


# This is for my Level 3 Find where all Tables for a Party Size are caculated 
def find_all_tables_for_party(restaurant_tables, party_size, timeslot):
    suitable_tables = []  # List to store all suitable tables

    # I have to make a loop through each table so it can check its capacity and availability
    for col in range(1, len(restaurant_tables[0])):
        table_label = restaurant_tables[0][col]
        capacity = int(table_label.split('(')[1].split(')')[0])  # i can remove seating capacity

        if capacity >= party_size and restaurant_tables[timeslot][col] == 'o':  # If the table is free
            suitable_tables.append(table_label)  # Add the table to the list

    return suitable_tables


# Level 4: Combine Adjacent Tables for Larger Parties
def find_combinations_for_party(restaurant_tables, party_size, timeslot):
    combinations = []  # I have to list to store possible table combinations

    # Loop through each table and check adjacent ones
    for col in range(1, len(restaurant_tables[0]) - 1):  # Skip the last column for combination
        table_label_1 = restaurant_tables[0][col]
        table_label_2 = restaurant_tables[0][col + 1]
        capacity_1 = int(table_label_1.split('(')[1].split(')')[0])
        capacity_2 = int(table_label_2.split('(')[1].split(')')[0])

        # I have to check if the combination of the two adjacent tables can fit the party size I was given
        if capacity_1 + capacity_2 >= party_size and \
           restaurant_tables[timeslot][col] == 'o' and \
           restaurant_tables[timeslot][col + 1] == 'o':
            combinations.append([table_label_1, table_label_2])  # and I will add the combination to the list of the tables

    # I will also check for individual tables that will be large enough
    for col in range(1, len(restaurant_tables[0])):
        table_label = restaurant_tables[0][col]
        capacity = int(table_label.split('(')[1].split(')')[0])

        if capacity >= party_size and restaurant_tables[timeslot][col] == 'o':  # If the table is free
            combinations.append([table_label])

    return combinations


#  This will be the Bonus Friendly Output for Tables or Combinations
def format_table_message(table_combo):
    if len(table_combo) == 1:  # If it's only one table
        return f"Table {table_combo[0]} is free and can seat {int(table_combo[0].split('(')[1].split(')')[0])} people."
    else:  # this shows me if it's a combination of tables
        tables = " and ".join(table_combo)  # join table names with "and"
        total_capacity = sum([int(table.split('(')[1].split(')')[0]) for table in table_combo])
        return f"Tables {tables} together can seat {total_capacity} people."


# Example Usage

# Example: List Free Tables at Timeslot 1
free_tables = list_free_tables(restaurant_tables, 1)
print(free_tables)  # Output: ['T1(2)', 'T2(4)', 'T3(2)', 'T4(6)', 'T5(4)', 'T6(2)']

# Example: Find One Table for a Party of 3 at Timeslot 2
table_for_party = find_table_for_party(restaurant_tables, 3, 2)
print(table_for_party)  # Output: 'T2(4)' or similar

# Example: Find All Tables for a Party of 4 at Timeslot 2
tables_for_party = find_all_tables_for_party(restaurant_tables, 4, 2)
print(tables_for_party)  # Output: ['T2(4)', 'T5(4)']

# Example: Find Table Combinations for a Party of 6 at Timeslot 2
table_combos = find_combinations_for_party(restaurant_tables, 6, 2)
for combo in table_combos:
    print(format_table_message(combo))  # Output: "Tables T2(4) and T3(2) together can seat 6 people."