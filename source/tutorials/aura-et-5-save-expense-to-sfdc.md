title: Expense Tracker App -  Save Expense To SFDC
---
When you enter text into the form and press Submit, you want to create a new expense record and save it in the model. This
action is wired up to the button component via the press attribute.

## Create ExpenseController.apxc 

First, create an Apex controller that saves or updates the records and then add the logic to update the counters dynamically.

1. Click **File > New > Apex Class**.
2. Enter `ExpenseController` in the **New Class** window. This creates a new Apex class, `ExpenseController.apxc`.
3. In `ExpenseController.apxc`, enter this code.
*Replace namespace with the name of your registered namespace.*

``` java
public class ExpenseController {
    @AuraEnabled
    public static Expense saveExpense(String id, String expname, Decimal amount, String
    client, DateTime expdate, Boolean reimbursed) {
        namespace__Expense__c expenseSobject = new namespace__Expense__c(id=id);
        expenseSobject.Name = expname;
        expenseSobject.namespace__Amount__c = amount;
        expenseSobject.namespace__Client__c = client;
        expenseSobject.namespace__Date__c = expdate;
        expenseSobject.namespace__Reimbursed__c = reimbursed;
        insertOrUpdate(expenseSobject);
        return new Expense(expenseSobject);
    }
    /*Create new or update the Expense record*/
    private static void insertOrUpdate(namespace__Expense__c expenseSobject) {
        if (expenseSobject.id != null) {
            update(expenseSobject);
        }
        else {
            insert(expenseSobject);
        }
    }
}
```
The custom controller enables you to insert or update an expense record.

## Update form.cmp

In **form.cmp**, add the controller attribute to the aura:component tag.
*Replace namespace with the name of your registered namespace.*
`<aura:component model="namespace.ExpenseModel" controller="namespace.ExpenseController">`


## Create newExpense JS Controller

Create the controller-side actions to create a new expense record when the Submit button is pressed. In formController.js, add this code **after the doInit** action.

``` javascript
newExpense: function(component, event, helper) {
	var expname1 = component.find("expname").get("v.value");
	var amount1 = component.find("amount").get("v.value");
	var client1 = component.find("client").get("v.value");
	var expdate1 = component.find("expdate").get("v.value");
	var reimbursed1 = component.find("reimbursed").get("v.value");
	//Validation for Amount field
	var amtSimpleVal = component.find("amount").getValue("v.value");
	var amtVal = amtSimpleVal.getValue("v.value");
	if (isNaN(amtVal)) {
		amtSimpleVal.setValid(false);
		amtSimpleVal.addErrors([{
			message: "Enter an amount in the format 0 or 0.00."
		}]);
	} else {
		if (!amtSimpleVal.isValid()) {
			amtSimpleVal.setValid(true);
		}
		helper.createExpense(component, {
			expname: expname1,
			amount: amount1,
			client: client1,
			expdate: expdate1,
			reimbursed: reimbursed1
		});
	}
}
```
Note: Add a coma "," after `doInit` if it's missing.

### Code Highlights

1. `newExpense` validates the amount field using the default error handling, which appends the error message to the field.
2. Notice that you’re passing in the arguments to a **helper** function `createExpense`, which then triggers the Apex class `saveExpense`.
3. A helper function is a resource for storing code that you want to reuse in your component bundle.

## Create createExpense Helper

Create the helper function to handle the record creation and dynamically update the counters. 

1. Click **HELPER** to create a new resource, `formHelper.js` 
2. Enter this code.

``` javascript
({
	createExpense: function(component, expense) {
		var expenses = component.getValue("m.expenses");
		var action = component.get("c.saveExpense");
		action.setParams(expense);
		action.setCallback(this, function(a) {
			expenses.push(a.getReturnValue());
			var allExpenses = component.getAttributes().getRawValue("allExpenses");
			allExpenses.setValue(expenses);
			this.updateTotal(component);
		});
		$A.enqueueAction(action);
	},
	updateTotal : function(component) {
		var expenses = component.getValue("m.expenses");
		var total = 0;
		var exp = 0;
		expenses.each(function(e, i) {
			total += e.unwrap().amount;
		});
		exp = expenses.getLength();
		component.setValue("v.total", total);
		component.setValue("v.exp", exp);
	}
})
```

### Code Highlights

1. `component.get("c.saveExpense");`
	**createExpense** defines an instance of the **saveExpense** *server-side action* and sets the **expense** object as a parameter.
2. `action.setCallback(this, function(a) {`
	The callback is executed after the server-side action returns, which updates the model, view, and counters.
	
3. `$A.enqueueAction(action)`
	 Adds the server-side action to the queue of actions to be executed.
	 
In **updateTotal**, you are retrieving the actual data in the value object, `e`, using `e.unwrap().amount`.

## Update doInit Controller 
In formController.js, update the `doInit` function to update the counters on initialization.

``` javascript
doInit: function(component, event, helper) {
	var expenses = component.getValue("m.expenses");
	var allExpenses = component.getAttributes().getRawValue("allExpenses");
	allExpenses.setValue(expenses);
	helper.updateTotal(component);  // <--- Add this line
}
```

## Test The App
1. Save your changes and reload your browser. 
2. Test your form by entering Breakfast, 10, ABC Co., Breakfast, Apr 30, 2014 9:00:00 AM. 
3. Click the Submit button. Notice that the record is added to both your view and model, and the counters are updated.