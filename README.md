import React, { useState } from "react";
import "./ExpenseTracker.css"; // Add some basic styling

const ExpenseTracker = () => {
  const [salary, setSalary] = useState(100000); // Initial total salary
  const [income, setIncome] = useState(0);
  const [expense, setExpense] = useState(0);
  const [transactions, setTransactions] = useState([]);
  const [showInput, setShowInput] = useState(false);
  const [amount, setAmount] = useState("");
  const [description, setDescription] = useState("");

  const handleAddTransaction = () => {
    const numericAmount = parseFloat(amount);
    if (!numericAmount || description.trim() === "") {
      alert("Please enter a valid amount and description.");
      return;
    }

    setTransactions((prev) => [
      { id: Date.now(), amount: numericAmount, description },
      ...prev,
    ]);

    if (numericAmount > 0) {
      setIncome((prev) => prev + numericAmount);
    } else {
      setExpense((prev) => prev + Math.abs(numericAmount));
    }

    setSalary((prev) => prev + numericAmount);

    // Clear input fields
    setAmount("");
    setDescription("");
    setShowInput(false);
  };

  return (
    <div className="expense-tracker">
      <header>
        <h1>Total Salary: {salary.toFixed(2)}</h1>
        <button className="add-btn" onClick={() => setShowInput(!showInput)}>
          {showInput ? "Cancel" : "Add"}
        </button>
      </header>

      {showInput && (
        <div className="input-container">
          <input
            type="number"
            placeholder="Amount"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
          />
          <input
            type="text"
            placeholder="Description"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
          />
          <button onClick={handleAddTransaction}>Add Transaction</button>
        </div>
      )}

      <div className="summary">
        <div>
          <h2>Income</h2>
          <p>{income.toFixed(2)}</p>
        </div>
        <div>
          <h2>Expense</h2>
          <p>{expense.toFixed(2)}</p>
        </div>
      </div>

      <ul className="transactions">
        {transactions.map((transaction) => (
          <li key={transaction.id} className={transaction.amount > 0 ? "income" : "expense"}>
            {transaction.description}: {transaction.amount.toFixed(2)}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default ExpenseTracker;

/* ExpenseTracker.css */
.expense-tracker {
  font-family: Arial, sans-serif;
  max-width: 500px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.add-btn {
  padding: 8px 16px;
  border: none;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
  border-radius: 4px;
}

.input-container {
  display: flex;
  gap: 8px;
  margin: 16px 0;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  flex: 1;
}

button {
  padding: 8px 16px;
  border: none;
  background-color: #28a745;
  color: #fff;
  cursor: pointer;
  border-radius: 4px;
}

.summary {
  display: flex;
  justify-content: space-between;
  margin: 16px 0;
}

.transactions {
  list-style-type: none;
  padding: 0;
}

.transactions li {
  padding: 8px;
  border-bottom: 1px solid #ddd;
}

.transactions li.income {
  color: green;
}

.transactions li.expense {
  color: red;
}

  
