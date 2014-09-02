title: Expense Tracker App -  Load the Expense Data
---
##  Load the Expense Data

Models load initial data and are created in an Apex class. 

### Expense Class

Create an Expense object and define a list variable. This list is loaded in your app using a getter method.

1. Click **File > New > Apex Class**.
2. Enter *Expense* in the **New Class** window. This creates a new Apex class, `Expense.apxc`.
3. Enter this code to create an Expense object.
*Replace namespace with the name of your registered namespace.*

	``` java
	public class Expense {
	    @AuraEnabled
	    public ID id {get; set;}
	    @AuraEnabled
	    public String expname {get;set;}
	    @AuraEnabled
	    public Decimal amount {get;set;}
	    @AuraEnabled
	    public String client {get;set;}
	    @AuraEnabled
	    public DateTime expdate {get;set;}
	    @AuraEnabled
	    public Boolean reimbursed {get;set;}
	    public Expense(namespace__Expense__c expenseSobject) {
	        this.id = expenseSobject.id;
	        this.expname = expenseSobject.Name;
	        this.amount = expenseSobject.namespace__Amount__c;
	        this.client = expenseSobject.namespace__Client__c;
	        this.expdate = expenseSobject.namespace__Date__c;
	        this.reimbursed = expenseSobject.namespace__Reimbursed__c;
	    }
	}
	```

The Expense object contains variables that correspond to the custom fields you created. `get` and `set` are Apex properties that make the variables readable and writeable. **@AuraEnabled** exposes those properties to your Aura app.

### ExpenseModel Class	

1. Create the `expense` model. Click **File > New > Apex Class ** and enter *ExpenseModel* in the **New Class** window. This creates a new Apex class, `ExpenseModel.apxc`.
2. Enter this code.
	
```	java
public class ExpenseModel {
    private List&lt;Expense&gt; expenses;
    public ExpenseModel() {
        loadExpenses();
    }
    private void loadExpenses() {
        String soql = 'select id, name, namespace__Amount__c, namespace__Client__c,' +
        'namespace__Date__c, namespace__Reimbursed__c,' +
        'CreatedDate FROM namespace__Expense__c ' +
        'ORDER BY CreatedDate ASC';
        List&lt;namespace__Expense__c&gt; expenseSobjects = Database.query(soql);
        expenses = new List&lt;Expense&gt;();
        for (namespace__Expense__c expenseSobject : expenseSobjects) {
            expenses.add(new Expense(expenseSobject));
        }
    }
    @AuraEnabled
    public List&lt;Expense&gt; getExpenses() {
        loadExpenses();
        return expenses;
    }
}

```

The **loadExpenses()** method runs a SOQL query to return all expense records. The for loop stores your expense records in a *expenseSobjects* variable. **getExpenses()** returns the list of records to the component using the `m.expenses` syntax.

### Update form.cmp

Go back to **form.cmp** component. Then update the aura:component tag to include the model attribute after the `<component>` tag (i.e. 2nd line). `<aura:component model="namespace.ExpenseModel">`

## Test The App

Save your changes and reload your browser. You should see the expense records. The counters aren’t working at this point as you’ll be adding the programmatic logic later.

## Summary

In this step, you created a model to load initial expense data. The model contains a SOQL query. **getExpenses()** returns the list of expense records. By default, Aura doesn’t call any getters. To access a method, annotate the method with `@AuraEnabled`, which exposes only the data in that method. Models are instantiated when the component is first requested.