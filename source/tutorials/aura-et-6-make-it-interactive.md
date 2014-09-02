title: Expense Tracker App -  Make It Interactive
---
Events add an interactive layer to your app by enabling you to share data between components. When the checkbox is checked or unchecked in the expense list view, you want to fire an event that updates both the view and model based on the relevant
data.
Start by creating the event and its handler before firing it and handling the event in the parent component.

# Create Event

Create `updateExpenseItem.evt`

1. Click **File > New > Aura Event**.
2. Enter `updateExpenseItem` in the **New Event** window. This creates a new event, `updateExpenseItem.evt`.
3. In `updateExpenseItem.evt`, enter this code.
	``` html
	<aura:event type="APPLICATION">
		<aura:attribute name="expense" type="Expense__c" />
	</aura:event>
	```
## Code Highlights

1. The attribute `expense` or type `Expense__c` you’re defining in the event is passed from the firing component to the handlers.
2. Aura provides *component* events and application events. An application event is used here, which when fired notifies its handlers. In this case, form.cmp is notified and handles the event.
<br/>

# Send The Event

Make `expenseList.cmp` send the event. Recall that **expenseList.cmp** contains the checkbox that’s wired up to a client-side controller action, denoted by **change="{!c.update}"**. You’ll set up the update action next.

## Add "update" JS Action

1. In the **expenseList** sidebar, click **CONTROLLER**. This creates a new resource, `expenseListController.js`.
2. Enter this code.
*Replace namespace with the name of your registered namespace*
	
``` JavaScript

({
	update: function(component, evt, helper) {
		var target = evt.getSource ? evt.getSource().getElement() : evt.target;
		var expense = component.get("v.expense");
		expense.reimbursed = target.checked;
		var updateEvent = $A.get("e.namespace:updateExpenseItem");
		updateEvent.setParams(expense).fire();
	}
})
```

### Code Highlights

1. When the checkbox is checked or unchecked, the **update** action runs, setting the `expense.reimbursed` parameter value to true or false, based on `target.checked`. 

2. `var updateEvent = $A.get("e.namespace:updateExpenseItem");`
 The **updateExpenseItem.evt** event is fired after the `expense.reimbursed` parameter
values are set on the object.

# Receive The Event

Update `form.cmp` to listen to the event.

## Add Handler

In `form.cmp`, add this handler code before the`<aura:attribute>` tags:
`<aura:handler event="namespace:updateExpenseItem" action="{!c.updateEvent}" />`

This event handler runs the updateEvent action when the application event you created is fired.

## Add Controller

Wire up the `updateEvent` action to handle the fired event. Click on the CONTROLLER, to open **formController.js**, enter this code.

``` javascript
	updateEvent : function(component, event, helper) {
		helper.updateExpense(component, event.getParams());
	}
```
This action calls a helper function and passes in event.getParams(), which contains the expense object with its parameters and values: `{amount: 10, expname: "Breakfast", id: "a03D0000003eRgtIAE”, reimbursed:
false}`.

## Create Helper

Create the **updateExpense** helper function in **formHelper.js** with this code.

``` javascript
updateExpense: function(component, expense) {
	var expenses = component.getValue("m.expenses");
	expenses.each(function(e, i) {
		if (e.getValue("id").getValue() === expense.id) {
			e.getValue("reimbursed").setValue(expense.reimbursed);
		}
	});
	var action = component.get("c.saveExpense");
	action.setParams(expense);
	$A.enqueueAction(action);
}
```


Note: `e.getValue("id")` returns the value object, which is a thin wrapper around the actual data. `e.getValue("id").getValue()` returns the actual data in the value object.

<br>

# Load on init

We need to make sure expenses are updated when the `form.cmp` controller is loaded and not just when the event is triggered.

Go back to **form.cmp**'s JS controller *formController.js* and add `helper.updateExpense(component);` to **doInit** function. 
```
doInit: function(component, event, helper) {
	var expenses = component.getValue("m.expenses");
	var allExpenses = component.getAttributes().getRawValue("allExpenses");
	allExpenses.setValue(expenses);
	helper.updateTotal(component);
	helper.updateExpense(component);  // <-- Add this line
}

```

# Test The App

The Aura app you just created is currently accessible as a standalone app by accessing
`http://<mySalesforceInstance>/<namespace>/expenseTracker.app`
