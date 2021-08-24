# sample-processes

## Excel Automation using Robusta

<div align="center">
      <a href="https://www.youtube.com/watch?v=gFAfMnxQr84">
     <img 
      src="https://user-images.githubusercontent.com/87966919/130401926-17ccf307-3942-414b-bef5-e49bcf641781.jpg" 
      alt="Robusta" 
      style="width:100%;">
      </a>
    </div>

      Hello there.

      In this video, we will show you how to design a sample process thatincludes Excel operations with Robusta RPA, step by step. In this automation process, we will open an Excel file and check whether there is any flight routes data in it. If there is, we will add two columns, one for calculating the number of days between departure and return dates, and one for combining these dates. Then, we will continue with a repeating sub-process since we want to perform operations for each row in the Excel file. In this sub-process, we will set the values for the newly added columns.` 

      1-) We started our process by using the Open activity under the Excel section to open the Excel file that contains the flight routes data. Just drag and drop the activities you want to use into the design area as I have just shown you and enter the relevant parameters. In the Name field, we wrote the excel name after the default Open to explain what is done in this step. When we similarly enter a value in the Name field for all the steps, it becomes easier to understand which operations are performed through the process flow. Then, we wrote the directory of our Excel file with its name to the Excel file name field. A reference name is given to the Excel file in the Excel name field in order to choose which excel file we are processing in the next steps of the process.` 

      2-) With Read Excel to Dataset activity, we transferred the data in the Excel file to a dataset. Later, we will modify this dataset and write data back to the Excel file once changes are completed. In the Excel Name field, we selected the reference name of the Excel file. We checked the “Has header” box because our data has a header row. By doing so, it will be possible to specify the column header values in the Column fields in other activities at which we get or set cell values. If there is no header row in the table, we should not select the “Has header” option and we should specify which column to be processed in other activities with and index value of 0 1 2 or by entering the column header as A B C. Since the name of the sheet that contains our data is Routes, we wrote this name in the Sheet Name field. If we left this field blank, the data on the Airports page, which is the first page, would be transferred to the dataset.nIn the “New dataset name” field, we gave a name to the dataset we will create.`

      <3-) In the next step, we got the row count of the dataset with the “Get Size” activity. After selecting the dataset from the list in the Dataset name field, we chose the “Row” option in the “Size Type” field because we want the number of rows to be counted. Then, in the “Result variable name” field, we defined the variable that we want to assign the row count value as ‘getSize’.`

      4-) Then, based on the variable value from the “Get Size” activity, we ensured that the process terminates if the data is not found. And the process continues if the number of rows is greater than zero. To achieve this, we added a “Exclusive Gateway” that allows us to create alternative flows in our process. For each sequence flow outgoing from this Gateway, we set a condition and process continues from the sequence flow at which the condition evaluates ‘true’ or from the sequence flow marked as default if all the conditions evaluate ‘false’. At the first sequence flow, our condition checks whether getSize variable value is equal to ‘true’ which means there is no flight route data. If this condition is met the process is completed with an end event. At the second sequence flow, we chose the “default flow” option and did not set any condition expression. The process will continue from here if the first condition evaluates ‘false’ which means there is flight data in the Excel file.`

      5-) In case the flight data exists, we continued our process by adding two columns to the dataset with the “Add Column” activity. In this step, after selecting the dataset to add columns, we wrote the column header name in the “Column Name” filed. When the “Column Index” field is left blank, the column is added to the end of the dataset. If an index value is given lower than total column size, all the columns right to newly added column is shifted. Since we want to add the column to the end of the dataset, we left this field blank. In this column, we will set a formula to calculate the number of days between flight departure and return date.`

      6-) In the next step, we added another column to set the combination of these dates.`

      7-) After this step, we included the “Sub-Process” activity under the “Structure” section to our process to create a recurring sub-process since we want to process each row in the dataset. As in our main process, we need to create a flow with a definite beginning and end in this sub-process. We need to define how many times this sub-process will be repeated in the “Cardinality” field. If we leave this field blank, the sub-process will run once. If repetition count is get from a variable, we need to write this variable name between curly braces after a dollar sign. This is the expression syntax that allows us to use any variable in the parameter fields. In case repetition count is a fixed value, we set an integer value in this field. In this example, since we want to repeat sub-process as many times as the number of rows, we wrote the getSize variable in the Cardinality field. We chose the value of the “Multi-instance type” field as “Sequential” as we want each operation to be done sequentially in our sub-process loop. When we make these definitions, the “loopCounter” variable is automatically defined at the beginning of the loop. This variable which first takes the value 0, increases by 1 at each iteration. When the value of the “loopCounter” variable reaches to the Cardinality value, the loop is automatically terminated.`

      8-) In the sub-process, we first obtained the flight return date from the dataset with the “Get action” activity. In this activity, we set the column header that we want to get data from in the “Column” field. Since we want to repeat the same operation for all the rows, we set ‘loopCounter’ variable in the Row field. The index value of rows and columns in the dataset starts with 0, same as loopCounter variable. Finally, we assigned the value we get to a variable. >`

      9-) In next step, we assigned ‘departure date’ to a variable, similarly as the previous one.`

      10-) Then, we used the Set Action activity to set an excel formula to calculate the difference of the dates we get to the newly created ‘Days Between’ column. In this activity, after selecting the dataset, we set the column header in the Column field and ‘loopCounter’ variable in the Row field. In the Value field, we wrote the formula to calculate the difference between the dates by using the equal sign at the beginning. Since the dates we want to process start from the second row of the H and G columns in the table, the row value in the formula should also start from the 2 nd row. So, in the formula syntax, we added 2 to the loopCounter variable.`

      11-) Thus, our formula will start from H2 minus G2 on the first iteration, and it will be at H3 minus G3 on the second iteration. Finally, we chose the “String” option in the “Type” field. In this step, we combined the dates and set the value in the newly added column named ‘Concatanated Dates’. Here, we have written the variables that hold the date values in the Value field with a hyphen between them. Then we completed the loop activity with the End event, which allow us to end the process flow. `

      12-) We used Write Dataset to Excel activity after we finished the operations in the subprocess, which allows us to transfer all the data in a dataset to an Excel file. In this activity, after selecting the dataset and the Excel file, we chose the “Include header” option because we want to copy the header information for newly added columns.`

      13-) We saved the Excel file in the output folder with the Save and Close activity. For this, we entered the directory, file name and extension of where we want to save our file in the Excel file name field. In the Action field, we selected the Save and Close option from the list and completed our process.`

      14-) Now let’s run our process by clicking the Run button. Since the activities used in this process completely run at the backend, we do not see any activity on our screen. In the scheduled process pages, we can see our process was completed successfully. You can follow the steps ran in the process and see the detailed information about the variables that was used in the process on this screen. When we look at the final version of our Excel file, you can see the columns we added. >`

      15-) We have completed our How To video. I hope it was useful for you. Hope to see you again.`

