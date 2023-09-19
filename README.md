# Create CRM Tasks and Standalone Functions



## How it Works
Standalones are great to use when you want to reuse code and call it from different functions. Keeps code a lot cleaner.


## Set Up Standalone

Create the standalone function that you want to call

<img src="Image 9-19-23 at 4.50 AM.jpeg" width="600">

Add the parameter and make note of the function name with underscores. Add the task code (leads snippet below)

<img src="Image 9-19-23 at 5.32 AM.jpeg" width="600">

```
lead_record = zoho.crm.getRecordById("Leads",lead_id);
reminder_date_time = zoho.currenttime.addDay(1).toTime();
//info reminderTime;
lead_map = Map();
lead_map.put("Subject","Lead Follow up");
lead_map.put("$se_module","Leads");
lead_map.put("What_Id",input.lead_id);
lead_map.put("Owner",lead_record.get("Owner").get("id"));
lead_map.put("Due_Date",zoho.currenttime.toDate().addDay(2));
lead_map.put("Remind_At",{"ALARM":"FREQ=NONE;ACTION=EMAIL;TRIGGER=DATE-TIME:" + reminder_date_time.toString("yyyy-MM-dd'T'HH:mm:ss'+05:30'")});
lead_map.put("Status","Not Started");
lead_map.put("Send_Notification_Email",true);
createTask = zoho.crm.createRecord("Tasks",lead_map);
info lead_map;
info createTask;


return createTask;

```


Store the list you want to use in a list variable, then iterate through the list with a for each loop, getting the pieces of information you need for each record. In this example, I want to get the first and last name, email and phone number. I have omitted the null and empty checks for brevity, but you may want to add these in case you do not have complete data coming from your CRM. Concatenate the Deluge variables with the HTML/CSS syntax, using opening and closing quotations as needed. 

```
...
	//iterate through all contacts for a particular account record
	getRelatedContacts = zoho.crm.getRelatedRecords("Contacts","Accounts",input.Account_ID);
	for each contact in getRelatedContacts
	{
		getRecord = zoho.crm.getRecordById("Contacts",contact.get("id").toLong());
		firstName = getRecord.get("First_Name");
		lastName = getRecord.get("Last_Name");
		phone = getRecord.get("Phone");
		email = getRecordc.get("Email");
		//
		//adds the current iteration's contact name, phone and email to the table row. Append the 'x' variable so it's storing the contact info each time. Clicking the email will open your email client. 
		//
		x = x + "<tr><td>" + firstName + " " + lastName + "</td><td>" + phone + "</td><td><a href='mailto:" + email + "'>" + email + "</a></td></tr>";
	}
...

```

Append the closing table tags to the 'x' variable, then set the Note field to equal 'x'.

```
...
//close the table tags
x = x + "</tbody></table>";
//set the blank note to equal x
input.YOUR_NOTE_VARIABLE= x;
}
//end of if statement Account_ID == null
```
