title: Expense Tracker App -  Create Nested Components
---
As your component grows, you want to break it down to maintain granularity and encapsulation. This step walks you through creating a component with repeating data and whose attributes are passed to its parent component. You’ll also add a client-side controller action to load your data on component initialization.

## Create expenseList.cmp

Create a sub-component expenseList that shows list of expenses. 

1. Click File > New > Aura Component.
2. Enter expenseList in the New Aura Bundle window. This creates a new Aura component, expenseList.cmp.
3. In expenseList.cmp, enter this code.
	
	``` html
	<aura:component>
		<aura:attribute name="expense" type="Expense__c"/>
		<!-- Color the item blue if the expense is reimbursed -->
		<div class="{!v.expense.reimbursed == true
		? 'listRecord recordLayout blue' : 'listRecord recordLayout white'}">
		<a aura:id="expense" href="{!'/' + v.expense.id}">
			<div class="itemTitle">{!v.expense.expname}</div>
			<div class="recordItem">Amount:
				<ui:outputNumber value="{!v.expense.amount}" format=".00"/>
			</div>
			<div class="recordItem">Client:
				<ui:outputText value="{!v.expense.client}"/>
			</div>
			<div class="recordItem">Date:
				<ui:outputDateTime value="{!v.expense.expdate}" />
			</div>
			<div class="recordItem">Reimbursed?
				<ui:inputCheckbox value="{!v.expense.reimbursed}" change="{!c.update}"/>
			</div>
		</a>
	</div>
	</aura:component>
	```

### Code Highlights

1. Instead of using `{!expense.amount}`, you’re now using `{!v.expense.amount}`. This expression accesses the **expense** attribute and the amount value on it. The expense attribute is of type `Expense__c`. 
2. For non-primitive types, use the format `myNamespace.myApexClass`.
3. Additionally, `href="{!'/' + v.expense.id}"` uses the expense id to set the link to the detail page of each expense record.

### Add Styles

Click **STYLE** in the sidebar and enter these CSS rule sets.

```
.THIS.recordLayout {
	list-style: none;
	padding: 14px;
	margin: 10px;
	border-bottom:1px solid #cfd4d9;
}
.THIS.listRecord {
	margin: 14px;
	border-radius: 5px;
	border: 1px solid #cfd4d9;
	position: relative;
}
.THIS .uiOutputText {
	line-height: 1.1em;
}
.THIS .itemTitle {
	font-size: 15px;
	padding-bottom: 3px;
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
}
.THIS .recordItem {
	text-overflow: ellipsis;
	white-space: nowrap;
}
```

### Update form.cmp

In **form.cmp**, 

1. Add a **List** attribute that stores **allExpenses**.
`<aura:attribute name="allExpenses" type="List" />`
2. Update the **aura:iteration** tag to use `{!v.allExpenses}` instead of ~~`{!m.expenses}`~~.

Finally it should look like this

``` html
<aura:component model="namespace.ExpenseModel">
<aura:attribute name="allExpenses" type="List" />  <!-- Add This Line-->

<!-- Other aura:attribute tags here -->
<!-- Other code here -->

<div class="row">
<aura:iteration items="{!v.allExpenses}" var="expense">  <!-- Update This Line-->
	<namespace:expenseList expense="{!expense}"/>
</aura:iteration>
</div>
</aura:component>
```

You’re now passing the whole expense object, represented by `namespace:expenseList`, into the parent component using an attribute. The `aura:attribute` tag represents a list of the expenses on an instance of the component. Next, create the event handler to load your model on component initialization.

<br>
<br>
## Load Data Upon Initialization.

Aura automatically fires `init` event when the component loads. We will add a event handler to listen to `init` event and make call to SFDC and finally populate `allExpenses` in the view.

### Create Event Handler

In **form.cmp**, add an **init** handler immediately after the opening *aura:component* tag.

```
<aura:component model="namespace.ExpenseModel">
	<aura:handler name="init" value="{!this}" action="{!c.doInit}" />  <!-- Add This Line -->
	<!-- Other aura:attribute tags here -->
	<!-- Other code here -->
</aura:component>
```

On initialization, this event handler runs the doInit action that you’re creating next

### Create A Controller 

Add the client-side controller action for this handler. In the sidebar, click **CONTROLLER** to create a new resource, `formController.js`. Enter this code.

``` javascript
({
	doInit: function(component, event, helper) {
		var expenses = component.getValue("m.expenses");
		var allExpenses = component.getAttributes().getRawValue("allExpenses");
		allExpenses.setValue(expenses);
	}
})
```

Save your changes and reload your browser. Your expense records are now displayed.

## Test The App

Congratulations! Your app now loads and displays records from the model. Next, add client-side logic to process user input and save them in the model.