## Email automation using Robusta RPA

<div align="center">
      <a href="https://www.youtube.com/watch?v=cRY6BupUSSE&t=10s">
     <img 
      src="https://user-images.githubusercontent.com/87966919/130401638-5fe5aaf0-13d9-4ad8-a407-994216cf2db0.jpg" 
      alt="Robusta" 
      style="width:100%;">
      </a>
    </div>
    
    Hello there,
    
    In this video, we will show you how to design a sample process that includes e-mail operations with Robusta RPA tool step by step. In our process, we will first connect to an e-mail account and search for unread e-mails with a specific subject and an attached file. In the next step, we will forward each matching e-mail which meets certain condition to another e-mail account. For the e-mails which do not meet the condition, we will reply to the sender that the request is invalid.

    1- We started our process first by using the IMAP-SMTP Connection activity under the Mail section to connect to the e-mail account. Just drag and drop the activities you want to add to the process flow into the design area as I have just shown you.While SMTP protocol is used for sending e-mail; IMAP protocol is used for e-mail reading. The configuration of both protocols is provided at this component to be able to perform e-mail operations. You can easily learn the parameter values for this component from the website of the e-mail account provider. This table contains information about how to set the parameters. According to the information here, we filled the relevant fields in the IMAP SMTP connection activity. 

    2-) After the connection is done, we found the date three days before today, which we want to use as the minimum date in Mail search activity, with the Script Task activity. We used the javascript to find the date. You can see the script to find the date of 3 days ago in the script area. Here, we first assign the current date to the variable threeDaysAgo with the New Date function. The current date that we assigned came in the form of year, month, day with hyphen between them. Then we updated this variable that we created to be the date of 3 days ago. After this step, we converted this variable to string format with toLocaleDateString function. From this string, we took the year, month, day values that we had separated with the split method and concatenated them with adding a dot between them. Finally, we saved this variabl with the execution.setVariable function to use it as a minimum date in our next step, the Search activity. The MinDate variable here will be used to find e-mails less than 3 days old. Again, we wrote the variable name between curly braces after a dollar sign which is the standard way referring to a variable.

    3-) After this step, we used the Search activity under the Mail section and in the connection name field, we selected the connection reference name that we defined in the first activity from the list. Then, we found the e-mails with the subject containing the phrase “Flight Routes” and containing an attached file. For this, we wrote “Flight Routes” in the subject field and ticked the has attachment box. We did not change the default value of INBOX in the Folder Name field, as we want the relevant search to be in the Inbox. Then, by selecting the unread box, we searched for unread e-mails, and we defined a variable in the dataset field to assign the all e-mails’ data to a dataset. In this example, we set the e-mail limit to be searched as 10. This means that even if there are more than 10 e-mails matching the search criteria, only the 10 most recent e-mails will be imported into the dataset. In this example, we left the From parameter blank, where we can choose from whom the e-mail came from. Now, I want to open an Excel file that contains the data of the dataset resulting from this activity and show you which information we received via e-mail. As you see, we get various information such as subject, sender’s e-mail, mail body, date sent.

    4-) After the search activity, we found the number of e-mails transferred to the dataset and assigned the result to a variable. Since we want the rows of the dataset to be counted here, we chose the ROW option in the Size Type field from the list. 

    5-) Then, the process is terminated if no e-mails is found, and the process continues if there is at least one e-mail. To do this, we used a gateway that allowed us to control how a process flows according to the conditions we set. For each arrow leaving the gateway we set a condition expression according to the e-mail count variable. If the value of this variable is 0, we ended the process flow. Otherwise, we chose the default flow option, and we did not set any condition expression. The process will continue from here if it does not match a condition.

    6-) If the process does not end at this point, the process continues with a loop activity, since the operations will be done independently for each e-mail. We included the Sub-process activity from the Structure section to create a loop activity in the process. As in our main process, we need to create a flow with a beginning and end event in this sub-process. We need to define how many times this sub-process will be repeated in the Cardinality field, and if we do not make this definition, the sub-process will run once. In this example, since we want to loop as many as the number of matching e-mails, we wrote the variable that holds the number of e-mails in the Cardinality field in curly brackets. We chose the value of the Multi-instance type field as Sequential because we want each operation to be done sequentially in our loop. When we make the definitions, the “loopCounter” variable is automatically defined at the beginning of the loop. This variable, which first takes a value of 0 increases by 1 at each iteration. When the value of the “loopCounter” variable reaches the Cardinality value, the loop is automatically terminated.

    7-) In the loop, we first ensured that the e-mail we are processing with the Read/Save/Attachment activity is marked as read, the attached files list are transferred to a dataset, and the files attached are downloaded to the desired directory for archiving purposes. In this activity, after selecting the testCon connection name from the list in the onnection name field, we wrote the loopCounter variable in the E-mail number field, which indicates current iteration number. In this way, we ensured that e-mails are processed one by one. Since we want to archive the attached files, we checked the Save attachments box and entered the directory path where we want the files to be saved in the Save Path field. By selecting the Mark as Read option, we ensured that the e-mails are marked as read. Thus, when the process runs again, we will not be processing the same e-mails again and again. Finally, we imported the list of attached files into a dataset. 

    8-) In this step, similar to the use of the Get Size activity that we explained in the first parts of the process, we have found the row number of the dataset to which the list of the attached files is transferred.

    9-) After finding the number of attached files, we added another gateway to the process as we want to perform a different operation if there is more than 1 attached file, and a different operation if there is only 1 file.

    10-) If there are more than one attached file, the flow continues with Reply activity, and the message “Cannot process more than one attached file” is returned as a response to the sender. In this activity, after selecting the testCon connection from the list in the connection name field, we wrote the loopCounter variable in the mail no field to respond to the e-mail in progress. Later, we chose the is HTML box because our e-mail body is in html format. At the same time, we have selected the reply all option by ticking the reply all box.
    
    11-) If there is 1 file attached, the e-mail is forwarded to another e-mail address with the Forward activity. In this activity, the from and to e-mail addresses, e-mail no, subject, and message fields are defined. We completed our subprocess flow with an End event. Then we used another end event to terminate the main process. 

    12-) Now let’s run our process by clicking the Run button. Since the activities used in this process work at the backend, we do not see any action on the screen. As seen on this screen, our process is completed successfully. You can follow the steps of the process and information about the variables in the process on this screen. As a result, the attachments are downloaded to the specified folder and the e-mail is successfully forwarded to our e-mail account.

    13-) We came to the end of our How To. I hope it was useful for you.

## Price comparison on Booking.com with Robusta RPA

<div align="center">
      <a href="https://www.youtube.com/watch?v=R2Z-9_0tDIw&t=21s">
     <img 
      src="https://user-images.githubusercontent.com/87966919/130402017-0f64271f-3d01-4bb4-95ab-6c08be38f75f.jpg" 
      alt="Robusta" 
      style="width:100%;">
      </a>
    </div>

      In this video, we will show you an RPA process developed by roboosta RPA tool which allows you design no-code RPA processes. In this automation process, we will open booking.com website and search the ticket prices for a given flight route and dates. Among the listed ticket prices, the price which is marked as ‘Best’ option will be determined and shared by clicking the share link and populating necessary information on the popup window. 
      
      Let’s run the process and see how the robot works. The process is successfully scheduled and will run in few seconds on the robot. Now the process is running and the operations in the process flow is being performed by the robot. Flight route and dates are populated on the page. And search button is clicked. After search results are loaded, the share link is clicked for the price given as best option. 
      
      On the popup window, all the fields are populated. And share button is clicked. After the sharing is done, the process is completed. Let’s check our email for the shared ticket price. As you see we got the email from booking website. Our RPA process is successfully run.
