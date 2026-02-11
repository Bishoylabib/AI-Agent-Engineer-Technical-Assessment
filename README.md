# Workflow Automation
---
## 📝 Brief explanation
I built an n8n workflow that fetches user data from a public API (dummyjson.com/users), process the records and store them in Google sheets.The workflow split the API response into seperate items, then filters out users with emails not ending with '.com', then making fields using user data, like having Full Name field with values of users' firstName and lastName fields, to append these processed users as new rows in Google Sheet. Also added retry logic in the HTTP request node to API failures that retry 3 times each within 1000ms, and implemented error path to go through if there is still an error. On a successful workflow execution, a summary email is sent containing the spreadsheet link and important metrics, while if there is HTTP request error, an email is also sent with the error status and code. 

---
## ⚠️ Challenges
**API response:** 

The response was 1 output with multiple users so I had to use a Split Node to split the users to be several outputs.

**Post-processing data:** 

After processing data and inserting them in the spreadsheet, the output of the spreadsheet was equal to the several users, so when I tried to send the successful email at first it returned it several times. Initially I solved it by making the 'send an email' node to execute once, but I saw a more professional approach was aggregating the data to make it 1 output, then sending the email. That also helped me when making the summary fields for the summary/success email.

---
## ✨ Bonus
**Success & Error Emails** - Email sent on error or success for clear workflow outcomes.

**Retry Logic** - The HTTP Node retries failed API requests to handle temporary API issues.

**Summary Metrics** - Aggregated data are used in the Success email to show:
- Total users fetched
- Total users processed
- Total excluded users
- Workflow completion time
- Link to the Spreadsheet

These extra features improve workflow reliability, observability and useability 

---
## ▶️ Instructions to run the workflow
1. import the JSON workflow in n8n (included in the repo)
2. Use a Google account to sign in Google Sheets and Gmail in their nodes
3. Click Execute Workflow
4. On success, verify rows are added to the Google Sheet and email is recieved
5. break the HTTP request temporarily (by misspelling the URL for example) to confirm the error email is triggered and sent

---
## 📸 Screenshots of the Workflow
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/066d8cf6-aae7-4a8d-a6c4-b11fc73d483a" />

<img width="1920" height="1200" alt="Screenshot (233)" src="https://github.com/user-attachments/assets/c26dc99f-deb6-4dce-8821-3d602b535103" />

<img width="1920" height="1200" alt="Screenshot (242)" src="https://github.com/user-attachments/assets/eca7851c-5ba5-469b-b494-e5d93b7b5ea6" />

<img width="1920" height="1200" alt="Screenshot (245)" src="https://github.com/user-attachments/assets/1bb25a73-e0ea-4892-9d2f-7ff226421c73" />

<img width="1920" height="1200" alt="Screenshot (246)" src="https://github.com/user-attachments/assets/bbee9b71-5303-41af-9d56-b4e214ad91e5" />

---
## 📊 Workflow Output

<img width="1920" height="1200" alt="Screenshot (252)" src="https://github.com/user-attachments/assets/284ad77f-dee6-4ac2-923a-dd45dfea51ed" />

<img width="1920" height="1200" alt="Screenshot (254)" src="https://github.com/user-attachments/assets/08abb2db-a683-4459-b4eb-757f91c49e39" />

<img width="1920" height="1200" alt="Screenshot (253)" src="https://github.com/user-attachments/assets/fcbe4004-e928-4bb6-8487-978cf32547e6" />

