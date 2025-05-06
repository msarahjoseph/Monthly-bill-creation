This README contains explanation of the python code used to generate bill of items. 

from datetime import datetime  #Imports the datetime class from Python’s datetime module.
# datetime module converts string dates (like "2024-11-01") into actual date objects you can compare and manipulate.

from calendar import monthrange                      
# Imports monthrange which helps determine how many days are in a specific month.
#For example, monthrange(2025, 5) returns (calendar.Thursday, 31) — the first value is the weekday of May 1st, and the second (31) is the number of days in May.

from collections import defaultdict       
#Imports defaultdict from Python’s collections module.
#This is like a regular dictionary, but it auto-creates default values so you don’t get KeyError if a key doesn't exist yet.

#item_list is the predefined item list each contain index no, item code, sales description, quantity, rate, amount, start date and stop date

item_list = [
{
"idx": 1,
"item_code": "Executive Desk (4*2)",
"sales_description": "Dedicated Executive Desk",
"qty": 10,
"rate": "1000",
"amount": "10000",
"start_date": "2023-11-01",
"stop_date": "2024-10-17",
},
{
"idx": 2,
"item_code": "Executive Desk (4*2)",
"qty": "10",
"rate": "1080",
"amount": "10800",
"start_date": "2024-10-18",
"stop_date": "2025-10-31",
},
{
"idx": 3,
"item_code": "Executive Desk (4*2)",
"qty": 15,
"rate": "1080",
"amount": "16200",
"start_date": "2024-11-01",
"stop_date": "2025-10-31",
},
{
"idx": 4,
"item_code": "Executive Desk (4*2)",
"qty": 5,
"rate": "1000",
"amount": "5000",
"start_date": "2024-11-01",
"stop_date": "2025-10-31",
},
{
"idx": 5,
"item_code": "Manager Cabin",
"qty": 5,
"rate": 5000,
"amount": 25000,
"start_date": "2024-11-01",
"stop_date": "2025-10-31",
},
{
"idx": 6,
"item_code": "Manager Cabin",
"qty": 7,
"rate": "5000",
"amount": 35000,
"start_date": "2024-12-15",
"stop_date": "2025-10-31",
},
{
"idx": 7,
"item_code": "Manager Cabin",
"qty": 10,
"rate": 4600,
"amount": 46000,
"start_date": "2023-11-01",
"stop_date": "2024-10-17",
},
{
"idx": 8,
"item_code": "Parking (2S)",
"qty": 10,
"rate": 1000,
"amount": 10000,
"start_date": "2024-11-01",
"stop_date": "2025-10-31",
},
{
"idx": 9,
"item_code": "Parking (2S)",
"qty": 10,
"rate": 0,
"amount": 0,
"start_date": "2024-11-01",
"stop_date": "2025-10-31",
},
{
"idx": 10,
"item_code": "Executive Desk (4*2)",
"qty": "8",
"rate": "1100",
"amount": "8800",
"start_date": "2024-11-15",
"stop_date": "2025-01-31",
},
{
"idx": 11,
"item_code": "Manager Cabin",
"qty": "3",
"rate": "5200",
"amount": "15600",
"start_date": "2024-10-10",
"stop_date": "2024-11-10",
},
{
"idx": 12,
"item_code": "Conference Table",
"qty": 1,
"rate": "20000",
"amount": "20000",
"start_date": "2024-11-05",
"stop_date": "2024-11-20",
},
{
"idx": 13,
"item_code": "Parking (2S)",
"qty": 5,
"rate": "1000",
"amount": "5000",
"start_date": "2024-11-15",
"stop_date": "2025-02-28",
},
{
"idx": 14,
"item_code": "Reception Desk",
"qty": 2,
"rate": "7000",
"amount": "14000",
"start_date": "2024-11-01",
"stop_date": "2025-03-31",
},
{
"idx": 15,
"item_code": "Reception Desk",
"qty": 1,
"rate": "7000",
"amount": "7000",
"start_date": "2024-11-10",
"stop_date": "2024-11-25",
},
{
"idx": 16,
"item_code": "Breakout Area",
"qty": 3,
"rate": "3000",
"amount": "9000",
"start_date": "2024-01-01",
"stop_date": "2024-01-31",
}
]

# Below defines a function named generate_monthly_bill that takes two arguments; item list and target month. 
# It returns a dict representing grouped billing details.

