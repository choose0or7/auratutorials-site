title: Aura (AotP) Setup
---
## Environment

AotP is currently in Pilot. Please contact Salesforce to sign-up for a sandbox org on **gs0**. You will get a standard Developer Edition license with Aura on the Platform enabled.

## Namespaces

Aura uses namespaces **extensively** for encapsulation. Every component needs to have namespace prefixed like `<myNamespace:componentName>`. For example Aura uses `ui:` for ui-elements `<ui:inputFieldText>` like input fields and `aura:` for others like: `<aura:handler>`, `<aura:event>` etc. So register a unique namespace for your org. Create a namespace using Setup.
1. Navigate to Setup
2. Select Create > Packages
3. Click Edit
4. Click Continue to agree
4. Enter namespace and click Check Availability
6. Click Review My Selections
7. Click Save

<img src="/images/aura-setup-namespace.png"/>

### Using Namespaces

1. Every component must be namespace prefixed. 
2. You will have to use your own unique namespace for components you build like: `<myNamespace:componentName>`.
3. Namespaces are also used in the url for any Aura apps: ` http://<mySalesforceInstance>/<namespace>/<app>.app`

{% note tip Copying Components %}
When you **manually** copy code from tutorials and other places, you'll have to **manually** change it's namespace to yours.
{% endnote %}

<br/><br/>
## Aura Debug Mode

By default Aura minifies JavaScript and CSS code which makes it impossible to debug during development. So make sure to enable Debug mode.
1. Navigate to Setup > Create > Packages
2. Check `Enable Aura Debug Mode`

<img src="/images/aura-setup-enable-debug.png"/>

