title: Static Resources App -  Load Dependent Files
---

In this tutorial we will be using [load.cmp](https://github.com/rajaraodv/loadcomponent) resource loader component to load CSS and JavaScript. We can use any other script loaders like requireJS as long as it is wrapped in a component.

## Static Resources

Only way to load external scripts into Aura app is by first uploading them to Static Resources.

### Static Resource Paths

1. If the resource is not in a Zip file, you can access it by going to `<yourOrg>/resource/yourfile` (Note: there is no JS or CSS extension).
2. If the resource is inside a Zip file, you can access it by going to `<yourOrg>/resource/zipfileName/path/to/file.js` (note you need to provide file extension).

## Upload static resources

This tutorial uses three files: a `jquery` ([jquery.js](/files/twtr-bootstrap-app/jquery.js)) file and a `bootstrap.css` + `bootstrap.js` contained inside a zip file ([aotp_bootstrap.zip](/files/twtr-bootstrap-app/aotp_bootstrap.zip)).

Please follow the steps to upload both static resources.
1. Download both of them.
2. Setup > Develop > Static Resources
3. Upload <b>jquery.js</b> file with <b>name</b> `jquery`
4. Upload <b>aotp_bootstrap.zip</b> with the resource  <b>name</b> `aotp_bootstrap`

{% note info Dependency Manamegement %}
Note: In our example, bootstrap.js is actually dependent on jQuery. Thankfully load.cmp handles even dependencies in addition to loading either CSS or JS files.
{% endnote %}

## Load load.cmp

Load `load.cmp` own files <b>manually</b>

1. Go to [https://github.com/rajaraodv/loadcomponent](https://github.com/rajaraodv/loadcomponent)
2. Create `load.cmp`, `loadController.js` and `staticResourcesLoaded.evt` files in your org 
3. Copy-paste contents from each of those files.
4. Change namespace from `jam` to your org's namespace in all those files.

{% note info Manual loading %}
Unfortunately, at the time of writing this tutorial there is no tool to automatically upload components.
{% endnote %}

