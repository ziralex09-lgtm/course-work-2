from collections import Counter


def sales_report(data, brief=False):
    orders = data["orders"]
    revenue = sum(order["total"] for order in orders)

    item_quantities = Counter()
    for order in orders:
        for item in order["items"]:
            item_quantities[item["name"]] += item["quantity"]

    print("\n--- SALES AND PERFORMANCE REPORT ---")

    if not orders:
        print("No orders have been recorded yet.")
    else:
        print(f"Today's total revenue: UGX {revenue:,.0f}")
        best_item, best_quantity = item_quantities.most_common(1)[0]
        print(
            f"Best-selling item: {best_item} "
            f"({best_quantity} units)"
        )

    status_counts = Counter(order["status"] for order in orders)

    print("\nORDERS BY STATUS")
    for status in [
        "Pending",
        "Preparing",
        "Out for Delivery",
        "Delivered",
    ]:
        print(f"{status:<20}: {status_counts.get(status, 0)}")

    if not brief:
        print(f"\nTotal orders: {len(orders)}")
