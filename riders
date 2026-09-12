RIDERS = {
    "R01": {"id": "R01", "name": "Alex", "available": True},
    "R02": {"id": "R02", "name": "Brian", "available": True},
    "R03": {"id": "R03", "name": "Charles", "available": True},
    "R04": {"id": "R04", "name": "David", "available": True},
}


def available_riders(data):
    return [
        rider for rider in data["riders"].values()
        if rider["available"]
    ]


def display_available_riders(data):
    riders = available_riders(data)
    print("\nAVAILABLE RIDERS")
    print("-" * 45)
    if not riders:
        print("No riders are currently available.")
        return
    for rider in riders:
        print(f"{rider['id']}: {rider['name']}")


def assign_rider(data, order):
    riders = available_riders(data)
    if not riders:
        print("No rider is currently available.")
        return False

    display_available_riders(data)

    while True:
        rider_id = input("Enter rider ID: ").strip().upper()
        rider = data["riders"].get(rider_id)

        if rider is None:
            print("Invalid rider ID.")
            continue
        if not rider["available"]:
            print("That rider is not currently available.")
            continue

        rider["available"] = False
        order["rider_id"] = rider_id
        order["rider_name"] = rider["name"]
        print(f"Rider {rider['name']} assigned to {order['order_id']}.")
        return True


def release_rider(data, order):
    rider_id = order.get("rider_id")
    if rider_id in data["riders"]:
        data["riders"][rider_id]["available"] = True
