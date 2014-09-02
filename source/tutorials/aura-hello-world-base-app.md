title: Hello World App - Create The App
---

## Open Developer Console

Open developer console by using following steps


 1. Login to your pilot org
 2. Click on the user dropdown menu
 3. Select the link to open the Developer Console.


<p align=center>
<img src="/auratutorials/images/aura-first-aura-app-devconsole.png"/>

## Create HelloWorld App


1. Select File > New > Aura Application
2. Enter application name, e.g. helloworld
3. Click Submit


<p align=center>
<img src="/auratutorials/images/aura-first-aura-app-createApp.png"/>
<p align=center>
<img src="/auratutorials/images/aura-first-aura-app-createApp-bundle.png"/>

## Aura Bundles

Every component is made up of a set files together known as an `Aura Bundle`. An Aura Bundle is nothing but a folder containing various parts of a component or an app.

<img src="/auratutorials/images/aura-first-aura-app-comp-bndl-files.png"/>

``` html
	Note: Aura App "*.app" itself is a component with some special features.
```

### Aura Bundles In Developer Console

Developer console  shows all these parts of a component or an app in the side bar. You can click on it to edit any of them.

<img src="/auratutorials/images/aura-first-aura-app-console-side-bar.png"/>

## Save The App

Select the `helloworld.app` app from the side bar, add some text to the body of the app and save it (File > Save). 

``` html
<aura:application>
	Hello, World!
</aura:application>
```

<p align=center>
<img src="/auratutorials/images/aura-first-aura-app-hello-world-app.png"/>

## Test the App

Aura apps use the following URL pattern: `<protocol>://<hostname>/<namespace>/<appname>.app`
(Example: `https://gs0.salesforce.com/aotp1/helloworld.app`). Open the browser and open up your app.

<img src="/auratutorials/images/aura-first-aura-app-hello-world-app-running.png"/>

{% note info Salesforce1 Mobile App %}
Note: Salesforce1's mobile app uses `one` as namespace and is named `one.app`. So if you open `<protocol>://<hostname>/one/one.app`, you'll see it running just like your helloworld app.
<img src="/auratutorials/images/aura-first-aura-app-s1app.png"/>
{% endnote %}
