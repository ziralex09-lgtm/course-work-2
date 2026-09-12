from datetime import datetime
from menu_data import find_item
from riders import assign_rider, release_rider

STATUS_STAGES = ["Pending", "Preparing", "Out for Delivery", "Delivered"]


def generate_order_id(data):
    numbers = []
    for order in data["orders"]:
        try:
            numbers.append(int(order["order_id"].replace("ORD", "")))
        except (ValueError, AttributeError):
            pass
    return f"ORD{max(numbers, default=0) + 1:04d}"


def calculate_delivery_fee(subtotal, distance_km):
    if subtotal >= 50000:
        return 0
    if distance_km <= 2:
        return 2000
    elif distance_km <= 5:
        return 3000
    elif distance_km <= 10:
        return 5000
    return 8000


def get_positive_integer(prompt):
    while True:
        try:
            value = int(input(prompt))
            if value <= 0:
                print("Enter a number greater than zero.")
                continue
            return value
        except ValueError:
            print("Enter a valid whole number.")


def get_non_empty(prompt):
    while True:
        value = input(prompt).strip()
        if value:
            return value
        print("This field cannot be empty.")


def create_order(data):
    print("\n" + "=" * 72)
    print("CREATE NEW ORDER")
    print("=" * 72)

    customer = get_non_empty("Customer name: ")
    phone = get_non_empty("Customer phone: ")

    while True:
        try:
            distance = float(input("Delivery distance in kilometres: "))
            if distance < 0:
                print("Distance cannot be negative.")
                continue
            break
        except ValueError:
            print("Enter a valid distance.")

    print("\nEnter menu item codes. Type DONE when finished.")
    print("Examples: M01, M02, D01, S01")

    cart = {}

    while True:
        item_code = input("Item code: ").strip().upper()

        if item_code == "DONE":
            if not cart:
                print("Your order is empty. Add at least one item.")
                continue
            break

        category, item = find_item(item_code)

        if item is None:
            print("Invalid item code. Please check the menu.")
            continue

        quantity = get_positive_integer(
            f"Quantity of {item['name']}: "
        )

        if item_code in cart:
            cart[item_code]["quantity"] += quantity
        else:
            cart[item_code] = {
                "code": item_code,
                "name": item["name"],
                "category": category,
                "price": item["price"],
                "quantity": quantity,
            }

        print(f"Added {quantity} x {item['name']}.")

    subtotal = sum(
        item["price"] * item["quantity"]
        for item in cart.values()
    )
    delivery_fee = calculate_delivery_fee(subtotal, distance)
    total = subtotal + delivery_fee

    order = {
        "order_id": generate_order_id(data),
        "customer": customer,
        "phone": phone,
        "distance_km": distance,
        "items": list(cart.values()),
        "subtotal": subtotal,
        "delivery_fee": delivery_fee,
        "total": total,
        "status": "Pending",
        "rider_id": None,
        "rider_name": None,
        "created_at": datetime.now().isoformat(timespec="seconds"),
    }

    print("\n" + "-" * 60)
    print("ORDER SUMMARY")
    print("-" * 60)
    for item in order["items"]:
        line_total = item["price"] * item["quantity"]
        print(f"{item['name']} x {item['quantity']} = UGX {line_total:,}")
    print("-" * 60)
    print(f"Subtotal:     UGX {subtotal:,}")
    print(f"Delivery fee: UGX {delivery_fee:,}")
    print(f"TOTAL:        UGX {total:,}")

    if not assign_rider(data, order):
        print("Order was not submitted because no rider is available.")
        return False

    data["orders"].append(order)
    print(f"\nOrder {order['order_id']} created successfully.")
    return True


def update_order_status(data):
    print("\n--- UPDATE ORDER STATUS ---")

    if not data["orders"]:
        print("There are no orders.")
        return

    order_id = input("Enter order ID: ").strip().upper()

    order = next(
        (item for item in data["orders"] if item["order_id"] == order_id),
        None
    )

    if order is None:
        print("Order not found.")
        return

    current = order["status"]
    current_index = STATUS_STAGES.index(current)

    print(f"Current status: {current}")

    if current == "Delivered":
        print("This order is already delivered.")
        return

    next_status = STATUS_STAGES[current_index + 1]

    confirmation = input(
        f"Move order to '{next_status}'? (Y/N): "
    ).strip().upper()

    if confirmation != "Y":
        print("Status update cancelled.")
        return

    order["status"] = next_status
    order["updated_at"] = datetime.now().isoformat(timespec="seconds")

    print(f"Order {order_id} moved from {current} to {next_status}.")

    if next_status == "Delivered":
        release_rider(data, order)
        print(f"Rider {order['rider_name']} is now available.")


def display_active_orders(data):
    print("\n--- ACTIVE ORDERS ---")
    active = [
        order for order in data["orders"]
        if order["status"] != "Delivered"
    ]

    if not active:
        print("No active orders.")
        return

    for order in active:
        print("\n" + "-" * 60)
        print(f"Order ID: {order['order_id']}")
        print(f"Customer: {order['customer']}")
        print(f"Total: UGX {order['total']:,}")
        print(f"Rider: {order['rider_name']}")
        print(f"Status: {order['status']}")
