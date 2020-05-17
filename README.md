<!-- prettier-ignore -->
563aa3f05cd9dc2eadd66c02c5d3dff7b4719f24
branch without MERN stack ```react-exercise-no-backend```

## Step 1

Build out the ui using this html file https://github.com/bradtraversy/vanillawebprojects/blob/master/expense-tracker/index.html

Add Balance component. IncomeExpenses component. TransactionList. AddTransaction

Remove ids from jsx, change class to className and for to htmlFor.

## Step 2

in AddTransactions add hooks for both inputs and they should change when the input changes.

Need to add global state.

1. Make a context file and import React, { createContext, useReducer } into a file called GlobalState
2. make an inital state called initialState with following transactions

```
[
   { id: 1, text: 'Flower', amount: -20 },
   { id: 2, text: 'Salary', amount: 300 },
   { id: 3, text: 'Book', amount: -10 },
   { id: 4, text: 'Camera', amount: 150 }
];
```

3. Create context with value of intial state and call it GlobalContext
4. In same file export GlobalProvider. It does a useReducer and return the state
5. Make a AppReducer file

## Step 3

1. Import GlobalContext into the Transaction List file. Need to import useContext
2. Save the global context into a variable called context. This can be changed to destrcture just the transactions.
3. Get the transaction value and loop through it and display the <li>
4. Make a new file called Transaction.js. Move the li list above and place in this file. Need to pass down props.
5. Need to determine what sign to be used based on positive or negative number for the dollar amount. Use a ternary and a variable.
6. Need to handle the border color. Use a ternary

## Step 4

1. Import Globalcontext and transaction state into Balance.js.
2. Get each amount of transaction using a map function and then a reduce function to get the total.
3. Import Globalcontext and transaction state into IncomeExpenses.js
4. Get the total of income and expenses. Use a filter/reduce function.

## Step 5

1. Dipatch a DELETE_TRANSACTION action from the GlobalState to the App Reducer. It's a function with id as param. Pass the function to the GlobalContext.Provider so that context has access to it.
2. In app Reducer add the action type and filter by the id
3. Import Global Context etc. to Transaction.js. Get the deleteTransaction from the GlobalContext. Use function to delete transaction from the button
4. Add Transaction:

- Create an action in global state. Complete the steps as with deleteTransaction
- Add it to App Reducer. Use a spread operator
- Import it to AddTransaction.js. Add an onSubmit on form. Make it into its own function. Send a new transaction (Need to generate a randomId). Set component state to blank.

5. Remove initial state from Global Context once this is all done. Not PR'd
