<!-- loio3943c74328df43ad9a1fd22c39882aae -->

# Using an SAP BTP System as ATC Central Check System

You can use a system in SAP BTP, ABAP Environment as a central check system to run ATC checks from an on-premise system against this system \(ATC Developer Scenario\).



<a name="loio3943c74328df43ad9a1fd22c39882aae__prereq_hzn_gys_zyb"/>

## Prerequisites

-   You’ve defined communication arrangement `SAP_COM_0464` \(SAP Custom Code Migration Integration\) to enable communication from your ABAP environment to your on-premise systems using remote function calls \(RFC\). For more information, see [Maintaining Communication Arrangements](https://help.sap.com/docs/btp/sap-business-technology-platform/maintaining-communication-arrangements-52579888e08546ea80700c5df791582e).

-   You’ve implemented [SAP Note 3358660 - Developer Scenario Cloud](https://me.sap.com/notes/3358660/E) in your checked system.



## Context

To enable the ATC Developer Scenario, you've to define communication arrangement `SAP_COM_0936` for your cloud system.



## Procedure

1.  Log on to the *SAP Fiori launchpad* of your ABAP environment.

2.  In the *Communication Management* section, select the *Communication Arrangements* tile.

3.  Choose *New*.

4.  Use the value help to choose the `SAP_COM_0936` scenario.

5.  Define and enter an *Arrangement Name*.

6.  If not yet available or defined, create a new communication system for the communication arrangement to define an endpoint for your checked system.

    1.  For the *Communication System*, choose *New*.

    2.  In the *New Communication System* dialog, enter the *System ID* and *System Name* of the checked system.

    3.  Choose *Create*.

    4.  In the *Technical Data* tab under *General*, select the *Inbound Only* check box.

    5.  In the *Users for Inbound Communication* tab, choose *\+* to add a new inbound communication user.

    6.  Choose *User Name and Password* as *Authentication Method*, and enter a user name, description, and password for the user.

    7.  Choose *Create*.


7.  In the Communication Arrangement in the *Inbound Communication* tab, enter the *User Name* you’ve previously defined as described in step 6.

8.  In the *Additional Properties* tab, select the *Object Provider* you’ve defined in `SAP_COM_0464`.

9.  Choose *Save*.

10. In your checked system, run transaction `ATC`.

    The *ABAP Test Cockpit* is opened.

11. Choose *ATC Administration* \> *Setup* \> *Basic Settings*.

12. In the *Code Inspector* section, enter the *Reference Check System*. This is the RFC connection to your cloud system. If not yet available or defined, create a new RFC destination in transaction `SM59`. For more information, see [Maintaining Remote Destinations](https://help.sap.com/docs/ABAP_PLATFORM_NEW/8f3819b0c24149b5959ab31070b64058/488965b484b84e6fe10000000a421937.html).

    > ### Note:  
    > Choose connection type "3" \(ABAP Connection\) to connect to your cloud system via [Cloud Connector](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/cloud-connector?version=Cloud). Connection type "W" \(WebSocket RFC\) isn't yet supported for the developer scenario.

13. Go to transaction `SCI` and create a proxy variant \(as described in step 2 in [Configuring the Checked System](https://help.sap.com/docs/ABAP_PLATFORM_NEW/ba879a6e2ea04d9bb94c7ccd7cdac446/17eb1a1d504442b3ad438451197b937b.html)\).

    You can reference one of the SAP delivered ATC variants in your Cloud system, for example `SLIN_SEC`, or one of your own variants that you've created in the Cloud system.

14. Go back to transaction `ATC` in the *Basic Settings* and enter the name of this proxy variant as *Global Check Variant*.

15. Confirm.




<a name="loio3943c74328df43ad9a1fd22c39882aae__result_snj_hct_zyb"/>

## Results

You've enabled a cloud system as central check system for remote ATC checks from an on-premise system against the cloud system. In SAP GUI or ABAP Development Tools for Eclipse, you can now run ATC checks in your checked system. In ABAP Development Tools for Eclipse in the *ATC Problems* view, you can create exemptions that you can then manage in the [Approve ATC Exemptions](https://help.sap.com/docs/btp/sap-business-technology-platform/approve-atc-exemptions) app.

