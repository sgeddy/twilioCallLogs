# twilioCallLogs
Scripts to get all call logs from your Twilio account. Includes getting all logs in parent and subaccounts and getting logs from the Archive (older than 13 months) with the Bulk Export Utility API.

# Get call logs from parent & all subaccounts for past 13 months (all logs available from /Call resource)
File = call_logs_all_parent_and_subaccounts_past13months.js

1. Ensure your directory has a .env file OR update the file with your parent account SID and auth token.
2. Run the file (command = node call_logs_all_parent_and_subaccounts_past13months.js).

All available call logs in the parent and all subaccounts will be printed to the Console separated by account.


# Get call logs older than 13 months with the Bulk Export API from parent & all subaccounts
Files = bulk_export_initiate_jobs.js and bulk_export_fetch_day_logs.js

1. Update the file bulk_export_initiate_jobs.js  
    a). Ensure your directory has a .env file OR update the file with your parent account SID and auth token.  
    b) Update the vars statusEmail, startDate, and endDate.  
    NOTE: This email will receive status updates for all jobs in all accounts. Please note this can be a lot of emails depending on the account volume.  
    ![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/f394c703-820d-4fc6-ac07-f113b53bfe2d)  
    c) Update the startDay and endDay to the time span you want the logs from. This should be a time span older than 13 months (past 13 months should be fetched with the /Call Resource of the API).  
    The time span should be no greather than one year. The Bulk Export API has a daily limit of about one year of jobs. ![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/1e57de7f-df2b-4730-b756-75e67d52fb93)

7. Once you've updated the file and saved, execute it (command = node bulk_export_initiate_jobs.js). This will initiate jobs for all parent and subaccounts to get call logs from our archive with the bulk export API.
![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/6f25f0fc-26b3-441f-9a00-35091b7b15b0)

8. Wait until all jobs are complete before executing the next file. You can check the status of your job and how many Days are left to fetch in the Console: https://www.twilio.com/console/voice/settings/calls-archives.  
OR you will know when all jobs are complete when you stop receiving status emails. 

9. Update the file bulk_export_fetch_day_logs.js.  
      a) If this file is not in the same directory as the other files, ensure your directory has a .env file OR update the file with your parent account SID and auth token.  
      b) Install 'fs' and 'https' npm packages if you do not already have them installed (https://docs.npmjs.com/cli/v6/commands/npm-install).  
      ![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/c8ac3d90-bb3b-4a4f-b582-00d495784eff)  

      c) Update the vars startDate and endDate to the same dates you initiated jobs for.  
      ![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/97f5f40a-93d6-423a-89ff-69089f2bdd00)  

10. After you've updated the file and saved, execute it (command = node bulk_export_fetch_day_logs.js). This will fetch the logs from each "Day" with the Bulk Export API for all parent and subaccounts during the timespan you defined. The Bulk Export API returns a "redirectTo" field which contains a one time accessible S3 url. The script then uses fs to download the .gz file from the S3 url and save the contents to a new file in your directory.  
![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/0bb701c7-d42e-478d-a701-71ab74438e25)

11. Now you will unzip all the files and remove the ones which are empty (i.e. there were no call logs on that day in that particular account). Command = gunzip *.gz; rm *.gz  
![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/28096233-0d9d-4cce-a38a-8ac226e1b7ca) ![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/416772e5-c94f-4429-9f19-beeb24174df8)

12. You will now have files that contain the call logs for each day in each account in a JSON format.  
![image](https://github.com/sgeddy/twilioCallLogs/assets/26208555/dbfa4996-b36a-455d-ad76-0f9ba3757763)
