title: Intro To Aura
---
## Aura

Aura is a component based UI framework to build mobile and web apps on Salesforce Platform.


## Why Aura

1. Aura provides an easy way to <b>cleanly breakup your apps</b> into distinct and encapsulated <b>components</b> while still allowing them to interact with each other by passing <b>events</b>. This makes it easy for teams to build complex apps with ease. 
2. Aura also comes with whole host of pre-built components and <b>enterprise features</b> like versioning, localization support, caching techniques to name a few.
3. Helps build better and more modern mobile apps than Visualforce but with the same skillset.
4. Aura is <b>already battle tested</b> in several apps including [official Salesforce1 mobile app](http://www.salesforce.com/mobile/) and several Salesforce apps. 

<img src="/images/aura-what-is-aura-s1mobileapp.png" alt="Drawing" style="height: 436px;"/>

## Aura Framework Flavors

It comes in two flavors, `Aura OSS` and `Aura On The Platform(AotP)` although both share the same code base. 

### Aura OSS

This is the open source version of Aura. This can run on any Java back-end and is a super-set of Aura on the Platform. You can get find more details here: [http://documentation.auraframework.org/](http://documentation.auraframework.org/auradocs#)

### Aura On The Platform (AotP)

This version of Aura uses APEX backend and runs inside Salesforce1 platform. <b>If you want to build Aura apps and run it inside Salesforce org or inside Salesforce1 mobile app, you should use AotP</b>.

{% note tip AotP Only %}
All the tutorials contained in this site are AotP only. 
{% endnote %}
