<!-- loio2e1e3931d39b4e2e88a411799de31616 -->

# Accessing the Landscape Portal

The Landscape Portal is part of SAP BTP ABAP environment and requires no additional license. To set up access to the Landscape Portal, you just need to add the Landscape Portal as entitlement to your subaccount, subscribe to it, and assign the necessary role collections to your user. Once you have done this, you can easily access the Landscape Portal by clicking on its tile in the Service Marketplace.

> ### Note:  
> *Important*: The Landscape Portal can only be accessed from subaccounts located in region cf-eu10. Please use a subaccount located in this region to add the entitlement and to subscribe to and access the Landscape Portal.
> 
> You can manage systems for \(and deploy your SaaS solution to\) all regions within the same global account using the same Landscape Portal instance. Only the subaccount used for Landscape Portal access must be located in cf-eu10.



<a name="loio2e1e3931d39b4e2e88a411799de31616__section_pgv_55b_jtb"/>

## Adding the *Landscape Portal* as Entitlement

The*Landscape Portal* needs to be assigned to your subaccount as an entitlement.

1.  Sign into your account in the *SAP BTP Cockpit*.
2.  On global account level, navigate to *Entity Assignments* in the Entitlements menu tree.
3.  Open the value help for *Select Entities* and choose the subaccount \(located in cf-eu10\) to which the *Landscape Portal* entitlement shall be added. Click *Select*.
4.  Click the button *Configure Entitlements* and then *Add Service Plans*.
5.  Select*Landscape Portal* from the list and choose the service plan *standard*, then click *Add 1 Service Plan*.
6.  Click *Save* to add the service plan to your selected subaccout.



<a name="loio2e1e3931d39b4e2e88a411799de31616__section_phm_2rp_qmb"/>

## Subscribing to the *Landscape Portal* in the SAP BTP cockpit

To enable the *Landscape Portal*, you need to subscribe to it in the SAP BTP cockpit. Here’s how:

1.  Sign into your account in the *SAP BTP Cockpit*.
2.  Select your global account, then navigate to *Subaccounts* and select your production subaccount \(located in cf-eu10\).
3.  Navigate to *Service Marketplace*.
4.  The *Landscape Portal*tile is now visible and says "*Not subscribed*".
5.  Click the tile to subscribe to it. The subscription process might take a few minutes. When everything is ready, the tile will say *Subscribed*. From now on, you can open the *Landscape Portal* simply by navigating to the tile and clicking on*Go to application*.



<a name="loio2e1e3931d39b4e2e88a411799de31616__section_rr3_5sp_qmb"/>

## Assigning the Landscape Portal Role Collections

In order to log into the *Landscape Portal*, a user first needs to be assigned the necessary role collection. This is done by the security administrators of your production subaccount in the SAP BTP cockpit. For more information on how to add security administrators, see [Add Security Administrators to Your Subaccount \[Feature Set A\]](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/fea877c449ba4c5fbb0aafd92a80afb4.html). For more information on how to assign role collections, see [Assign Users to Role Collections](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/c5766765bda74ad59fe656977c8fa4d6.html).

> ### Remember:  
> You need to be a security administrator in your production subaccount in order to assign the role collections. 

There are two different role collections with which you can access the *Landscape Portal*:

-   Landscape Portal Admin \("LandscapePortalAdminRoleCollection"\): Full functionality of the *Landscape Portal*.

-   Landscape Portal User \("LandscapePortalUserRoleCollection"\): Read-only view of the *Landscape Portal*and the right to create Support Users.


> ### Note:  
> If you have subscribed to the *Landscape Portal* from multiple subaccounts, you have to adjust the role collection assignment consistently, that is you need to assign the same role collections to the same user in every subaccount you are accessing the *Landscape Portal* from.

