title: Hello World App - Running In Salesforce1 Mobile
---
##Overview

Running our app in Salesforce1 mobile app is very similar to adding Visualforce tabs to Salesforce1. However there are some requirements and limitations.

### Requirements & Limitations

* Components are the integration point, <b>not Apps</b>
* Components must implement force:appHostable 
* Styling, sizing, etc. are not provided
* Not integrated with other S1 components

## Add force:appHostable

We can only embed a component inside another component but not an app inside another app. Since Salesforce1 app is itself is an app, we need only embed our component.
Add `implements="force:appHostable"` to <b>helloworld.cmp</b>.

``` html
<aura:component implements="force:appHostable">
    Hello World!
</aura:component>
```

## Hello World App - Overview

Creating an Aura tab is similar to creating a Visualforce tab. If your org has Aura on the platform enabled, you will see `Aura Tabs` section for you to add tabs.

1. Navigate to Setup in the browser
2. Expand Create and select Tabs
3. Click the New button in the Aura Tabs section
<img src="/images/hello-world-add-aura-tabs.png"/>
4. Select `<ns>:helloworld` from the Aura selector
5. Set the Tab Label to `Hello World`
6. Set the Tab Name to HelloWorld
7. Choose a Tab Style
8. Press the Next button
9. Press the Save button on the next screen

<img src="/images/hello-world-add-aura-tabs-save.png"/>

## Setup Mobile Navigation

1. Ensure that you are in Setup
2. Type “mobile n” in the search box
3. Select Mobile Administration -> Mobile Navigation
4. Select `Hello World` in the Available list
5. Click the Add button to move it to the Selected list
6. Select `Hello World`) in the Selected list
7. Press the Up button to move it below People
8. Press the Save button

<img src="/images/hello-world-mobile-navigation.png"/>


## View in Salesforce1 Mobile

Salesforce1 mobile app is runs in `https://<your org domain>/one/one.app`. Open Salesforce1  app and click on the "hamburger icon". You should see your "Hello World" app.

<img src="/images/hello-world-in-s1.png"/>