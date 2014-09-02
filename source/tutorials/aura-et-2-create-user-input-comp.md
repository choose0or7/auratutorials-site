title: Expense Tracker App - Create User Input Component
---
## Create Component

Components are the building blocks of an app. They can be wired up to a model to load your data. The component you create in this step provides a form that takes in user input about an expense, such as expense amount and date.

1. Click **File > New > Aura Component**.
2. Enter `form` for the Name field in the **New Aura Bundle** popup window. This creates a new Aura component, `form.cmp`.
3. In the source code editor, enter this code.

	``` html	
	<aura:component>
	<!-- Attributes for Expense Counters -->
	<aura:attribute name="total" type="Double" default="0.00" />
	<aura:attribute name="exp" type="Double" default="0" />
	<!-- Input Form using Aura Components -->
	<form>
		<fieldset>
			<ui:inputText aura:id="expname" label="Expense Name" class="form-control"
			placeholder="My Expense" required="true"/>
			<ui:inputNumber aura:id="amount" label="Amount" class="form-control"
			placeholder="0" required="true"/>
			<ui:inputText aura:id="client" label="Client" class="form-control"
			placeholder="ABC Co."/>
			<ui:inputDateTime aura:id="expdate" label="Expense Date" class="form-control"
			displayDatePicker="true"/>
			<ui:inputCheckbox aura:id="reimbursed" label="Reimbursed?"/>
			<ui:button class="btn" label="Submit" press="{!c.newExpense}"/>
		</fieldset>
	</form>
	<!-- Expense Counters -->
	<div class="row">
		<!-- Change the counter color to red if total amount is more than 100 -->
		<div class="{!v.total >= 100 ? 'alert alert-danger' : 'alert alert-success'}">
			<h3>Total Expenses</h3>$<ui:outputNumber value="{!v.total}" format=".00"/>
		</div>
		<div class="alert alert-success">
			<h3>No. of Expenses</h3><ui:outputNumber value="{!v.exp}"/>
		</div>
	</div>
	<!-- Display Expense Data from Model -->
	<div class="row">
		<aura:iteration items="{!m.expenses}" var="expense">
		<p>{!expense.expname}, {!expense.client}, {!expense.amount}, {!expense.expdate},
			{!expense.reimbursed}</p>
		</aura:iteration>
	</div>
	</aura:component>
	
	```

### Code Highlights	

1. `<ui:button class="btn" label="Submit" press="{!c.newExpense}"/>`
Aura components provide a rich set of attributes and browser event support. For example, the `press` event in `ui:button` enables you to wire up an action (`c.newExpense`) when the button is pressed.
2. The **c.**  in `{!c.newExpense}` represents the client-side JavScript controller action that runs when the Submit button is clicked, which creates a new expense.
3. The attributes and expressions here will become clearer as you build the app. For example, `{!v.exp}` evaluates the number of expenses in the model and `{!v.total}` evaluates the total amount. 

## Add Styles

Click **STYLE** in the sidebar to create a new resource named form.css. Enter these CSS rule sets.

``` css
.THIS .uiInput {
	margin: 10px;
}
.THIS form {
	width: 100%;
}
.THIS .form-control {
	display: block;
	width: 90%;
	padding: 10px 15px;
	font-size: 15px;
	line-height: 1.4;
	color: #2c3e50;
	background-color: #ffffff;
	background-image: none;
	border: 1px solid #dce4ec;
	border-radius: 4px;
}
.THIS .uiCheckbox {
	margin-left: 20px;
}
.THIS .btn, .THIS .btn:active {
	display: inline-block;
	margin: 0px 10px 20px 1%;
	font-weight: normal;
	text-align: center;
	vertical-align: middle;
	cursor: pointer;
	background-image: none;
	border: 1px solid #bbc0c4;
	border-radius: 5px;
	background: #ffffff;
	box-shadow: none;
	padding: 10px 15px;
	font-size: 15px;
	line-height: 1.4;
	border-radius: 4px;
	text-shadow: none;
}
.THIS .row {
	width: 100%;
	margin-bottom: 20px;
}
.THIS .alert {
	padding: 15px;
	margin: 20px;
	border: 1px solid transparent;
	border-radius: 4px;
	width: 45%;
	line-height: 18px;
	text-align: center;
}
.THIS .alert-danger {
	background-color: #e74c3c;
	border-color: #e74c3c;
}
.THIS .alert-success {
	background-color: #18bc9c;
	border-color: #18bc9c;
	color: #ffffff;
}
.THIS .blue {
	background: #7BBCE8;
}
.THIS .white {
	background: #ffffff;
}
.THIS .uiInput.uiInputDateTime {
	position: relative;
}
.THIS .uiInputDateTime+.datePicker-openIcon {
	display: inline-block;
	left: 90%;
	top: 25px;
	background-position: center;
}

```

## Add To expenseTracker.app

Add the component to the app. In expenseTracker.app, add the new component to the markup.
<b>Replace namespace with the name of your registered namespace</b>.

``` html
<aura:application>
<div class="container">
	<h1>Add Expense</h1>
	<namespace:form />
</div>
</aura:application>
```

## Test The App

Save your changes and click **Update Preview** in the sidebar to preview your app. Alternatively, reload your browser.

**Note:** In this step, the component you created doesn’t display any data since you haven’t created the model yet.
You created a component that provides an input form and view of your expenses. Next, you’ll set up the model to load expense data in the component, which uses `<aura:iteration>` to iterate through a list of items provided by the model using the expression `{!m.expenses}`. 