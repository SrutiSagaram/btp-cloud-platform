<!-- loioff540477f5404e3da2a8ce23dcee602a -->

# Develop the Multitenant Application

In the Cloud Foundry environment of SAP BTP, you can develop and run multitenant applications that can be accessed by multiple consumers \(tenants\) through a dedicated URL.



## Context

When developing tenant-aware applications in the Cloud Foundry environment, keep in mind the following general programming guidelines:

-   Shared in-memory data such as Java static fields will be available to all tenants.

-   Avoid any possibility that an application user can execute custom code in the application JVM, as this may give them access to other tenants' data.

-   Avoid any possibility that an application user can access a file system, as this may give them access to other tenants' data.

-   To perform internal tenant onboarding activities, such as creating a database schema for tenants, you must implement the `Subscription` callbacks of the SAP Software-as-a-Service Provisioning service \(`saas-registry`\) and use the information provided in the subscription event. You can also implement the `getDependencies` callback to obtain the dependencies of any SAP reuse services by your application. See details in the procedure below.




## Procedure

1.  Develop the application as for any other Java and Node.js application on the SAP BTP.

    For more information, see [Developing Java in the Cloud Foundry Environment](developing-java-in-the-cloud-foundry-environment-a3f9006.md).

2.  In your application, you must implement a REST API that will be called by the SAP SaaS Provisioning service \(`saas-registry`\) on a subscription event. Optionally, you can implement the `getDependencies` REST API to obtain the dependencies of any SAP reuse services consumed by your application.

    When a consumer tenant creates or revokes a subscription to your multitenant application via the cockpit, the SAP SaaS Provisioning service calls the application with two `Subscription` callbacks. The first callback gets the dependencies of any reuse services used by your application. The second callback informs the application about the subscription event \(`PUT` for subscribe and `DELETE` for unsubscribe\) and provides details about the subscriber \(the consumer tenant\).

    -   To inform the application that a new consumer tenant wants to subscribe to the application, implement the callback service with the `PUT` method. The callback must return a 200 response code and a string in the body, which is the access URL of the tenant to the application subscription \(the URL contains the subdomain\). This URL is generated by the multitenant application based on the approuter configuration \(see [Configure the approuter Application](configure-the-approuter-application-5af9067.md)\)

        > ### Sample Code:  
        > Callback response \(`PUT`\).
        > 
        > ```
        > https://subscriber_subdomain-approuter_saas_app.cfapps.eu10.hana.ondemand.com
        > ```
        > 
        > The URL must be in the format: `https://<SUBSCRIBER_TENANT_SUBDOMAIN >-<APPROUTER_APPLICATION_HOST>.<CF_DOMAIN>`

    -   To inform the application that a tenant has revoked its subscription to the multitenant application and is no longer allowed to use it, implement the same callback with the `DELETE` method.

        > ### Sample Code:  
        > Payload of subscription `PUT` and `DELETE` methods:
        > 
        > ```
        > {
        >     "subscriptionAppId": "<value>",                     # The application ID of the main subscribed application.
        >                                                         # Generated by Authorization and Trust Management service(xsuaa)-based on the xsappname.
        >     "subscriptionAppName": "<value>"                    # The application name of the main subscribed application.
        >     "subscribedTenantId": "<value>"                     # ID of the subscription tenant
        >     "subscribedSubdomain": "<value>"                    # The subdomain of the subscription tenant (hostname for 
        >                                                         # the identityzone).
        >     "globalAccountGUID": "<value>"                      # ID of the global account
        >     "subscribedLicenseType": "<value>"                  # The license type of the subscription tenant
        > 
        > }
        > ```

    -   If your application consumes any reuse services provided by SAP, you must implement the `getDependencies` callback to return the service dependencies of the application. The callback must return a 200 response code and a JSON file with the dependent services `appName` and `appId`, or just the `xsappname`.

        > ### Note:  
        > The JSON response of the callback must be encoded as either UTF8, UTF16, or UTF32, otherwise an error is returned.

        > ### Sample Code:  
        > Sample dependency response:
        > 
        > ```
        > 
        > [ 
        >    {
        >       "xsappname" : "<value>"   # The xsappname of the reuse service which the application consumes.
        >                                 # Can be found in the environment variables of the application:
        >                                 # VCAP_SERVICES.<service>.credentials.xsappname
        >    }
        > ]
        > ```


    For more information about the subscription callbacks and dependencies, see [Register the Multitenant Application to the SAP SaaS Provisioning Service](register-the-multitenant-application-to-the-sap-saas-provisioning-service-3971151.md).

3.  Configure the application's authentication and authorization for the SAP HANA XS advanced Java run time.

    For general instructions, see [SAP Authorization and Trust Management Service in the Cloud Foundry Environment](../60-security/sap-authorization-and-trust-management-service-in-the-cloud-foundry-environment-6373bb7.md).

4.  Create a security descriptor file in JSON format that specifies the functional authorization scopes for the application:

    -   Define the application provider tenant as a shared tenant:

        ```
        "tenant-mode":"shared"
        ```

    -   Provide access to the SAP SaaS Provisioning service SAP Authorization and Trust Management service \(technical name: `saas-registry`\) for calling callbacks and getting the dependencies API by granting scopes:

        ```
        "grant-as-authority-to-apps" : [ "$XSAPPNAME(application,sap-provisioning,tenant-onboarding)"]
        ```


    > ### Sample Code:  
    > Security descriptor file in JSON format:
    > 
    > ```
    > {  
    >    "xsappname":"saas-app",
    >    "tenant-mode":"shared",
    >    "scopes":[  
    >       {  
    >          "name":"$XSAPPNAME.Callback",
    >          "description":"With this scope set, the callbacks for subscribe, unsubscribe and getDependencies can be called.",
    >          "grant-as-authority-to-apps":[  
    >             "$XSAPPNAME(application,sap-provisioning,tenant-onboarding)"
    >          ]
    >       }
    >       ....
    >    ],
    >    ....
    > }
    > ```

5.  In the Cloud Foundry space in the provider's subaccount where your application is going to be deployed \(see [Deploy the Multitenant Application to the Provider Subaccount](deploy-the-multitenant-application-to-the-provider-subaccount-2204416.md)\), create an `xsuaa` service instance with the security configurations, which you defined in the security descriptor file in the previous step, by executing this command in the Cloud Foundry command line interface \(cf CLI\):

    ```
    cf create-service xsuaa application <XSUAA_INSTANCE_NAME> -c <XS_SECURITY_JSON>
    ```

    Specify the following parameters:


    <table>
    <tr>
    <th valign="top">

    Parameter
    
    </th>
    <th valign="top">

    Description
    
    </th>
    </tr>
    <tr>
    <td valign="top">
    
    `XSUAA_INSTANCE_NAME`
    
    </td>
    <td valign="top">
    
    The new name for the `xsuaa` service instance. Use only alphanumeric characters, hyphens, and underscores.
    
    </td>
    </tr>
    <tr>
    <td valign="top">
    
    `<XS_SECURITY_JSON>`
    
    </td>
    <td valign="top">
    
    Name of the security descriptor file from the previous step.
    
    </td>
    </tr>
    </table>
    
    > ### Sample Code:  
    > ```
    > cf create-service xsuaa application xsuaa-application -c xs-security.json
    > ```

    > ### Tip:  
    > You can also create the service instance directly in the cockpit.

    For general instructions, see [Create Spaces Using the Cloud Foundry Command Line Interface](../50-administration-and-ops/create-spaces-using-the-cloud-foundry-command-line-interface-a2e5e29.md).


