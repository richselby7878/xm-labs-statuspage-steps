# xMatters Labs Statuspage Steps
A collection of seven custom steps for use with xMatters Flow Designer. The steps are designed to make integrating with Atlassian Statuspage Incidents even easier. Although two of the steps (Statuspage Create/Update Incident with Components) cover the same ground as built-in xMatters Flow Designer steps, the steps in this workflow allow you to work with components and component-groups. There are also brand new steps to resolve, delete, get, list and search for Statuspage Incidents. 

The steps are packaged with a simple test interface, Statuspage Trigger Form, which lets you choose and try out each step with different input values. You can view the output either via Flow Designer's activity log or in an alert notification.

---------
<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics"><img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png"></a>
</kbd>

----------

# Pre-Requisites
* Access to xMatters Flow Designer
* xMatters account - If you don't have one, [get one for free](https://www.xmatters.com/free)!
* A Statuspage page to create and update Incidents. If you don't have one, you can sign up for an [Atlassian Statuspage Cloud Free account](https://www.atlassian.com/try/cloud/signup?bundle=statuspage&utm_source=pricing_free)

# Statuspage Authentication Info
* To integrate with Statuspage, you will need to know your **Page Id** and **API Key**. Once logged into your Statuspage mangement console, head to the top right circular logo, click it, then choose *API Info*. On this page you will find your *Page ID* and can generate your *Organization API Key*.  


# Files
* [xMattersLabsStatuspageSteps.zip](xMattersLabsStatuspageSteps.zip) - xMatters workflow containing 3 forms, 1 flow canvas and 8 custom flow steps. Even though we said 7, you get a free bonus step, Statuspage Make Shortname, used to format the flow.


# How it Works
The Statuspage API is used to work with Incidents in Statuspage. Here's an example of an Incident created via xMatters using the Statuspage Trigger Form interface in conjunction with the Create Incident with Components flow step.

The flow step, highlighted green in the Activity Log after being executed...
	<kbd>  <img src="/media/create_step" width="500"> </kbd>
	
	
The resulting Incident in Statuspage
	<kbd>  <img src="/media/sp_end_of_the_world" width="750"> </kbd>
	
Scrolling down the page to Components, we see The Doctor is an affected component, with a component status of Partial Outage.

	<kbd>  <img src="/media/sp_component" width="750"> </kbd>

When an Incident is created in Statuspage, it generates a Unique Id, also known as the Statuspage Incident Id. This Id is not visible on the page, but it is required for subsequent steps.

You can grab it from the flow Activity Log
	<kbd>  <img src="/media/activity_log_unique_id" width="500"> </kbd>

If you're using the Statuspage Trigger From to test, you can also get it from the alert email sent post-creation. It's also present in the Alerts Report.

	<kbd>  <img src="/media/email_unique_id" width="500"> </kbd>

With subsequent steps you can send updates, then resolve the Incident, so it moves to the Past Incidents section of your Statuspage.
	<kbd>  <img src="/media/past_incident" width="500"> </kbd>
	
	

# Custom Flow Steps
All flow steps have common outputs to allow for easy error-trapping within the flow. For example, the **Statusapge Create Incident With Components** step has a **created_boolean** output, which is set to "true" when the Incident is created, and "false" otherwise. This allows you to place a Switch step following the Create step and take action should the Create fail. The inverse of this is the **error** output, which can also be used. It's just a matter of style and preference. You can use one, both, or neither.  This contrasts with the built-in Statuspage Create/Update steps which throw an error on failure and stop the flow in its tracks.

 
The custom flow steps are...
*	Statuspage Create Incident with Components
  
	<kbd>  <img src="/media/step_create.png" width="200"> </kbd>

Create a new incident in Statuspage. Unlike the built-in Statuspage Create Incident step, this step allows you to optionally set affected components. You can choose either individual components or component-groups. For the latter, every component in the group is affected. N.B. Setting a component status applies to *all* components. Any unrecognised component names are silently ignored. Other inputs include Name, Status, Body (can be thought of as either first update or Incident Details) and Impact Override.  Step outputs include the Incident ID as assigned by Statuspage on creation. You should retain this ID to use in update/resolve/delete/get steps. 

*	Statuspage Update Incident with Components
  
	<kbd>  <img src="/media/step_update.png" width="200"> </kbd>
 
Update an incident in Statuspage, optionally with updates to affected components. To use this step, you must supply the Statuspage Unique Id, also referred to as the Statuspage Incident ID. Otherwise, step inputs and outputs are the same as the Create step. We recommened you use the Body field for a text update. N.B. If updating components are component-groups, only the named components are affected. Any prior components retain their original status.


*	Statuspage Resolve Incident with Components

	<kbd>  <img src="/media/step_resolve.png" width="200"> </kbd>
 
Resolve an Incident in Statuspage, automatically setting any impacted components to operational. However, you can choose another component status, e.g. degraded_performance,  if preferred. To use this step, you must supply the Statuspage Unique Id, aka Statuspage Incident ID. We recommened you use the Body field for a text update.

*	Statuspage Delete Incident

	<kbd>  <img src="/media/step_delete.png" width="200"> </kbd>
 
Deletes a Statuspage Incident using the Statuspage Unique Id. Delete as in remove it entirely, not change a status. 


*	Statuspage Get Incident From Keywords

	<kbd>  <img src="/media/step_get_kw.png" width="200"> </kbd>
 
Search for a Statuspage Incident using keywords. The keywords are Or'd together, so more than one Incident may be found. If more than one Incident is found, details from only the first Incident are returned. However the other Unique Ids are also supplied in the **Other Ids** output, should you want to dig further. 

