title: Hello World App - Component
---

## Create a component

Instead of having html directly inside our app, let's put it in a component and embed this component inside the app. 

Adding markups in components instead of .app makes it more portable. For example you can easily embed it in Salesforce1 Mobile App or in other apps.

``` html
1. Select File > New > Aura Component
2. Enter `helloworld.cmp` for the name
3. Click Submit
4. Add the following code
<aura:component>
	Hello World!
</aura:component>
5. Save
```

<img src="/images/aura-first-aura-app-createComponent.png" width="400px"/>
<br/><br/>
<img src="/images/hello-world-create-comp.png"/>
<br><br>

## Embed Component In App

Open helloworld.app and embed your component in it. <b>Make sure to change the namespace "jam" to your namespace </b>

``` html
<aura:application>
    <jam:helloWorld />
</aura:application>
```
	
<img src="/images/hello-world-embed-component.png"/>
<br>

## Test The App

Click on the convenient "Preview" button on the side bar to open up the app.

<img src="/images/hello-world-click-preview-button.png"/>

