title: Opportunity Select App - Creating Events
---
## Events

There are two types of events in Aura. A component level (`COMPONENT`) event and an application level global (`APPLICATION`) event. A component-level event can be handled by a component itself or by a component that instantiates or contains the component. Where as application-level events are global to the app.

### Passing data

Events can also pass data from publisher to listener via `aura:attribute` property.

## Create Event

Create an application-level Event `selectOpportunity` that can pass data in `id` parameter. In our example this id will contain opportunity id.

1. Select File > New > Aura Event
2. Enter `selectOpportunity` for the event name
3. Click Submit
	<img src="/auratutorials/images/aura-opp-create-event.png"/>
4. Paste the following code.
	``` html
	<aura:event type="APPLICATION">
		<aura:attribute name="id" type="String"/>
	</aura:event>
	``` 
5. Save