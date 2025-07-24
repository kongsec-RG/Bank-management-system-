# Bank-management-system-
import json
import os

DATA_FILE = "bank_data.json"

# Load accounts from file
def load_accounts():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    return {}

# Save accounts to file
def save_accounts(accounts):
    with open(DATA_FILE, "w") as f:
        json.dump(accounts, f, indent=4)

# Bank System Class
class BankSystem:
    def __init__(self):
        self.accounts = load_accounts()

    def create_account(self):
        acc_no = input("Enter new account number: ")
        if acc_no in self.accounts:
            print("❌ Account already exists!")
            return
        name = input("Enter account holder name: ")
        try:
            balance = float(input("Enter initial deposit: "))
        except ValueError:
            print("❌ Invalid amount!")
            return
        self.accounts[acc_no] = {"name": name, "balance": balance}
        save_accounts(self.accounts)
        print("✅ Account created successfully!")

    def deposit(self):
        acc_no = input("Enter account number: ")
        if acc_no not in self.accounts:
            print("❌ Account not found!")
            return
        try:
            amount = float(input("Enter amount to deposit: "))
        except ValueError:
            print("❌ Invalid amount!")
            return
        self.accounts[acc_no]["balance"] += amount
        save_accounts(self.accounts)
        print("✅ Deposit successful!")

    def withdraw(self):
        acc_no = input("Enter account number: ")
        if acc_no not in self.accounts:
            print("❌ Account not found!")
            return
        try:
            amount = float(input("Enter amount to withdraw: "))
        except ValueError:
            print("❌ Invalid amount!")
            return
        if amount > self.accounts[acc_no]["balance"]:
            print("❌ Insufficient balance!")
        else:
            self.accounts[acc_no]["balance"] -= amount
            save_accounts(self.accounts)
            print("✅ Withdrawal successful!")

    def check_balance(self):
        acc_no = input("Enter account number: ")
        if acc_no not in self.accounts:
            print("❌ Account not found!")
            return
        balance = self.accounts[acc_no]["balance"]
        name = self.accounts[acc_no]["name"]
        print(f"👤 Name: {name}")
        print(f"💰 Balance: ${balance:.2f}")

    def main_menu(self):
        while True:
            print("\n====== Bank Management System ======")
            print("1. Create Account")
            print("2. Deposit")
            print("3. Withdraw")
            print("4. Check Balance")
            print("5. Exit")
            choice = input("Enter your choice (1-5): ")

            if choice == "1":
                self.create_account()
            elif choice == "2":
                self.deposit()
            elif choice == "3":
                self.withdraw()
            elif choice == "4":
                self.check_balance()
            elif choice == "5":
                print("👋 Thank you for using the system. Goodbye!")
                break
            else:
                print("❌ Invalid choice! Please try again.")

# Run the program
if __name__ == "__main__":
    bank = BankSystem()
    bank.main_menu()
