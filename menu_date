MENU = {
    "Meals": {
        "M01": {"name": "Chicken and Chips", "price": 12000},
        "M02": {"name": "Beef Pilau", "price": 10000},
        "M03": {"name": "Rice and Beans", "price": 7000},
        "M04": {"name": "Chapati and Beef", "price": 9000},
    },
    "Drinks": {
        "D01": {"name": "Soda", "price": 3000},
        "D02": {"name": "Fresh Juice", "price": 5000},
        "D03": {"name": "Mineral Water", "price": 2000},
    },
    "Snacks": {
        "S01": {"name": "Samosa", "price": 2000},
        "S02": {"name": "Mandazi", "price": 1500},
        "S03": {"name": "Rolex", "price": 6000},
    },
}


def display_menu():
    print("\n" + "=" * 72)
    print("CANTEEN MENU")
    print("=" * 72)
    for category, items in MENU.items():
        print(f"\n{category}")
        print("-" * 50)
        for code, item in items.items():
            print(f"{code:<6} {item['name']:<25} UGX {item['price']:>8,}")


def find_item(item_code):
    item_code = item_code.upper()
    for category, items in MENU.items():
        if item_code in items:
            return category, items[item_code]
    return None, None
