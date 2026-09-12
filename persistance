import json
import os

from riders import RIDERS

DATA_FILE = "food_delivery_data.json"


def default_data():
    riders = {}

    for rider_id, rider in RIDERS.items():
        riders[rider_id] = {
            "id": rider_id,
            "name": rider["name"],
            "available": rider["available"],
        }

    return {
        "orders": [],
        "riders": riders,
    }


def load_data():
    if not os.path.exists(DATA_FILE):
        print("No previous data file found. Starting a new system.")
        return default_data()

    try:
        with open(DATA_FILE, "r", encoding="utf-8") as file:
            data = json.load(file)

        if "orders" not in data:
            data["orders"] = []
        if "riders" not in data:
            data["riders"] = default_data()["riders"]

        print("Previous order records loaded successfully.")
        return data

    except (json.JSONDecodeError, OSError) as error:
        print("\nWARNING: Could not read the data file.")
        print("Reason:", error)
        print("Starting with a fresh data set.")

        try:
            backup = DATA_FILE + ".damaged"
            os.replace(DATA_FILE, backup)
            print(f"Damaged file preserved as {backup}.")
        except OSError:
            pass

        return default_data()


def save_data(data):
    temporary = DATA_FILE + ".tmp"

    try:
        with open(temporary, "w", encoding="utf-8") as file:
            json.dump(data, file, indent=4)

        os.replace(temporary, DATA_FILE)

    except OSError as error:
        print("Error while saving data:", error)