*	Statuspage Get Incident From Id

	<kbd>  <img src="/media/step_get_id.png" width="2)0"> </kbd>
 
Search for a Statuspage Incident using the Unique Id, also known as the Statuspage Incident Id. By definition, this will only return a single Incident. However, it returns a greater selection of output properties than the other steps.

*	Statuspage Get Incident List

	<kbd>  <img src="/media/step_get_list.png" width="250"> </kbd>
 
Returns a list of Statuspage Incidents. Filter options available include all, unresolved, upcoming. These are passed on to the Statuspage API for processing. Max results returned by the API in one page is 100 Incidents. You can optionally set a page\_offset and a per\_page count. N.B. Sorting is supported, e.g. by updated\_at or created\_at, but sorting is applied AFTER the API has returned information. That means if you have more than 100 Incidents on your Statuspage, you may be sorting a partial result set.



# Installation
1. Download the [xMattersLabsStatuspageSteps.zip](xMattersLabsStatuspageSteps.zip) zip file from this repo. Click the name, then the ... icon from the top right, then select Download. Do not inflate the zip. It should be 44Kb in size. For some reason, if you choose Save Link As, the file successfully downloads and retains the .zip extension but inflates. It won't import into xMatters though.  Alternatively, you can download the file via the list of media at the top of this page. 
   
2. Log into xMatters as a user with either the Developer or Full Access User role. Navigate to Workflows, click the Import button on the top right and import the file. After a successful import, this is what you will see:

 <kbd>  <img src="/media/workflow_import_ok.png" width="850"> </kbd>
	
3. OPTIONAL. Navigate into the new workflow, either via the Open Workflow button after importing or from the workflow list. You will see a list of three forms under the FORMS tab. You need to set Sender Permissions on the forms. If you don't set Sender Permissions, only the user who imported the workflow can use/edit them. This may be too narrow. The <b>Statuspage Trigger Form</b> is the form-based test interface, you may want to let other users try it. Click the left-most of the two buttons to the right of the form (it says <b>Web UI</b>) and then <b>Sender Permissions</b> at the bottom of the pop-up. Add the users or roles who can use the Trigger form. We recommend allowing all those with Full Access User or Developer roles to use the form. If you prefer, you can name specifc users. Do the same for the other two forms, <b>Statuspage Response Good</b> and <b>Statuspage Response Bad</b>. The same users/roles should be selected. 

 <kbd>  <img src="/media/sender_permissions.png" width="350"> </kbd>
 
	
4. OPTIONAL. By default, only the importing user has permission to use and modify the custom steps within the workflow. We recommend broadening permissions to allow all users with the Developer role to be able to use and/or edit the new flow steps. To do this, shift to the FLOW DESIGNER tab to the right of FORMS. Open the <b>Statuspage Trigger Form</b> flow canvas. This is the flow for trying out the new steps.

	<kbd>  <img src="/media/sptf_canvas.png" width="750"> </kbd>
 
	
5. To the right of the screen on the Palette, highlight the CUSTOM tab. Type "statuspage" in the search bar to show just the new steps in this workflow. 

	<kbd>  <img src="/media/custom_step_tab.png" width="750"> </kbd>
	
	Navigate to each new flow step in turn. Click the gear icon then Usage Permissions. In the pop-up window, grant ACCESS to other users/roles as required, e.g. select the Developer role and grant permission to Use or Edit the step.
	 
	<kbd>  <img src="/media/step_usage_permissions.png" width="500"> </kbd>

6. Set up authentication constants. Navigate to the FLOW DESIGNER tab of the workflow. At the top right of the screen, click Components > Constants.  

	<kbd>  <img src="/media/components_constants.png" width="500"> </kbd>

  You need to set the correct API Key and Page ID constants to work with your Statuspage. Placeholder constants are there to be over-typed. See Statuspage Authentication section above for help on where to obtain these properties.
  
	<kbd>  <img src="/media/constants.png" width="500"> </kbd>

	  
  N.B. There is no need to set a new Endpoint. The standard Statuspage endpoint of https://api.statuspage.io will work.
 
# Testing
You can try out each new custom via the Statuspge Trigger Form. On the left hand nav, navigate to Messaging > xMatters Labs Statuspage Steps >  Statuspage Trigger Form. 
 
	<kbd>  <img src="/media/messaging_list.png" width="750"> </kbd>

Open the form and select a step in the <b>Action</b> dropdown. Each Action corresponds to a new custom step. Fill in the appropriate values in the lower sections, e.g. Incident Name and Body. 

<kbd>  <img src="/media/action_steps.png" width="750"> </kbd>
	
At the bottom of the form, you will see the Recipients section. You can optionally set yourself as a Recipient or proceed without one. When you click Send Message, an initial alert emial will be generated confirming the flow has been launched. A short while later, a second email alert will arrive with the step’s output values clearly shown. Alternatively, you can leave Recipient blank and view step output in the Statuspage Trigger Form flow activity log or even the Alerts report.

After sending the form, we recommend you click the Duplicate button (top right of the Alert Report page) to quickly commence your next test.


# Troubleshooting
Navigate to xMatters Labs Statuspage Steps  > Flow Designer tab > Statuspage Trigger Form canvas. You can see the canvas used to select each step based on an initial Switch block. Each custom step outputs log notes. If a step fails, you can click the Activity button (top right) to view the Activity Log. You can inspect step inputs, outputs and logs. 

<kbd>  <img src="/media/run_flow_step_activity_log.png" width="750"> </kbd>

# Disclaimer
As per all xMatters Labs projects, the flow steps here are provided on an as-is basis. Although every effort has been made to ensure they work as expected, we cannot guarantee they run in all situations going forward. The steps are not officially supported by Everbridge Support. 
