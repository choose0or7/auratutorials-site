title: Expense Tracker App - Create The App
---
## Create expenseTracker.app

Create a static mockup in a .app file, which is the entry point for your app. It can contain other components and HTML markup.

1. Click Your ***Name > Developer Console*** to open the Developer Console.
2. In the Developer Console, click ***File > New > Aura*** Application.
3. Enter `expenseTracker` for the *Name* field in the ***New Aura Bundle*** popup window. This creates a new Aura app, `expenseTracker.app`.
4. In the source code editor, enter this code.
	
	``` html
	<aura:application>
	<div class="container">
	   <h1>Add Expense</h1>
	</div>
	</aura:application>
	```
5. Add styles to your new component.
	a. In the sidebar, click **STYLE**. This creates a new CSS resource, `expenseTracker.css`.
	b. Enter these CSS rule sets.

	``` css
	.THIS {
		width: 100%;
		margin: 2%;
	}
	.THIS h1 {
		font-size: 20px;
		line-height: 1.8;
		background: url(/img/icon/creditCard32.png) no-repeat;
		padding-left: 40px;
	}
	```

{% note info THIS %}
Note: **THIS** is a keyword that adds namespacing to CSS to prevent any conflicts with another component’s styling.
{% endnote %}

## Test the app

Save your changes and click **Preview** in the sidebar to preview your app. Alternatively, go to `http://<mySalesforceInstance>/<namespace>/expenseTracker.app` in your browser. You should see a header and icon.
