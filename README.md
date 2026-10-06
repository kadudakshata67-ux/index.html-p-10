<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Student Expense Tracker</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 min-h-screen text-gray-800">

  <!-- Header -->
  <header class="bg-blue-600 text-white">
    <div class="max-w-6xl mx-auto px-4 py-6">
      <h1 class="text-3xl font-bold">
        Student Expense Tracker
      </h1>

      <p class="mt-1 text-blue-100">
        Track your daily expenses and manage your budget.
      </p>
    </div>
  </header>


  <!-- Main Content -->
  <main class="max-w-6xl mx-auto px-4 py-8">

    <!-- Summary Cards -->
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-5 mb-8">

      <!-- Total Expenses -->
      <div class="bg-white rounded-xl shadow p-5">
        <p class="text-gray-500 text-sm">
          Total Expenses
        </p>

        <h2 id="totalExpense"
            class="text-3xl font-bold text-blue-600 mt-2">
          ₹0
        </h2>
      </div>


      <!-- Number of Expenses -->
      <div class="bg-white rounded-xl shadow p-5">
        <p class="text-gray-500 text-sm">
          Number of Expenses
        </p>

        <h2 id="expenseCount"
            class="text-3xl font-bold text-green-600 mt-2">
          0
        </h2>
      </div>


      <!-- Average Expense -->
      <div class="bg-white rounded-xl shadow p-5">
        <p class="text-gray-500 text-sm">
          Average Expense
        </p>

        <h2 id="averageExpense"
            class="text-3xl font-bold text-purple-600 mt-2">
          ₹0
        </h2>
      </div>

    </div>


    <!-- Add Expense Form -->
    <div class="bg-white rounded-xl shadow p-6 mb-8">

      <h2 class="text-xl font-bold mb-5">
        Add New Expense
      </h2>

      <form id="expenseForm">

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">

          <!-- Expense Name -->
          <div>
            <label
              for="expenseName"
              class="block text-sm font-semibold mb-2"
            >
              Expense Name
            </label>

            <input
              type="text"
              id="expenseName"
              placeholder="e.g. Lunch"
              required
              class="w-full px-4 py-3 border border-gray-300
                     rounded-lg focus:outline-none
                     focus:ring-2 focus:ring-blue-500"
            >
          </div>


          <!-- Amount -->
          <div>
            <label
              for="expenseAmount"
              class="block text-sm font-semibold mb-2"
            >
              Amount
            </label>

            <input
              type="number"
              id="expenseAmount"
              placeholder="e.g. 150"
              min="1"
              required
              class="w-full px-4 py-3 border border-gray-300
                     rounded-lg focus:outline-none
                     focus:ring-2 focus:ring-blue-500"
            >
          </div>


          <!-- Category -->
          <div>
            <label
              for="expenseCategory"
              class="block text-sm font-semibold mb-2"
            >
              Category
            </label>

            <select
              id="expenseCategory"
              required
              class="w-full px-4 py-3 border border-gray-300
                     rounded-lg focus:outline-none
                     focus:ring-2 focus:ring-blue-500"
            >
              <option value="">Select Category</option>
              <option value="Food">Food</option>
              <option value="Travel">Travel</option>
              <option value="Education">Education</option>
              <option value="Shopping">Shopping</option>
              <option value="Other">Other</option>
            </select>
          </div>


          <!-- Date -->
          <div>
            <label
              for="expenseDate"
              class="block text-sm font-semibold mb-2"
            >
              Date
            </label>

            <input
              type="date"
              id="expenseDate"
              required
              class="w-full px-4 py-3 border border-gray-300
                     rounded-lg focus:outline-none
                     focus:ring-2 focus:ring-blue-500"
            >
          </div>

        </div>


        <!-- Add Button -->
        <button
          type="submit"
          class="mt-5 bg-blue-600 text-white px-6 py-3
                 rounded-lg font-semibold hover:bg-blue-700
                 transition w-full sm:w-auto"
        >
          + Add Expense
        </button>

      </form>
    </div>


    <!-- Expense List -->
    <div class="bg-white rounded-xl shadow p-6">

      <div class="flex flex-col sm:flex-row
                  sm:items-center sm:justify-between
                  gap-3 mb-5">

        <h2 class="text-xl font-bold">
          Recent Expenses
        </h2>

        <!-- Category Filter -->
        <select
          id="filterCategory"
          class="px-4 py-2 border border-gray-300
                 rounded-lg focus:outline-none
                 focus:ring-2 focus:ring-blue-500"
        >
          <option value="All">All Categories</option>
          <option value="Food">Food</option>
          <option value="Travel">Travel</option>
          <option value="Education">Education</option>
          <option value="Shopping">Shopping</option>
          <option value="Other">Other</option>
        </select>

      </div>


      <!-- Expenses -->
      <div id="expenseList" class="space-y-3">
        <!-- JavaScript adds expenses here -->
      </div>


      <!-- Empty Message -->
      <p
        id="emptyMessage"
        class="text-center text-gray-400 py-8"
      >
        No expenses added yet.
      </p>

    </div>

  </main>


  <!-- JavaScript -->
  <script>

    // Store expenses
    let expenses = [];


    // Get elements
    const expenseForm =
      document.getElementById("expenseForm");

    const expenseList =
      document.getElementById("expenseList");

    const emptyMessage =
      document.getElementById("emptyMessage");

    const filterCategory =
      document.getElementById("filterCategory");

    const totalExpense =
      document.getElementById("totalExpense");

    const expenseCount =
      document.getElementById("expenseCount");

    const averageExpense =
      document.getElementById("averageExpense");


    // Set today's date
    document.getElementById("expenseDate").value =
      new Date().toISOString().split("T")[0];


    // Add expense
    expenseForm.addEventListener("submit", function(event) {

      event.preventDefault();


      // Get values
      const name =
        document.getElementById("expenseName").value.trim();

      const amount =
        Number(document.getElementById("expenseAmount").value);

      const category =
        document.getElementById("expenseCategory").value;

      const date =
        document.getElementById("expenseDate").value;


      // Validate
      if (!name || amount <= 0 || !category || !date) {

        alert("Please enter valid expense details.");

        return;
      }


      // Create expense object
      const expense = {
        id: Date.now(),
        name: name,
        amount: amount,
        category: category,
        date: date
      };


      // Add to array
      expenses.push(expense);


      // Reset form
      expenseForm.reset();

      document.getElementById("expenseDate").value =
        new Date().toISOString().split("T")[0];


      // Update UI
      displayExpenses();

    });


    // Display expenses
    function displayExpenses() {

      expenseList.innerHTML = "";


      // Get selected category
      const selectedCategory =
        filterCategory.value;


      // Filter expenses
      const filteredExpenses =
        selectedCategory === "All"
          ? expenses
          : expenses.filter(
              expense =>
                expense.category === selectedCategory
            );


      // Show empty message
      if (filteredExpenses.length === 0) {

        emptyMessage.classList.remove("hidden");

      } else {

        emptyMessage.classList.add("hidden");
      }


      // Create expense cards
      filteredExpenses.forEach(function(expense) {

        const expenseCard =
          document.createElement("div");


        expenseCard.className =
          "flex flex-col md:flex-row " +
          "md:items-center md:justify-between " +
          "gap-4 border border-gray-200 " +
          "rounded-lg p-4 hover:bg-gray-50";


        expenseCard.innerHTML = `

          <div class="flex-1">

            <h3 class="font-bold text-lg">
              ${expense.name}
            </h3>

            <div class="flex flex-wrap gap-2 mt-2">

              <span class="text-sm bg-blue-100
                           text-blue-700 px-3 py-1
                           rounded-full">
                ${expense.category}
              </span>

              <span class="text-sm text-gray-500">
                ${expense.date}
              </span>

            </div>

          </div>


          <div class="flex items-center
                      justify-between md:justify-end
                      gap-4">

            <span class="font-bold text-lg text-green-600">
              ₹${expense.amount.toFixed(2)}
            </span>

            <button
              onclick="deleteExpense(${expense.id})"
              class="bg-red-500 text-white px-4 py-2
                     rounded-lg text-sm font-semibold
                     hover:bg-red-600 transition"
            >
              Delete
            </button>

          </div>

        `;


        expenseList.appendChild(expenseCard);

      });


      // Update summary
      updateSummary();

    }


    // Delete expense
    function deleteExpense(id) {

      expenses =
        expenses.filter(
          expense => expense.id !== id
        );


      displayExpenses();

    }


    // Update summary cards
    function updateSummary() {

      const total =
        expenses.reduce(
          (sum, expense) =>
            sum + expense.amount,
          0
        );


      const count = expenses.length;


      const average =
        count > 0
          ? total / count
          : 0;


      totalExpense.textContent =
        "₹" + total.toFixed(2);


      expenseCount.textContent =
        count;


      averageExpense.textContent =
        "₹" + average.toFixed(2);

    }


    // Filter expenses
    filterCategory.addEventListener(
      "change",
      displayExpenses
    );


    // Initial display
    displayExpenses();

  </script>

</body>
</html>
