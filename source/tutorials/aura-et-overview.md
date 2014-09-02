title: Expense Tracker App - Overview
---
## Overview

In this example you will build an Expense Tracker app that allows you to add expenses, see list of expenses, mark expenses as reimbursed etc.

The goal of the app is to take advantage of many of the components Aura provides out-of-the-box, and to demonstrate the client and server interactions using JavaScript and Apex. As you build the app, you’ll learn how to use Aura expressions to interact with data dynamically and use events to communicate data between components.

<img src="/auratutorials/images/aura-et-app-final.png" />

1. Form contains Aura input components that updates the view and model when the Submit button is pressed
2. Counters are initialized with total amount of expenses and number of expenses, and updated on record creation or deletion
3. Display of expense list use Aura output components and are updated as more expenses are added
4. User interaction on the display of expense list triggers an update event that saves the record changes