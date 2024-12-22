// A Simple Java Project: Budget Expense Tracker
// This program allows users to track their expenses and manage a budget.

import java.util.ArrayList;
import java.util.Scanner;

class Expense {
    private String description;
    private double amount;

    public Expense(String description, double amount) {
        this.description = description;
        this.amount = amount;
    }

    public String getDescription() {
        return description;
    }

    public double getAmount() {
        return amount;
    }

    @Override
    public String toString() {
        return description + ": $" + String.format("%.2f", amount);
    }
}

class Budget {
    private double totalBudget;
    private ArrayList<Expense> expenses;

    public Budget(double totalBudget) {
        this.totalBudget = totalBudget;
        this.expenses = new ArrayList<>();
    }

    public void addExpense(String description, double amount) {
        if (amount <= 0) {
            System.out.println("Amount must be positive.");
            return;
        }

        expenses.add(new Expense(description, amount));
        System.out.println("Expense \"" + description + "\" added successfully!");
    }

    public void viewExpenses() {
        if (expenses.isEmpty()) {
            System.out.println("No expenses recorded.");
            return;
        }

        System.out.println("\nExpenses:");
        for (int i = 0; i < expenses.size(); i++) {
            System.out.println((i + 1) + ". " + expenses.get(i));
        }
    }

    public void deleteExpense(int index) {
        if (index > 0 && index <= expenses.size()) {
            Expense removed = expenses.remove(index - 1);
            System.out.println("Deleted expense: " + removed);
        } else {
            System.out.println("Invalid expense number.");
        }
    }

    public double getRemainingBudget() {
        double totalSpent = expenses.stream().mapToDouble(Expense::getAmount).sum();
        return totalBudget - totalSpent;
    }

    public void viewBudgetSummary() {
        double totalSpent = expenses.stream().mapToDouble(Expense::getAmount).sum();

        System.out.println("\nBudget Summary:");
        System.out.println("Total Budget: $" + String.format("%.2f", totalBudget));
        System.out.println("Total Spent: $" + String.format("%.2f", totalSpent));
        System.out.println("Remaining Budget: $" + String.format("%.2f", getRemainingBudget()));
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter your total budget: ");
        double totalBudget;
        try {
            totalBudget = Double.parseDouble(scanner.nextLine());
            if (totalBudget <= 0) {
                System.out.println("Budget must be a positive number.");
                return;
            }
        } catch (NumberFormatException e) {
            System.out.println("Invalid budget. Please enter a valid number.");
            return;
        }

        Budget budget = new Budget(totalBudget);

        while (true) {
            System.out.println("\nBudget Expense Tracker");
            System.out.println("1. Add Expense");
            System.out.println("2. View Expenses");
            System.out.println("3. Delete Expense");
            System.out.println("4. View Budget Summary");
            System.out.println("5. Exit");

            System.out.print("Enter your choice: ");
            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    System.out.print("Enter expense description: ");
                    String description = scanner.nextLine();

                    System.out.print("Enter expense amount: ");
                    double amount;
                    try {
                        amount = Double.parseDouble(scanner.nextLine());
                        if (amount <= 0) {
                            System.out.println("Amount must be positive.");
                            break;
                        }
                    } catch (NumberFormatException e) {
                        System.out.println("Invalid amount. Please enter a valid number.");
                        break;
                    }

                    budget.addExpense(description, amount);
                    break;

                case "2":
                    budget.viewExpenses();
                    break;

                case "3":
                    System.out.print("Enter the expense number to delete: ");
                    int index;
                    try {
                        index = Integer.parseInt(scanner.nextLine());
                        budget.deleteExpense(index);
                    } catch (NumberFormatException e) {
                        System.out.println("Invalid number. Please enter a valid expense number.");
                    }
                    break;

                case "4":
                    budget.viewBudgetSummary();
                    break;

                case "5":
                    System.out.println("Exiting Budget Expense Tracker. Goodbye!");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
