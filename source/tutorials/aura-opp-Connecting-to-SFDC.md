title: Opportunity Select App - Connecting to SFDC
---
## Code Reuse

One of the nice things about Aura is that you can reuse most of your Apex and Visualforce skills while build Aura apps. This Apex Model is apated from a Visualforce example. Let's create an Apex Model that returns list of Opportunities from SFDC.

1. Select File > New > Apex Class
2. Enter OpportunityListModel for the class name
3. Click OK to create the class

<img src="/auratutorials/images/aura-opp-create-opp-list-model.png"/>

## @auraEnabled annotation

Edit the model and add the following code. `@AuraEnabled` annotation makes the `getOpportunities` method visible to Aura components and is automatically loaded by components.

``` java
public class OpportunityListModel {
    
    private List<Opportunity> opportunities;
    
    public OpportunityListModel() {
        init();
    }
    
    private void init() {
        opportunities = (List<Opportunity>) setCon.getRecords();
    }    
    
    public ApexPages.StandardSetController setCon {
        get {
            if(setCon == null) {
                setCon = new ApexPages.StandardSetController(Database.getQueryLocator(
                    [SELECT Id, Name, CloseDate FROM Opportunity]));
            }
            return setCon;
        }
        set;
    }
    
    @AuraEnabled
    public List<Opportunity> getOpportunities() {
        return opportunities;
    }    
}
```

## opportunity Component

Create a component called `opportunitySelect`. This component will connect to SFDC and get list of opportunities via `OpportunityListModel`. Once it retrieves the list, it displays them in a drop-down list. And when a user selects an Opportunity from the list, it will fire an Aura event. 

1. Select File > New > Aura Component
2. Enter opportunitySelect for the name
3. Click Submit

<img src="/auratutorials/images/aura-opp-create-opp-list-devconsole-ac.png"/>

### Component Markup

Add the following markup to `opportunitySelect` component. Make sure to change the namespace to your Org's namespace.

``` html
<aura:component model="NAMESPACE.OpportunityListModel">
	<aura:handler name="init" value="{!this}" action="{!c.doInit}" />
	<select aura:id="opportunitySelect" onchange="{!c.onSelectOpportunity}"/>
</aura:component>
```

Let's take a look at what's going on line by line.

1. `<aura:component model=”NAMESPACE.OpportunityListModel”>`
Makes this component connect to `OpportunityListModel` on the server.

2. `<aura:handler name=”getOpportunities” value=”{!this}” action=”{!c.doInit}”/>`
When Aura loads a component, Aura fires an `init` event for components to initialize business logic (in JavaScript controllers). `aura:handler` component provides a way to listen to such events(`init`), pass some info to events publishers (`{!this}`) and call a controller function (`{!c.c.doInit}`) to do something.

3. `<select aura:id="opportunitySelect" onchange="{!c.onSelectOpportunity}"/>`
This is a standard HTML Select tag whose id is `aura:id` (unique component namespaced id). It listens to `onchange` and calls `onSelectOpportunity` function in the client-side controller.

{% note info HTML tags  = Aura Component %}
Aura treats regular HTML tags also as Aura components and adds all the component features to them as well!
{% endnote %}

### Create A Controller

Javascript Controllers act like a middleman between Views (component markups) and Models (Apex Models).  Every function in JS controller gets three parameters.

1.  `component` - The component it belongs to
2.  `Event` - The event that triggered the call.
3.  `helper` - A JS object that holds functions that can be reused (if there are multiple copies of the component). 

{% note info Controller Vs. Helper %}
Think of functions inside JS controller as "instance methods" and think of functions inside JS Helper as "Static methods". 
Tip: If your function doesn't interact directly with the View, put it in Helper.
{% endnote %}

Add a component controller that deals with `init` event from Aura and also handles `onchange` event when a user select an opportunity from the list.

1. Open `opportunitySelect.cmp` and click on CONTROLLER on the side panel. 
<img src="/auratutorials/images/aura-opp-create-opp-list-open-comp.png"/>
2. Copy paste the below code into the controller.

``` JavaScript
({
    doInit: function(component, evt, helper) {
        setTimeout(function() {
            var items = component.getValue("m.opportunities").unwrap();
            var select = component.find("opportunitySelect").getElement();
            var option = null;
            for (var i = 0; i < items.length; i++) {
                item = items[i];
                option = document.createElement("option");
                option.textContent = items[i].Name;
                option.value = items[i].Id;
                select.appendChild(option);
            }
            helper.fireSelectOpportunity(items[0].Id);
        }, 10);
    },
    
    onSelectOpportunity : function(component, event, helper) {
        var select = component.find("opportunitySelect").getElement();
        var value = select.value;
        helper.fireSelectOpportunity(value);
    }
})
```

Let's take a look at the code.

1. <b>doInit: function(component, evt, helper) {</b>
Remember we had `<aura:handler name=”getOpportunities” value=”{!this}” action=”{!c.doInit}”/>` in our View markup? Because of that doInit is called when the component receives `init` event from Aura. 

2. <b>var items = component.getValue("m.opportunities").unwrap();</b>
<b>component.getValue</b> is a way to get values from either models or views. Use `m.` prefix to get values from <b>server</b> models and use `v.` prefix to get value from views. As you can imagine `component.getValue("m.opportunities").unwrap()` simply returns list of opportunities.
 
 ``` html
For example:
1. `component.getValue("v.myInputfieldValue")` - Gets value from an input field.
2. `component.getValue("m.getContacts")` - Gets value from SFDC, if the server has `contacts` method (with a lowercase "c"). You'll also need to call `unwrap` to convert it to JSON.
```

3. <b>for (var i = 0; i < items.length; i++) {</b>
Once it gets the list of opportunities, it simply populates the list.
4. <b>helper.fireSelectOpportunity(items[0].Id);</b>
Calls a helper function `fireSelectOpportunity` that internally fires an event indicating that the first item was selected.


## Create a Helper

Create a helper by clicking on the side panel and add the following code. <b> Remember to change namespace</b>.

```JavaScript
({
    fireSelectOpportunity : function(id) {
        var selectOpportunityEvent = $A.get("e.NAMESPACE:selectOpportunity");
        selectOpportunityEvent.setParams({id: id});
        selectOpportunityEvent.fire();    		
    }
})
```

Let's look at the code:

1. <b>var selectOpportunityEvent = $A.get("e.NAMESPACE:selectOpportunity");</b>
`$A` returns a global Aura object that provides various Aura functions like`$A.get` that allows retrieving an artifact by name string.  `$A.get("e.NAMESPACE:selectOpportunity")` returns an event (`e.` prefix) in your namespace with name `selectOpportunity`.

2. <b>selectOpportunityEvent.setParams({id: id})</b>
Events allows us to pass data. In this case we are setting Opportunity Id.

3. <b>selectOpportunityEvent.fire()</b>
You can then finally fire the event and if anyone who may be listening to it will get the data.

## Embed it in an App

Create an app called `opportunities.app` and embed this component in it.

<img src="/auratutorials/images/aura-opp-create-opp-list-createApp.png"/>

Add the following code to the app and <b>Change the namespace to your org's namespace</b>

``` html
<aura:application>
	<NAMESPACE:opportunitySelect/>
</aura:application>
```

## Test the app

Click on the `Preview` button in the side bar of the Developer console or go to: `<your org>/<namespace>/opportunities.app` in a browser.

<img src="/auratutorials/images/aura-opp-opplist-select-list.png"/>
