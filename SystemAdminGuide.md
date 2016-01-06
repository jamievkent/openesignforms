<table width='100%'><tr>
<td width='50%'><a href='http://open.esignforms.com'><img src='http://open.esignforms.com/images/OpenESignFormsLogo.gif' /></a></td>
<td width='50%' align='right'><a href='http://www.yozons.com'><img src='http://www.yozons.com/images/logoSmall.gif' /></a></td>
</tr></table>

<h1>System Administrator's Guide</h1>

#### How-To Guides ####
  * [Concepts, Tips & Tricks](HowToGuides.md) (Read first)
  * [User's Guide](UserGuide.md) (Read first)
  * **[System Administrator's Guide](SystemAdminGuide.md)** (this guide)
  * [Point & Click Programming Guide](ProgrammingGuide.md)
  * [Reports and Transaction Setup Guide](ReportsAndTransactionSetup.md)
  * [Point & Click Programming Integration API Guide](ProgrammingGuide_Integration_API.md)
  * [Point & Click Programming Tutorial](ProgrammingTutorial.md)

#### Table of contents ####


# Introduction #

Every deployment should have at least one user assigned to the **System/Administrator** group. The system administrator can delegate powers to other groups, including creating partner/department administrator groups who have similar capabilities, but over only a subset of all users.

The system administrator is responsible for various system-wide settings, creating new users, groups, permissions, resetting passwords and checking system activity logs.

See the [User Guide](HowToGuides#User_Guide.md) for common user activities, or the [Point & Click Programming Guide](HowToGuides#Point_&_Click_Programming_Guide.md) for programming documents, packages, transactions and reports.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Users and Groups Overview #

A key concept in Open eSignForms is that permissions are assigned to Groups, and Users are assigned to Groups. Therefore, a User has the sum of permissions for all groups he or she belongs in.

All deployments will have the **System/Administrator** group. It has access to all regular`*` users and groups. We recommend at least the following additional groups be created:
  1. A group for your company or department, such as MyCompany. You can put all users who belong into this group.
  1. A group for the programmers in your company or department, such as MyCompany/Programming. You can put just those folks, whether in your company or a hired professional services consultant, who will program documents and transactions in this group.
  1. A group for managers in your company or department who can see across all users in your company or department, such as MyCompany/Manager.
  1. If you will have multiple companies or departments using your deployment, you may consider a group for that plays the system administrator function, but limited to the company/department, such as MyCompany/Administrator.

(`*` There is also one special group called **ESF/Group/Deployment/SuperAdmin** that is created when the system is deployed. It is used to ensure access like "root" or "Administrator" provides for servers and PCs and is managed by whoever is responsible for the web application deployment.)

Watch our [How-To Video on YouTube: HTV Users, Groups and Permissions](http://www.youtube.com/watch?v=4Y7TnY8NMZ4&hd=1).

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Permissions Overview #

All records (also known as objects) in the system generally have four common permissions associated with them:
  1. **List** permission specifies the groups that are allowed to see the record in a list, such as a drop down list or menu.
  1. **View Details** permission specifies the groups that are allowed to see the details of a record, such as all of the fields used to configure it.
  1. **Create/Create Like** permission specifies the groups that are allowed to create a new record (or object) like the existing record.
  1. **Update** permission specifies the groups that are allowed to update or modify the record.
  1. **Delete** permission specifies the groups that are allowed to delete the record.

For User Interface (screens/views) components, the permissions generally describe whether a 'Create like' or 'Save' or 'Delete' button will even appear on that screen, and 'List' means whether that item will even appear on the navigation menu list on the left side.

The Group object itself is special in that it also has a similar set of List, View, Create, Update and Delete permissions that are directed at the Users who belong to the group. An administrator for a group of users needs to have these permissions to manage the users in the group, but may only need List permission on the group itself.

Watch our [How-To Video on YouTube: HTV Users, Groups and Permissions](http://www.youtube.com/watch?v=4Y7TnY8NMZ4&hd=1).

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Configuring Permissions ##

If you have View Details and Update permission for a given record, you can change the groups who have permission to manipulate that record, too. For example, for a given Group record's **List** permission:

![http://open.esignforms.com/images/HowToGuidesWiki/groupListPermission.png](http://open.esignforms.com/images/HowToGuidesWiki/groupListPermission.png)

Select one or more groups from the left column (available groups you can choose from -- because you have **List** permission to those groups), and click the **>>** button to move them to the right side, which is the list of Groups that will have the specified permission.

Of course, you will have to click the **Save** button after your have made your changes.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Users ![http://open.esignforms.com/images/HowToGuidesWiki/usersIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/usersIcon.png) #

Click on **Access control->Users** to list and update user records.

Each user who can start transactions, run reports, or process work via the To Do queue needs a user account. While you can share user accounts, we recommend you give each user his or her own login and password to aid in tracking user activity and to make it easier to handle password issues should one of the users no longer need access to the system.

Configuring a user involves setting up the user's email address, name and what groups he or she belongs to:

![http://open.esignforms.com/images/HowToGuidesWiki/userEdit.png](http://open.esignforms.com/images/HowToGuidesWiki/userEdit.png)
  * Select **Enabled** to allow this user to login. Choose **Disabled** to block user logins. We recommend disabling user accounts, rather than deleting them, to keep any associations that point back to the user.
  * **Time zone** specifies the user's preferred time zone for dates and times shown in forms, logs, reports, etc.
  * **Email address** must uniquely identify this user, and it should be a valid, working email address in order for the user to set a password, get email notifications, etc.
  * **Email display name** is the name shown that goes with the email address to create a "fully qualified" email address. Most people use their full name or role, such as "Support" or "David Jones", which results in an email address of "Support <support@example.com>" or "David Jones <djones@example.com>".
  * **First name** is the user's first, or personal, name.
  * **Last name** is the user's last, or family, name.

The following additional fields are generally for information only and can be accessed in documents and custom logic using field specifiers.
  * **Phone number** is the user's contact phone number.
  * **Job title** is the user's job title.
  * **Employee ID** is the user employee ID or related ID.
  * **Location/region** is the user's location or region.
  * **Department/division/workgroup** is the user's department.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Member of group(s) ##

To list the groups the user belongs to, open the **Member of groups(s)** panel:
![http://open.esignforms.com/images/HowToGuidesWiki/userMemberOfGroups.png](http://open.esignforms.com/images/HowToGuidesWiki/userMemberOfGroups.png)

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Update member of group(s) ##

To update the groups the user belongs to, open the **Update member of groups(s)** panel:
![http://open.esignforms.com/images/HowToGuidesWiki/userMemberOfGroupsUpdate.png](http://open.esignforms.com/images/HowToGuidesWiki/userMemberOfGroupsUpdate.png)

Select one or more groups from the left side and click the **>>** button to add the user to those groups.  Select one or more groups from the right side and click the **<<** button to remove the user from those groups.

_Remember to click **Save** after changing the group membership._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## View user activity log ##

Click on the **View activity log** button to display activity associated with the user:

![http://open.esignforms.com/images/HowToGuidesWiki/userActivityLog.png](http://open.esignforms.com/images/HowToGuidesWiki/userActivityLog.png)

  * Enter a **From date** to start from a specific date (or select the date from the calendar popup). Leave blank to include the oldest records.
  * Enter a **To date** to specify and ending date (or select the date from the calendar popup). Leave blank to include the newest records.
  * Check **Login** to include user logins and logoffs.
  * Check **Security** to include user security-related events.
  * Check **Config** to include user updates, like adding, changing or deleting records.
  * Enter some text in **Log record contains** field to search for records that contain the specified string.

Click the **Find matching** button to list all matching records.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## View user email ##

Click on the **View email** button to display emails sent to or on behalf of the user:

![http://open.esignforms.com/images/HowToGuidesWiki/userEmails.png](http://open.esignforms.com/images/HowToGuidesWiki/userEmails.png)

  * Enter a **From date** to start from a specific date (or select the date from the calendar popup). Leave blank to include the oldest records.
  * Enter a **To date** to specify and ending date (or select the date from the calendar popup). Leave blank to include the newest records.

Click the **Find matching** button to list all matching records.

Emails that have an arrow next to them have a correlated reply or bounce. Click on the arrow to view those emails.

Click on any row to view the email text:

![http://open.esignforms.com/images/HowToGuidesWiki/userEmailDetails.png](http://open.esignforms.com/images/HowToGuidesWiki/userEmailDetails.png)

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Send RESET password email ##
Click the **Send RESET password email** to reset a user's password so that the user cannot login until he or she retrieves the email, clicks on the unique link it contains, and then chooses a password.

_Note that the user cannot login once you click this button until they set a new password. The original password will no longer be valid once the button is clicked._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Create a new user account ##

Click on **Access control->Users**:

  1. Select an existing user that is most like the new user you want to add.
  1. Click the **Create like** button to create a new user record.
  1. Ensure the record id **Enabled**.
  1. Change the **Email address** to the new user's email address.
  1. Change the **Email display name**, **First name** and **Last name**.
  1. Click **Member of Groups** to open the panel that shows the groups. Adjust as necessary.
  1. Click the **Save** button to save the new user.
  1. Once you are ready to allow the user to login, click the **Send RESET password email** button.

An email will be sent to the user's email address so he or she can choose a password and set the forgotten password question & answer.

If the user does not receive the email in after a normal amount of time, you can click the **View email** button to see if there was a bounce, such as if you had the email address incorrect or there was some other error.

Watch our [How-To Video on YouTube: HTV Password Reset & Add New Users](http://www.youtube.com/watch?v=VnJGjbEwIfI&hd=1).

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Reset a user's password ##
While every user can reset his or her own password using the **forgotten password** feature linked on the login page, it is possible the user has forgotten the answer to that question as well.

Click on **Access control->Users**:
  1. Select the user who has forgotten his password.
  1. Ensure the user is not **Disabled** as this could have been the reason why he or she could not login.
  1. Click the **Send RESET password email**.

An email will be sent to the user's email address so he or she can choose a new password and, you hope, set a better forgotten password question & answer.

If the user does not receive the email in after a normal amount of time, you can click the **View email** button to see if there was a bounce, such as if the email address was no longer valid or there was some other error.

Watch our [How-To Video on YouTube: HTV Password Reset & Add New Users](http://www.youtube.com/watch?v=VnJGjbEwIfI&hd=1).

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Groups ![http://open.esignforms.com/images/HowToGuidesWiki/groupsIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/groupsIcon.png) #

Click on **Access control->Groups** to list and update group records.

All permissions are applied to groups, which is just a simple name used to identify one or more users who perform similar jobs or work together. A user can belong to one or more groups, and it's possible to create multiple groups to support multiple companies/partners who are sharing a single system so that each such company only sees users and groups that belong to their company, while allowing the owner of the deployment to view all users and all transactions across those companies.

Configuring a group involves giving it a name and specifying which users belong to the group:

![http://open.esignforms.com/images/HowToGuidesWiki/groupEdit.png](http://open.esignforms.com/images/HowToGuidesWiki/groupEdit.png)

We recommend using a simple **PathName** that describes your group. However, if you support multiple companies or departments, you can prefix them to keep them organized, such as Company1 (for all users who work at Company1), Company1/Admin (for the users who can do the user administration functions for Company1), Company1/Manager (for the managers in Company1 that can see across all users), and Company1/Programming (for those who program the forms for Company1).

You are free to name as you see fit, of course, but since you will use them for assigning permissions, it's best if they make clear sense to you. If you have multiple companies with multiple departments in each, and you want each department to have control or limited access to just that department, consider using a scheme like Company1/Department1, Company1/Department1/Admin, Company1/Department1/Manager, etc., so you can then have Company1/Department2, Company1/Department2/Admin, Company1/Department2/Manager, etc.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Permissions to access this group ##

See [Overview the permissions overview](#Permissions.md) for this basic setup of group permissions:

![http://open.esignforms.com/images/HowToGuidesWiki/groupPermissions.png](http://open.esignforms.com/images/HowToGuidesWiki/groupPermissions.png)

This list specifies who can perform various operations related to this group record alone.

_Remember to click **Save** after changing permissions._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Member users of this group ##

To set which users are members of this group, click to open:

![http://open.esignforms.com/images/HowToGuidesWiki/groupMembers.png](http://open.esignforms.com/images/HowToGuidesWiki/groupMembers.png)

You can click one or more users from the left side and then click the **>>** button to add users to the group.  Or click one or more users from the right side and then click the **<<** button to remove users from the group.

You can also set which groups a given [user](#Users.md) belongs to by modifying that user's record.

_Remember to click **Save** after changing the group membership._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Permissions to access the member users of this group ##

The permissions for accessing the records of the [users](#Users.md) who belong to a group are set here:

![http://open.esignforms.com/images/HowToGuidesWiki/groupMemberPermissions.png](http://open.esignforms.com/images/HowToGuidesWiki/groupMemberPermissions.png)

Like the standard permissions, you can specify who can List, View Details, Create, Update and Delete users who belong to this group. Normally, this permission is left to the **System/Administrator** or if the group administrator, such as Company1/Admin if you build such a group.

In addition, there's a special Manage To Do permission that allows members of a group to access the To Do screen of members of the selected group. Normally, other users do not need access to the To Do list of other users, but managers may want it to unlock transactions queued up to a user based on the To Do Group setting for a party, when that user has left the company, is on vacation, etc.

_Remember to click **Save** after changing permissions._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Show members ##

If you click on the **Show members** button, a popup window that lists all users appears:

![http://open.esignforms.com/images/HowToGuidesWiki/groupMembersList.png](http://open.esignforms.com/images/HowToGuidesWiki/groupMembersList.png)

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Who's On Now ![http://open.esignforms.com/images/HowToGuidesWiki/whosOnNowIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/whosOnNowIcon.png) #

Click on **Access control->Who's on now** to display all active user sessions:

![http://open.esignforms.com/images/HowToGuidesWiki/userSessions.png](http://open.esignforms.com/images/HowToGuidesWiki/userSessions.png)

  * For logged in users, the **Name** column is the user's login name. For external users, it is the transaction and role they are playing if that has been determined.
  * The **IP** column shows the IP address (and host name when it can be determined by a reverse DNS lookup).
  * The **Last** column shows the date-time when the user last accessed the server.
  * The **Started** column shows the date-time when the user first accessed the server.
  * The **Timeout** column shows the number of minutes the user's session will remain active if no user activity is taking place. Generally speaking, if the **Last** column is more than **Timeout** minutes from now, that user's session will timeout and the user will need to restart.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# UI Views ![http://open.esignforms.com/images/HowToGuidesWiki/uiViewsIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/uiViewsIcon.png) #

Click on **Access control->UI Views** to list all of the views (main pages/screens) that are accessible from the left side panel menu navigation.  "UI" stands for "user interface."

![http://open.esignforms.com/images/HowToGuidesWiki/uiViews.png](http://open.esignforms.com/images/HowToGuidesWiki/uiViews.png)

We do not recommend changing the **View EsfName** of any views as it will break their functionality unless other programmer-level changes are made.

We do not recommend changing permissions here much as it is generally best to set them once for all users, since users will naturally be limited to the users, groups, libraries and transactions that have been granted to them based on their permissions.  The UI permissions are mostly to determine whether a given item appears in the menu navigation or not.

  * DeploymentPropertiesView controls **System config->Deployment**
  * DeploymentStatsView controls **System config->Stats**
  * GroupView controls **Access control->Groups**
  * LibraryDocumentProgrammingTipsEditView controls **System config->Document program tips**
  * LibraryProgrammingTipsEditView controls **System config->Program tips**
  * LibraryView controls **Programming->Libraries**
  * PackageAndVersionsMainView controls **Programming->Packages**
  * ReportTemplateView controls **Programming->Report templates**
  * SystemLogView controls **System config->System logs**
  * ToDoListingView controls **To Do**
  * TransactionCsvStartView controls **Start transaction->CVS Upload**
  * TransactionListingView controls **General tran search**
  * TransactionTemplateView controls **Programming->Tran templates**
  * UIView controls **Access control->UI Views**
  * UserChangePasswordView controls **Access control->Change my password**
  * UserView controls **Access control->Users**
  * UserSessionView control **Access control->Who's on now?**.  'Delete' permission allows the user to end any user session listed.
  * AccessControlTipsEditView controls **System config->Access control link**
  * AccessControlTipsView controls **Access control**
  * ProgrammingTipsEditView controls **System config->Program link**
  * ProgrammingTipsView controls **Programming**
  * ReportTipsEditView controls **System config->Report link tips**
  * ReportTipsView controls **Reports**
  * StartTransactionTipsEditView controls **System config->Start tran tips**
  * StartTransactionTipsView controls **Start transaction**
  * SystemConfigTipsEditView controls **System config->System config link**
  * SystemConfigTipsView controls **System config**
  * WelcomeTipsEditView controls **System config->Welcome link**
  * WelcomeTipsView**controls**Welcome

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

## Permissions to Access the XXXX screen ##

![http://open.esignforms.com/images/HowToGuidesWiki/uiViewsPermissions.png](http://open.esignforms.com/images/HowToGuidesWiki/uiViewsPermissions.png)

Permissions work the same here as for other permissions, but it doesn't affect the ability to change the UI record itself, but how the user interface works.
  * **List** permission means the user will see this UI in the menus.
  * **View details** means the user will see this UI in the menus.
  * **Create** means the user will be allowed a **Create** or **Create like** button.
  * **Update** means the user will be allowed a **Save** button.
  * **Delete** means the user will be allowed a **Delete** button.

_Note that having the permission here does not mean the button will always appear, since for records that have their own permissions, the user will need it on the specific object as well. If you remove **Delete** permissions for the UI, for example, that button will not appear even if the user's group allows them to otherwise delete that record._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Deployment configuration ![http://open.esignforms.com/images/HowToGuidesWiki/deploymentPropertiesIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/deploymentPropertiesIcon.png) #

Under **System config->Deployment** you can view and modify certain deployment properties. Please do so with care. Click the **Edit** button to change values, but be certain you know what you are doing.

![http://open.esignforms.com/images/HowToGuidesWiki/deploymentProperties.png](http://open.esignforms.com/images/HowToGuidesWiki/deploymentProperties.png)

  * Every deployment has a **Unique deployment ID**, which is displayed at the top.
  * Note that the **ID** field is the related to the deployment properties record id as stored in the database.
  * Ensure **Allow Production transactions** is checked if your deployment can run production transactions.
  * Ensure **Allow Test transactions** is checked if your deployment can run test transactions.
  * The **Website URL Protocol+Address** is the domain name where your application runs and is used in all links sent out. Most deployments use the 'https://' protocol to ensure secure communications between the browser and the server.
  * The **Default country** is used to specify what country the deployment is for. It is used, for example, to aid in phone numbers being entered and interpreted correctly.
  * The **Default locale** specifies the language the application runs as. Currently, we only support US English.
  * The **Default date format** specifies how dates are displayed by default. There is a known issue as of the 14.6.7 release if you use the DateComparison condition in an action and your date format is not one of the top 3 options. We recommend you only use the top 3 for your date formats (15-Feb-2011, 02/15/2011 or 2011-02-15) as these can be correctly transformed back into date objects.
  * The **Default log/detailed date format** specifies how dates are displayed in various logs and other places where full dates are displayed.
  * The **Default time zone** specifies the default time zone used. This can be overridden on a per-user basis.
  * The **Default time format** specifies how times are displayed by default. Most people only show times through minutes.
  * The **Default log/detailed time format** specifies how times are displayed in various logs. For logs, you probably want to display seconds, if not milliseconds.
  * The **SMTP Server** is the IP address or host name of the SMTP server to use for sending outbound emails. Most will keep this set to 'localhost.localdomain' to use the system's SMTP server.
  * The **SMTP Port** is the TCP port number SMTP is listening on. The default is 25. For some remote SMTP servers, you may have a value like 587 or perhaps 465 if you use SSL.
  * The **SMTP Auth User** is set if your SMTP server requires AUTH protocol to send.
  * The **SMTP Auth Password** is set if your SMTP server requires AUTH protocol to send.
  * Check the **Use TLS** box if your SMTP server communicates over TLS.
  * The **IMAP Server** is the IP address or host name of the IMAP server to use for retrieving inbound emails (mostly bounces and replies to automatically created emails sent by the application). Most will keep this set to 'localhost.localdomain' to use the system's IMAP server.
  * The **IMAP Port** is the TCP port number IMAP is listening on. The default is 143.
  * The **IMAP User** is generally set to your unique database name set for your deployment. It must be a valid user account on the server which is only used to accept inbound email for your application. On a secure server, we do not recommend allowing actual user logins to this account (we set it up so it's not valid to login to this account via SSH, for example), and keep it in its own group.
  * The **IMAP Password** is the password that goes with the IMAP User login.
  * Check the **Use SSL** box if your IMAP server communicates over SSL.
  * The **Default Email FROM** is used as the FROM email address for emails not sent out on behalf of a user. We recommend the No-Reply@yourhost.com form as automated emails generally do not expect replies. Note, however, that many emails sent out will correlate a reply back to it if you view the emails through the application.
  * The **SMTP Return Path Hostname** is the hostname where emails sent out will show. For anti-spamming measures, it should generally be the actual host name where your application is running and where the IMAP server will be able to process bounces.
  * The **Start year** is the year your deployment was installed.
  * The **Installation date** is the date your deployment was installed.
  * The **Context path** is the URL path name appended to the 'Website URL Protocol+Address' to indicate your web application instance.
  * The **License** specifies whether your system is running under the open source AGPL license, in which case you must provide source code for the application as well as all programming of documents you create, or if you have a commercial license based on number of users or database size.
  * The **License size** is 0 for open source users, or specifies the maximum number of users or database size (in MB) of your license. Note that exceeding the maximum will not affect the system operation, but additional licensing fees will apply on renewal.
  * The **Default session timeout** specifies the default web application session timeout in minutes. Most use 30 minutes. It should be long enough for a typical party to process your electronic documents. If a party is processing a document and does not interact with the server in this many minutes, the session will be terminated and the party will have to restart access.
  * The **User session timeout** specifies the web application session timeout in minutes for users who have logged in. This is often used to ensure that users who login have a shorter timeout than parties who are invited to sign your documents.
  * The **Retain stats logs** specifies how many days to keep usage statistics.
  * The **Retain system start/stop logs** specifies how many days to keep system activity logs related to system starts and stops. Most will keep it for 365 days.
  * The **System config changes** specifies how many days to keep system activity logs related to configuration changes, like creating, updating or deleting documents, packages and transactions across all users. Most will keep it for 90 days.
  * The **System user activity** specifies how many days to keep system activity logs related to users logging in and out.  Most will keep it for 90 days.
  * The **Retain user security logs** specifies how many days to keep security-related log records for each user who can login.
  * The **User config** specifies how many days to keep log records regarding configuration changes, like creating, updating and deleting documents, packages and transactions, for each user.
  * The **User login/logoff** specifies how many days to keep log records regarding logins and logoffs for each user.

_Remember to click **Save** after changing any values._

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Deployment Statistics ![http://open.esignforms.com/images/HowToGuidesWiki/deploymentStatsIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/deploymentStatsIcon.png) #

Under **System config->Stats** you can view statistics related to your system activity and versions.

![http://open.esignforms.com/images/HowToGuidesWiki/deploymentStats.png](http://open.esignforms.com/images/HowToGuidesWiki/deploymentStats.png)

Under the **System** column:
  * **Version** is the Open eSignForms application version. This is also displayed at the bottom right of the application window and you can click on the version to see the version history. ![http://open.esignforms.com/images/HowToGuidesWiki/copyrightAndVersion.png](http://open.esignforms.com/images/HowToGuidesWiki/copyrightAndVersion.png)
  * **System started** is the date+time when the application was last started.
  * **Java version** is the [Java](http://java.sun.com) version used to run your application.
  * **JVM Memory** is the amount of memory assigned to Java, as well as the maximum specified it can use. If the amount used reaches the maximum, garbage collection takes place to try to reduce the amount of memory in use.  This is for all web applications.
  * **Total threads** is the number of threads Java is using. This is for all web applications.

Under the **Web server** column:
  * **Version** is the web server version, which by default is [Apache Tomcat](http://tomcat.apache.org/).
  * **Vaadin version** is the version of [Vaadin](http://www.vaadin.com) used, which provides the user interface code for the Web 2.0 look.
  * **HTTP threads** specifies the number of HTTP (non-secure) access threads are in the web server (and how many are inactive). This is for all web applications.
  * **HTTPS threads** specifies the number of HTTPS (secure) access threads are in the web server (and how many are inactive). This is for all web applications.
  * **Backgrounder thread** should say 'Running' to indicate that the background task processor is running.
  * **Email out thread** should say 'Running' to indicate that the outbound email processor is running.
  * **Email in thread** should say 'Running' to indicate the inbound email processor is running.
  * **HTTP Send thread** should say 'Running' to indicate the HTTP(S) GET/POST processor is running.

Under the **Database** column:
  * **Version** is the database version, which by default is [PostgreSQL](http://www.postgresql.org).
  * **Max connections** specifies the maximum number of database connections in use out of the maximum allowed.
  * **Connections** specifies the number of database connections currently in use, the number available for use, as well as the "idle" timeout for connections if they are not used.
  * **Number of transactions** specifies the number of transactions in your database.
  * **Number of users** specifies the number of user accounts in your system. If you have a commercial users license, this is important to compare with your license size.
  * **Database size** specifies the size of your database. If you have a commercial DB license, this is important to compare with your license size.
  * **Backups/logs size** this is the additional space used to store database backups and application log files.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# System logs ![http://open.esignforms.com/images/HowToGuidesWiki/systemLogsIcon.png](http://open.esignforms.com/images/HowToGuidesWiki/systemLogsIcon.png) #

Under **System config->System logs**, you can view activities that take place on the system that are logged. This allows for trouble shooting as well as assigning blame (yes, we know _you_ did it)! See the [Deployment configuration](#Deployment_configuration.md) for how long these log records are kept.

![http://open.esignforms.com/images/HowToGuidesWiki/systemLogs.png](http://open.esignforms.com/images/HowToGuidesWiki/systemLogs.png)

Specify the start date (or leave blank for the oldest records) and end date (or leave blank for the most recent records) and check the various types of records you want to see. If you are looking for something in particular, you can enter a value in the **Log record contains** field.  Click **Find matching** to list all matching system activity log records.

Record types:
  * **Version** records indicate new version installations and updates. Note that only version changes that require 'DbSetup' log such changes here.
  * **Start/stop** records are recorded whenever your application starts and stops.
  * **User** records show when any user logs in or out, or has an invalid password.
  * **Config** records show when any user makes a configuration change to documents, packages, transactions, reports, etc.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Template library #

Under Programming->Libraries->ESF/Library/Template are the system configurations for you deployment. We will discuss a few of the key elements that you may want to tweak for you deployment.

## Documents ##

The system standard package listing and E-Sign Disclosures are defined in the document **StandardPackageDisclosures**. We do not recommend changing this document, but you may find that you want to export it and import into your own library so you can change the layout and wording of the standard package listing and disclosures.

## Button message sets ##

The system standard buttons and messages are defined in the button message set **ESF\_DefaultButtonMessage**. We do not recommend changing this button set, but you may find that you want to export it and import into your own library so you change the button labels and messages.

## Document styles ##

The system standard styles are defined in the document style **ESF\_DefaultDocumentStyle**. We do not recommend changing this document style, but you may find that you want to export it and import into your own library so you change the default styles for your documents.

## Drop downs ##

In general, the drop downs here will need to be changed if you need values that are not currently supported. Of course, you should take care to ensure your values are correct.

## Emails ##

The system sends a few emails for its own needs and they are defined here, as well as the standard pickup notification.

**DefaultPickupNotification** contains the default email notification that is sent out for a party to process your documents. We do not recommend changing this email, but you may find that you want to export it and import into your own library so you change the default emails for your transactions.

**ForgotPassword** is sent when a user requests that their password be reset automatically.

**PasswordChanged** is sent to the user's email address any time the password is changed.

**PasswordLockout** is sent to the user's email address whenever they are locked out because of three login failures.

**SetPassword** is sent to the user's email to have them set a new password for their account.

## Images/Logos ##

The file **Logo** is the default logo used for your deployment, such as on the login page.  The various other images allow you to change other icons used in documents, error messages, etc.

## Property sets ##

The **ESF** property set has a few properties you may want to tweak.

The **LoginPageAllowEmbedded** can be set to **true** if you plan to embed the login page in another web page. We do not recommend this for security reasons (the domain name, SSL cert, etc. are hidden and enable phishing attacks and hacks that occur on the web page where it is embedded. This basically removes the javascript that forces the login page to be to the top level page.

The **LoginPageTitle** sets the web page title for the login page.

The **LoginWelcomeHeader** sets the main welcome message that appears on the login page.

The **SendEmailFrom** contains the default FROM email address to be used when sending out emails from this deployment.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# XML Digital Signatures #

Open eSignForms uses industry standard XML Digital Signatures to create reliable records of every step of a transaction. Our digital signatures use the most powerful digital signatures on the marketplace today with 4096-bit RSA keys over SHA-512 hashes.

As each party completes a given document, the HTML sent to the party's web browser is captured and digitally signed along with an accurate timestamp providing an electronic record that cannot be subsequently changed without detection.

## Snapshot XML ##

You can see the snapshot XML from the details page of a given transaction, whether from [General Tran Search](UserGuide#General_tran_search.md), or from one of your [Reports](UserGuide#Report_transaction_details.md). Just click on the **Download Snapshot XML** option for a given document and party:

![http://open.esignforms.com/images/HowToGuidesWiki/downloadSnapshotXML.png](http://open.esignforms.com/images/HowToGuidesWiki/downloadSnapshotXML.png)

The downloaded XML file contains the digital signature that can be re-verified independently to prove it's the original. It includes both the data in the document as well as the HTML the user saw when completing the document.

The downloaded snapshot XML file contains the public key that is uniquely associated with each deployment of Open eSignForms. Here's an example showing the key value (the RSA public key used) as well as the signature seal:

![http://open.esignforms.com/images/HowToGuidesWiki/xmlDigitalSignatureKeys.png](http://open.esignforms.com/images/HowToGuidesWiki/xmlDigitalSignatureKeys.png)

## How the world knows its your public key ##

While anybody can revalidate the XML digital signature using any compliant XML digital signature verifier (we provide an example in the application code file `com.esignforms.open.crypto.XmlDigitalSignatureSimpleVerifier`), it is not obvious that the signing keys belong to you.

If there is any question about whether the public key and other information in the snapshot is valid and belongs to your deployment, anybody can compare them against your deployment's public keys where values like the KeyName, Modulus, Exponent and DeploymentId can be confirmed visually.

Just visit the showSigningKeys.jsp page for your deployment (i.e. `https://open.esignforms.com/demo/showSigningKeys.jsp` for our demo system) or whatever deployment you are checking. It will list all of the public keys used as well information like the key name and deployment id. The <font color='blue'>blue values</font> in particular should all match between this listing and the snapshot XML's values.

![http://open.esignforms.com/images/HowToGuidesWiki/showSigningKeys.png](http://open.esignforms.com/images/HowToGuidesWiki/showSigningKeys.png)

The web page lists the deployment id, KeyName and the XML Digital Signature style RSAKeyValue Exponent and Modulus. All validly signed documents created using your system will match one of your keys.

Furthermore, the self-sign X.509 Certificate, including the Subject Distinguished Name (DN) is shown to prove your deployment credentials. This is mostly used for PDF digital signature validations.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>

# Download transaction archives #

## Export transactions for long-term storage ##

Most will want to keep production transaction indefinitely to allow for easy online access via the reports, knowing that the data and documents are securely encrypted on disk at all times. However, as both a backup and a means to shorten the retention period of transactions to keep a database small, we recommend that you periodically download a transaction archive of completed or important transactions.

Simply create a [Report](ProgrammingGuide#Report_Templates.md) against transactions types you want to export. Most can use existing reports that are used on a daily basis to view the status of all transactions.  Just give the arhiving person, typically the system administrator, permission to run the report, see all transactions across users, download logs and snapshots, and of course the "download complete transaction archives" permission.

Then run that report to list matching transactions you'd like to export, and click the **Download archive** button.  A ZIP file will be returned that contains the matching transactions. Assuming the user has full permissions specified in the report, this will include XML data for the transactions and documents, the activity log, the email sent log, attached files, and digitally signed snapshots.

Just unzip the downloaded file to access your transactions from your computer.

Tip: If you do expire transactions (set the retention to any value besides 'forver'), the report allows you to specify a data range along with the 'Will expire' option so you can download all transactions, for example, that will expire in the next month (or next quarter, next year, etc.), so you can do this on a periodic basis to keep a copy yourself even if the service will no longer have a copy online.

<p align='right'><font size='1'><a href='#Table_of_contents.md'>Table of contents</a></font></p>