def generate_monthly_bill(item_list: list, target_month: str) -> dict:
    """
    Generates a monthly bill from a list of rented items.

    Parameters:
        item_list (list): List of dictionaries with item info.
        target_month (str): Month in 'YYYY-MM' format (e.g., '2024-11').

    Returns:
        dict: Grouped items by code and total revenue.
    """

    # Step 1: When the user inputs target month as “2024-11”, 
    # This line parses the first day of that particular month; for e.g.; user enters "2024-11", 
    # Then datetime. strptime(target_month, "%Y-%m") converts the string month into a datetime object representing the first day of the month: 2024-11-01.
   


    month_start = datetime.strptime(target_month, "%Y-%m")   
    # Now 'month_start' holds first day of that month in "YYYY-MM-DD" format
    print(month_start)  #output:  2024-11-01 00:00:00


    # Step 2: Find the last day of the target month, 
    # monthrange() take 2 arguments; year and month and returns the weekday of         first day of that month and total no. of days in that month.
    _, days_in_month = monthrange(month_start.year, month_start.month)
    print(_, days_in_month)        # outputs:  4 30
    
   
 
    # Step 3: Set the ending date of the month (e.g., "2024-11-30")
    # This is done by replacing the day of 'month_start' by total no.of days in the month.
    month_end = month_start.replace(day=days_in_month)
    print(month_end)      # Output:  2024-11-30 00:00:00

  
  # Step 4: Prepare a dictionary that automatically groups items
    # Creates a dictionary that automatically initializes any new key (like "Manager Cabin") to {'qty': 0, 'amount': 0.0}.
    # This simplifies adding quantities and amounts later.
    grouped_items = defaultdict(lambda: {'qty': 0, 'amount': 0.0})
   


# Now moving on to the item list , gets the rate , quantity and amount , then calculates total revenue.

#Loop starts here
    # Step 5: Loops through every item in the list
    for item in item_list:
        # Get item details (some values may be strings)
   

     
        item_code = item.get("item_code")       # gets the item code
        qty = int(item.get("qty", 0))           # gets the item quantity; if  not present, defaults to 0; Converts it to an int in case it’s a string like "10".
       


        rate = float(item.get("rate", 0))       # gets the item rate; 
#Converts it to a float to allow mathematical operations; if not present, defaults to 0.0.
        amount = float(item.get("amount", 0))   # gets the item amount; #Converts it to a float; if not present, defaults to 0.0.

   

      
# Convert start and stop dates from string to datetime , given start_date and stop_date already present in the item_list.

        start_date = datetime.strptime(item.get("start_date"), "%Y-%m-%d")   
# Converts the string start date (e.g., "2024-11-01") into a datetime object.
        print(start_date)             # goes through the loop and prints start date of all 16 items in item_list.

        stop_date = datetime.strptime(item.get("stop_date"), "%Y-%m-%d")  
# Converts the string stop date (e.g., "2025-10-11") into a datetime object.
        print(stop_date)              # goes through the loop and prints stop date of all 16 items in item_list.
        
        
        # Step 6: Check if this item is active during the target month
        if start_date <= month_end and stop_date >= month_start:   
# It means the item’s date range overlaps with the target month; 'and ' implies checking both conditions, true if both satisfies.
# If True, add its quantity and amount to the grouped totals
            grouped_items[item_code]['qty'] += qty  
# Adds the quantity to the correct item group in the dictionary.
            grouped_items[item_code]['amount'] += amount 
 # Adds the billing amount to the item’s total in the grouped dictionary.
 # This process continues until it comes to the last item in the item_list. 
 # grouped_items[item_code]['qty'] stores quantity of current item in the target month and  grouped_items[item_code]['amount'] stores amount of current item in the target_month.
   
   #Loop ends here

    # Step 7: Calculate the total revenue (sum of all item amounts)
    total_revenue = sum(item['amount'] for item in grouped_items.values()) 
    # After looping, calculates the total revenue by summing up all the amounts from each grouped item.
    print(total_revenue) # Outputs the sum of all 'amounts' listed by     grouped_items[item_code]['amount'] in the for loop
    
   

 # Step 8: Return the grouped items and the total 
    return {
        " line_items ": dict(grouped_items),      
#Returns a dictionary with: "line_items": all items grouped by item code with their total quantity and amount.
        "  total_revenue": total_revenue          
#"total_revenue": the grand total of all amounts.

    }                                            
                                                  
   

bill = generate_monthly_bill(item_list, "2024-11")     
# Calls the function generate_monthly_bill by passing two arguments; item_list and "2024-11"(target_month)
print(bill)

