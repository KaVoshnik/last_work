#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <fstream>
#include <string>
#include <ctime>
#include <iomanip>
#include <vector>
#include <map>
#include <sstream>


    using namespace std;

//структура для хранения инфы о трате или доходе
struct Transaction {
    string category;
    string description;
    double amount;
    time_t time;
    bool isExpense; //если true траты else доход
};

//траты или доход в файл
void writeTransactionToFile(Transaction transaction, string filename) {
    ofstream file(filename, ios::app);
    if (file.is_open()) {
        file << transaction.category << "," << transaction.description << "," << transaction.amount
            << "," << transaction.time << "," << (transaction.isExpense ? "expense" : "income") << endl;
        file.close();
    }
    else {
        cout << "Unable to open file " << filename << endl;
    }
}

//функция для чтения трат или доходоа из файла
vector<Transaction> readAllTransactionsFromFile(string filename) {
    vector<Transaction> transactions;
    ifstream file(filename);
    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            Transaction transaction;
            stringstream ss(line);
            getline(ss, transaction.category, ',');
            getline(ss, transaction.description, ',');
            ss >> transaction.amount;
            ss.ignore(1, ',');
            ss >> transaction.time;
            ss.ignore(1, ',');
            string expenseOrIncome;
            getline(ss, expenseOrIncome);
            if (expenseOrIncome == "expense") {
                transaction.isExpense = true;
            }
            else if (expenseOrIncome == "income") {
                transaction.isExpense = false;
            }
            transactions.push_back(transaction);
        }
        file.close();
    }
    else {
        cout << "Unable to open file " << filename << endl;
    }
    return transactions;
}

//функция для расчета баланса на всех счетах
double calculateBalance(vector<Transaction> transactions) {
    double balance = 0;
    for (Transaction transaction : transactions) {
        if (transaction.isExpense) {
            balance -= transaction.amount;
        }
        else {
            balance += transaction.amount;
        }
    }
    return balance;
}

//функция для расчета % для каждой категории трат от всех трат за месяц
void calculateCategoryExpensePercentageLastMonth(vector<Transaction> transactions) {
    time_t now = time(nullptr);
    tm timeinfo = *localtime(&now);
    int currentMonth = timeinfo.tm_mon;
    int currentYear = timeinfo.tm_year + 1900;
    double totalExpensesLastMonth = 0;
    map <string, double> categoryExpensesLastMonth;
    for (Transaction transaction : transactions) {
        tm transactionTimeinfo = *localtime(&(transaction.time));
        int transactionMonth = transactionTimeinfo.tm_mon;
        int transactionYear = transactionTimeinfo.tm_year + 1900;
        if (transaction.isExpense && transactionMonth == currentMonth && transactionYear == currentYear) {
            totalExpensesLastMonth += transaction.amount;
            categoryExpensesLastMonth[transaction.category] += transaction.amount;
        }
    }
    cout << "Category expenses last month:" << endl;
    for (auto it = categoryExpensesLastMonth.begin(); it != categoryExpensesLastMonth.end(); ++it) {
        cout << fixed << setprecision(2) << it->first << ": " << it->second * 100 / totalExpensesLastMonth << "%" << endl;
    }
}

double totalExpensesAllTime = 0;
map<string, double> categoryExpensesAllTime;

void calculateCategoryExpensePercentageAllTime(vector<Transaction> transactions) {
    categoryExpensesAllTime.clear();
    totalExpensesAllTime = 0;

    for (Transaction transaction : transactions) {
        if (transaction.isExpense) {
            totalExpensesAllTime += transaction.amount;
            categoryExpensesAllTime[transaction.category] += transaction.amount;
        }
    }

    cout << "Category expenses all time:" << endl;

    for (auto it = categoryExpensesAllTime.begin(); it != categoryExpensesAllTime.end(); ++it) {
        cout << fixed << setprecision(2) << it->first << ": " << it->second * 100 / totalExpensesAllTime << "%" << endl;
    }
}

int main() {
    string filename = "transactions.csv";

    Transaction expense1 = { "food", "groceries", 50.23, time(nullptr), true }; // текущее время
    writeTransactionToFile(expense1, filename);
    Transaction income1 = { "salary", "monthly salary", 5000, time(nullptr), false }; // текущее время
    writeTransactionToFile(income1, filename);
    Transaction expense2 = { "entertainment", "movie tickets", 25.0, time(nullptr), true };
    writeTransactionToFile(expense2, filename);
    Transaction expense3 = { "transportation", "gasoline", 35.0, time(nullptr), true };
    writeTransactionToFile(expense3, filename);
    Transaction expense4 = { "food", "dinner at a restaurant", 70.0, time(nullptr), true };
    writeTransactionToFile(expense4, filename);
    Transaction income2 = { "freelance", "website development", 1000, time(nullptr), false };
    writeTransactionToFile(income2, filename);

    vector<Transaction> transactions = readAllTransactionsFromFile(filename);

    double balance = calculateBalance(transactions);
    cout << "Balance: " << fixed << setprecision(2) << balance << endl;

    calculateCategoryExpensePercentageLastMonth(transactions);

    calculateCategoryExpensePercentageAllTime(transactions);

    return 0;
}
