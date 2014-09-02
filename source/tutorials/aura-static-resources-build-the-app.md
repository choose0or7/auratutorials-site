title: Static Resources App -  Build The App
---

## SRCmp.cmp

1. Open Developer Console.
2. Go to New > Aura Component
3. Enter Name `SRCmp`
4. Hit Submit
5. Click on the `Component` button in the side panel.
6. Paste the following content:

``` html
<aura:component>
   <aura:attribute name="title" type="String" default="Modal"/>
  	
   <!-- listen to staticResourcesLoaded and call "initScripts" function inside controller -->
   <aura:handler event="jam:staticResourcesLoaded" action="{!c.initScripts}"/>
    
   <div class="aotp">
      <div class="container">
         <!-- Button triggers a modal dialog-->  
         <div class="row well">     
            <button aura:id="modalToggle" class="btn btn-primary btn-lg">
            Launch Modal - JavaScript
            </button>            
         </div>
         <br />
         <!-- modal dialog -->
         <div class="modal fade" aura:id="modalDlg">
            <div class="modal-dialog">
               <div class="modal-content">
                  <div class="modal-header">
                     <button type="button" class="close" data-dismiss="modal"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
                     <h4 class="modal-title">Dialog Title</h4>
                  </div>
                  <div class="modal-body">
                     GlobalId = {!globalId}  <br/> This is Dialog Body
                  </div>
                  <div class="modal-footer">
                     <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                  </div>
               </div>
               <!-- /.modal-content -->
            </div>
            <!-- /.modal-dialog -->
         </div>
         <!-- /.modal -->
      </div>
   </div>
</aura:component>
```
7. <b>Change the namespace "jam" to your org's namespace</b>


###Code Highlights:

1. `<aura:handler event="jam:staticResourcesLoaded" action="{!c.initScripts}"/>`
This makes this component listen to `jam:staticResourcesLoaded` event and call `initScripts` function in its JavaScript controller.

2. The rest of the code is simply markup to create a Twitter Bootstrap button and a Modal dialog. 

3. `<button aura:id="modalToggle" class="btn btn-primary btn-lg">`
`aura:id` with a value `modalToggle` makes it easy to find from within component. It is recommended to use `aura:id` instead of regular `id` because your app may have multiple components.

<br/>
## SRController.js

1. Open `SRCmp.cmp`and click on `Controller` button in the side panel.
2. Paste the following code.
``` javascript
({
    initScripts: function(component, event, helper) {

        //Ignore duplicate notifications that may arrive because other components 
        //loading scripts using the same library. 
        if (component.alreadyhandledEvent)  
            return;
        
            var btn = component.find("modalToggle").getElement();
            var dlg = component.find("modalDlg").getElement();
            jQuery(btn).on("click", function() {
                jQuery(dlg).modal();
            });
            component.alreadyhandledEvent = true;
        }
    }
})
```

{% note tip Application-Level Events %}
Note: If you are listening to `Application level` events, you should guard your code from duplicate notifications!
{% endnote %}

<br/>
## SRApp.app

1. Open Developer Console.
2. Go to New > Aura Application
3. Enter Name `SRApp`
4. Hit Submit
5. Click on the `Application` button in the side panel.
6. Paste the following content:

``` html

<aura:application>
  <!--
    1. Loads aotp_bootstrap.css from a aotp_bootstrap(zip file) and loads jQuery file (not in zip file; notice "sfjs" extension)
  in parallel.
    And then loads bootstrap.js from aotp_bootstrap(zip file) as bootstrap.js is dependent on jQuery.
  -->
  <jam:load
    filesInParallel="/resource/aotp_bootstrap/css/aotp_bootstrap.css,/resource/jquery.sfjs"
    filesInSeries="/resource/aotp_bootstrap/js/bootstrap.js"
  />
  
  <div>1st copy of the component</div>
  <NAMESPACE:SRCmp/>
    
  <div>2nd copy of the component</div>
  <NAMESPACE:SRCmp/>
</aura:application>

```

## Test The App

1. Click on the convenient “Preview” button on the side bar to open up the app - You should see Twitter Bootstrap theme.
2. Click on the button and you should see a modal dialog triggered by jQuery.
3. Since we are guarding against multiple notifications, both buttons should work normally.

<img src="/auratutorials/images/aura-bootstrap-app-final.png"/>
<br>
<img src="/auratutorials/images/aura-bootstrap-app-final-dlg.png"/>
