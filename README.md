from datetime import datetime

class Expense:
    def __init__(self, amount, date, category, description=""):
        self.amount = amount
        self.date = date
        self.category = category
        self.description = description

    def __repr__(self):
        return f"{self.date} | {self.category} | ${self.amount:.2f} | {self.description}"

class ExpenseTracker:
    def __init__(self):
        self.expenses = []
        self.categories = set()

    def add_category(self, category_name):
        self.categories.add(category_name)

    def add_expense(self, amount, date, category, description=""):
        expense = Expense(amount, date, category, description)
        self.expenses.append(expense)
        print(f"Added expense: {expense}")

    def view_expenses(self):
        print("\nAll Expenses:")
        for expense in self.expenses:
            print(expense)

    def filter_expenses_by_category(self, category):
        filtered_expenses = [exp for exp in self.expenses if exp.category == category]
        print(f"\nExpenses in category '{category}':")
        for expense in filtered_expenses:
            print(expense)

    def generate_report(self, period="monthly"):
        report = {}
        for expense in self.expenses:
            period_key = self._get_period_key(expense.date, period)
            if period_key not in report:
                report[period_key] = 0
            report[period_key] += expense.amount

        print(f"\n{period.capitalize()} Report:")
        for period_key, total in report.items():
            print(f"{period_key}: ${total:.2f}")

    def _get_period_key(self, date, period):
        date_obj = datetime.strptime(date, "%Y-%m-%d")
        if period == "daily":
            return date_obj.strftime("%Y-%m-%d")
        elif period == "weekly":
            return f"{date_obj.year}-W{date_obj.isocalendar()[1]}"
        elif period == "monthly":
            return date_obj.strftime("%Y-%m")
        else:
            raise ValueError("Invalid period. Choose 'daily', 'weekly', or 'monthly'.")

def main():
    tracker = ExpenseTracker()

    # Automatically add categories
    tracker.add_category("Groceries")
    tracker.add_category("Utilities")
    tracker.add_category("Entertainment")

    # Automatically add test expenses
    tracker.add_expense(100, "2024-10-01", "Groceries", "Weekly shopping")
    tracker.add_expense(50, "2024-10-02", "Utilities", "Electricity bill")
    tracker.add_expense(75, "2024-10-03", "Entertainment", "Movie tickets")

    # Execute all required functions for testing
    tracker.view_expenses()
    tracker.filter_expenses_by_category("Groceries")
    tracker.generate_report("monthly")

    print("\nAutomated test completed successfully.")

if __name__ == "__main__":
    main()
