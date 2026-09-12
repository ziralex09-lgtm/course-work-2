from menu_data import display_menu
from orders import create_order, update_order_status, display_active_orders
from reports import sales_report
from persistence import load_data, save_data


def print_banner():
    print("\n" + "=" * 72)
    print("          CAMPUS FOOD DELIVERY AND ORDER MANAGEMENT SYSTEM")
    print("=" * 72)


def show_main_menu():
    print("\nMAIN MENU")
    print("-" * 72)
    print("1. Display food menu")
    print("2. Create new order")
    print("3. Update order status")
    print("4. View active orders")
    print("5. View sales and performance report")
    print("6. Save data")
    print("0. Exit")
    print("-" * 72)


def main():
    data = load_data()
    print_banner()
    sales_report(data, brief=True)

    while True:
        show_main_menu()
        choice = input("Enter your choice: ").strip()

        if choice == "1":
            display_menu()
        elif choice == "2":
            create_order(data)
            save_data(data)
        elif choice == "3":
            update_order_status(data)
            save_data(data)
        elif choice == "4":
            display_active_orders(data)
        elif choice == "5":
            sales_report(data)
        elif choice == "6":
            save_data(data)
            print("Data saved successfully.")
        elif choice == "0":
            save_data(data)
            print("Data saved. Goodbye.")
            break
        else:
            print("Invalid choice. Enter a number from 0 to 6.")


if __name__ == "__main__":
    main()
