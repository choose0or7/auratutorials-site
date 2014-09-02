title: Opportunity List App - Running In Salesforce1 Mobile
---
##Overview

Running our app in Salesforce1 mobile app is very similar to adding Visualforce tabs to Salesforce1. However there are some requirements and limitations.

### Requirements & Limitations

* Components are the integration point, <b>not Apps</b>
* Components must implement force:appHostable 
* Styling, sizing, etc. are not provided
* Not integrated with other S1 components

## Create A Component

We can only embed a component inside another component but not an app inside another app. Since Salesforce1 app is itself is an app, we need can only embed our component. Create a component.

1. Select File > New > Aura Component
2. Enter opportunityPanel for the name
3. Click Submit
4. Select opportunityPanel.cmp
5. Add the below markup to your component.
	
	``` html
	<aura:component implements="force:appHostable">
	    <div class="container">
	        <div aura:id="content" class="center content">
	            <div>
	                <NAMESPACE:opportunitySelect/>
	                <div aura:id="cards">
	                    <NAMESPACE:opportunityCard/>
	                </div>
	            </div>
	        </div>
	    </div>	
	</aura:component>

	```	

6. Change the namespace to yours
7. Save the changes

{% note info force:appHostable %}
Note: Components must have `implements="force:appHostable"` to embed in Salesforce1 Mobile app.
{% endnote %}

## Create Aura Tabs

Creating an Aura tab is similar to creating a Visualforce tab. If your org has Aura on the platform enabled, you will see `Aura Tabs` section for you to add tabs.

1. Navigate to Setup in the browser
2. Expand Create and select Tabs
3. Click the New button in the Aura Tabs section
4. Select `<ns>:opportunityPanel` from the Aura selector
5. Set the Tab Label to `Opportunities (AotP) `
6. Set the Tab Name to `Opportunities_AotP`
7. Choose a Tab Style
8. Press the Next button
9. Press the Save button on the next screen

## Setup Mobile Navigation

1. Ensure that you are in Setup
2. Type “mobile n” in the search box
3. Select Mobile Administration -> Mobile Navigation
4. Select `Opportunities (AotP)` in the Available list
5. Click the Add button to move it to the Selected list
6. Select `Opportunities (AotP)`) in the Selected list
7. Press the Up button to move it below People
8. Press the Save button

<img src="/auratutorials/images/hello-world-mobile-navigation.png"/>

## View in Salesforce1 Mobile

Salesforce1 mobile app is runs in `https://<your org domain>/one/one.app`. Open Salesforce1  app and click on the "hamburger icon". You should see your "Opportunities (AotP)" app.

<img src="/auratutorials/images/aura-opp-add-styles-final-app-in-s1.png"/>
