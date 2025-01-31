# NBA Game Day Notifications / Sports Alerts System
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lznr9eu5x3hnvlsfwnrc.png)

---
## Project Overview

> This project is an alert system that sends real-time NBA game day score notifications to subscribed users via SMS/Email. It leverages Amazon SNS, AWS Lambda, Python, Amazon EventBridge, and NBA APIs to provide sports fans with up-to-date game information. The project demonstrates cloud computing principles and efficient notification mechanisms.

---
## Features

- Fetches live NBA game scores using an external API.
- Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
- Automates scheduled updates using Amazon EventBridge.
- Designed with security in mind, following the principle of least privilege for IAM roles.

---
## Prerequisites

- Free account with subscription and API key at [sportdata](sportsdata.io) (subscribe for NBA games).
- Personal AWS account with a basic understanding of AWS and Python.

---
## Technical Architecture


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7jcvjloggunnsvojg93d.jpg)
---
## Technologies

- Cloud Provider: AWS
- Core Services: SNS, Lambda, EventBridge
- External API: NBA Game API (SportsData.io)
- Programming Language: Python 3.x
- IAM Security:
- Least privilege policies for Lambda, SNS, and EventBridge.

---
## Project Structure

Clone this repo: 
`git clone https://github.com/Vivixell/Game-Result-Notification.git`

```
Game-Result-Notification/
├── src/
│   ├── gd_notifications.py          # Main Lambda function code
├── policies/
│   ├── gd_sns_policy.json           # SNS publishing permissions
│   ├── gd_eventbridge_policy.json   # EventBridge to Lambda permissions
│   └── gd_lambda_policy.json        # Lambda execution role permissions
├── .gitignore
└── README.md                        # Project documentation
```
---
## Setup Instructions

#### Create an SNS Topic

 

- Open the AWS Management Console and search for SNS.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/znaatlg49cauzu5mttcr.png)

- Navigate to the **SNS** service, then click **Topics → Create Topic**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2hqpcykwrln1sbaftrrf.png)

- Select **Standard** as the topic type.
- Name the topic (e.g., `Victor_Game_Notification`) .

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0usckgdz78cgheu4h9wm.png)

- Leave other options as default, click **Create Topic** and copy the **ARN of the created topic** (needed later).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gvqct4rpurq17njkeyvn.png)

#### Create an SNS Subscription
 

- Go to **Create Subscription** under the newly created topic.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3drhcrf68uiyci687xrr.png)

- Select a **Protocol**:

 - **Email**: Enter a valid email address.
 - **SMS**: Enter a valid phone number in an international format (e.g., +2348056789012).

- Click **Create Subscription**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w53sdqmf49a0ogf7ibce.png)

- If using email, check your inbox and confirm the subscription. For SMS, it activates immediately.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zgy48cm203abo1pq61y3.png)

#### Create an SNS Publish Policy

- Open **IAM** in the AWS Management Console.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/do6dbhzbick74b053p2a.png)

- Navigate to **Policies** → **Create Policy**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1v1ueyc2purarmobondt.png)

- Click **JSON** and paste the policy of the GitHub repo from `gd_sns_policy.json` (Replace the ARN with your SNS topic's ARN copied earlier).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/oiklhk18pcukm8htukpe.png)

- Click **Next: Review**, and name the policy (e.g., `Victor_Game_Policy`), and create it.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nl68ch5z05nik440r8r3.png)

#### Create an IAM Role for Lambda

- In **IAM**, go to **Roles** → **Create Role**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d4ij06l7bf36sew4qr7y.png)

- Select **AWS Service** → **Lambda** → **Next.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uu23htv2p52f68a0mdj4.png)

- Attach these policies:

 - SNS Publish Policy: `Victor_Game_Policy`.
 - AWS Managed Policy: `AWSLambdaBasicExecutionRole`.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/00hgfyclo18ob5g1vw85.png)

- Click **Next: Review**, name the role (e.g., `Lambda_Game_Role`), and create it.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hy86aijqzzg2ymph1flp.png)

#### Deploy the Lambda Function

- Open the **AWS Management Console** and go to **Lambda**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j3szcyxkzfw79lt4ecxz.png)

- Click **Create Function** → **Author from Scratch**.
- Name the function (e.g., `Victor_Game_Notty`).
- Select **Python 3.x** as the runtime.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jqju98a7jfss0yp2b0pl.png)

- Assign the **Lambda_Game_Role** created earlier.
- Click **Create Function**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yjcrikz840prt2achocb.png)

- Under **Function's Code Tab**:

 - Copy the content of `src/gd_notifications.py` and paste it into the inline editor.
 - **Click Deploy**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2fz9wyfwvx0ujqynt3m9.png)

- Under **Configuration** → **Environment Variable**s, add:
- `NBA_API_KEY`: Your NBA API key.
- `SNS_TOPIC_ARN`: The ARN of the SNS topic created earlier.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8lmxrhck5vkf14nk1nui.png)


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a2ms4js63cq9lbl9hdzv.png)

- Save the settings.

#### Test the Lambda Function

- Click **Test** → **Create a test event**.
- Name it (e.g., `Test1`) and save it.
- Click **Test** and check your email/SMS for notifications.
- Retry if needed (delays may occur due to API latency).


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j17pnd8s6s0h5m0s677a.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r47g0znfzuvpou128mbq.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gh0wr5ed69ao5rad6q0f.png)

#### Set Up Automation with EventBridge

- Open **EventBridge** in the AWS Console.
- Go to** Rules** → **Create Rule**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/chd04n4hsll3wlzd5621.png)

- Choose **Schedule** as the event source.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eap0r1jtus0lyn90ebbc.png)

- Set a **recurring schedule** using this cron expression:
- `0 9–23/2,0–2/2 * * ? *`

- Breakdown:

 - **Minute (0):** Runs at the start of the hour.
 - **Hour (9–23/2,0–2/2):** Every 2 hours from 9 AM–11 PM and 12 AM–2 AM.
 - **Day of Month (*):** Any day.
 - **Month (*):** Any month.
 - **Day of Week (?):** Any day.
 - **Year (*):** Any year.

- Turn off **Flexible Window** and click **Next**


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ds2wzvlxku8xyp4duzts.png)

- Under **Targets**, select the Lambda function (`Victor_Game_Notty`).


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y060q7ponuhtjvzdnyeg.png)

Click **Next, Next,** then **Create Schedule**.


#### What We've Learned

- Designing a notification system with **AWS SNS** and **Lambda**.
- Securing AWS services using **least privilege IAM policies**.
- Automating workflows with **EventBridge**.
- Integrating external APIs into cloud-based workflows.


#### Future Enhancements

- Add **NFL score alerts** for extended functionality.
- Store user preferences (teams, game types) in **DynamoDB** for personalized alerts.
- Implement a **web UI**.

---
That's it! You now have an automated NBA game alert system running in the cloud. 
