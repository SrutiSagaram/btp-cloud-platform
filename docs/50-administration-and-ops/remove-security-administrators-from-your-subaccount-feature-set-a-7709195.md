<!-- loio7709195cab8d4807be08b0b5167e1940 -->

<link rel="stylesheet" type="text/css" href="../css/sap-icons.css"/>

# Remove Security Administrators from Your Subaccount Feature Set A

Running on the cloud management tools feature set A: You can remove security administrators from your subaccount by deleting them.



<a name="loio7709195cab8d4807be08b0b5167e1940__prereq_q24_by4_4cb"/>

## Prerequisites

-   You can remove users as security administrators: You either created this subaccount, or an authorized user has assigned the `User & Role Administrator` role to your user for it.

-   Your user and these users must have at least one of the following member roles \(see the related link\):

    -   They are members of the Cloud Foundry organization \(if available\) in the subaccount.

    -   They are members of the Cloud Foundry spaces that belong to the organization.

    -   They are members of the global account that contains your subaccount.





<a name="loio7709195cab8d4807be08b0b5167e1940__context_qsr_ry5_4cb"/>

## Context

To remove security administrators from your subaccount, take the following steps.



<a name="loio7709195cab8d4807be08b0b5167e1940__steps_rsr_ry5_4cb"/>

## Procedure

1.  Open the SAP BTP cockpit.

2.  Go to your subaccount\(see [Navigate to Orgs and Spaces](navigate-to-orgs-and-spaces-5bf8735.md)\). Since your user is the security administrator of your subaccount, you see *Security* in the navigation area.

3.  Choose *Security* \> *Administrators*.

    You see a list of the security adminstrators with their respective roles.

4.  To remove a security administrator from your subaccount, choose :wastebasket:.


**Related Information**  


[Add Org Members Using the Cockpit](add-org-members-using-the-cockpit-a4eeaf1.md "Add users as org members and assign roles to grant the users access to information, such as user and quota information in a Cloud Foundry org.")

 <?sap-ot O2O class="- topic/link " href="30eaa74f50154ebbb8d41439bc814f31.xml" text="" desc="" xtrc="link:2" xtrf="file:/home/builder/src/dita-all/jjq1673438782153/loio2080d0faf9d84ce6aa14caa4caa32935_en-US/src/content/localization/en-us/7709195cab8d4807be08b0b5167e1940.xml" ?> 

[User and Member Management](../10-concepts/user-and-member-management-cc1c676.md "On SAP BTP, member management happens at all levels from global account to environment, while user management is done for business applications.")

