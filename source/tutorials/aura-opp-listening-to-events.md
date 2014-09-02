title: Opportunity Select App - Handling Events
---

In this tutorial we will create another component `opportunityCard.cmp` that listens to `opportunitySelect.evt` event and then calls an Apex controller `opportunityController` on the server to retrieve component details. Finally `opportunityCard.cmp` uses the data from `opportunityController` and displays it in a simple form.

## Create Apex Controller.

Create `opportunityController` Apex controller.

1. Select File > New > Apex Class
2. Enter OpportunityController for the name
3. Click Submit	
	<img src="/images/aura-opp-create-apex-controller.png"/>
4. Paste the following code. It has a method `getOpportunity` that returns an Opportunity details for a given `Id`. 

	``` java
	public class OpportunityController {
	
	    @AuraEnabled
	    public static Opportunity getOpportunity(Id id) {
	        Opportunity opportunity = [
	 			SELECT Id, Account.Name, Name, CloseDate, Owner.Name, Amount, Description, StageName
	            FROM Opportunity
	            WHERE Id = :id
	         ];
	        
	        return opportunity;
	    }
	}
	```

{% note info @AuraEnabled and static %}
	Note: Aura controller can <b>only call</b> Apex controller methods that have `@AuraEnabled` annotation and are `Static`.
{% endnote %}

<br/><br/>

## Create 2nd Component

Let's create another component `opportunityCard.cmp`.

1. Select File > New > Aura Component
2. Enter opportunityCard for the name
3. Click Submit
	<img src="/images/aura-opp-create-oppCardComp.png"/>
4. Add the following code to connect to `opportunityController` Apex controller.

``` html
<aura:component controller="NAMESPACE.OpportunityController">
</aura:component
```
(Remember to change the namespace)

## Handle Event

Update the opportunityCard.cmp` Component's markup to handle the event.

``` html
<aura:component controller="NAMESPACE.OpportunityController">
	<aura:handler event="NAMESPACE:selectOpportunity" action="{!c.handleSelectOpportunity}" />
	<div>Id: {!m.Id}</div>
	<div>Name: {!m.Name}</div>
	<div>Description: {!m.Description}</div>
	<div>Amount: {!m.Amount}</div>
	<div>Close Date: {!m.CloseDate}</div>
	<div>Stage Name: {!m.StageName}</div>
	<div>Owner Id: {!m.OwnerId}</div>
	<div>Owner Name: {!m.OwnerName}</div>
</aura:component>
```

Let's look at the code:

1. <b><aura:component controller="NAMESPACE.OpportunityController"></b>
 This connects the component to Apex controller.
2. <b><aura:handler event="NAMESPACE:selectOpportunity" action="{c.handleSelectOpportunity}" /></b>
This makes this component to listen to `NAMESPACE:selectOpportunity` and call Javascript controller `c.handleSelectOpportunity`.
3. <b>`<div>Id: {!m.Id}</div>`</b>
`m.` prefix stands for model. Typically when we get data from the server it can be accessed by using `m.myServerData`.

##  Add it to the App

Open our `Opportunities.app` and embed this component `<NAMESPACE:opportunityCard/>` below `opportunitySelect` component so the application looks like below:

``` html
<aura:application>    
	<NAMESPACE:opportunitySelect/>
	<NAMESPACE:opportunityCard/> 
</aura:application>
```

## Test The App

Click on the "Preview" button in the side panel of the app or open up `https://<yourOrg>/<namespace>/<appName>.app` in a browser.
 
<img src="/images/aura-opp-both-controller-in-the-app.png"/>
