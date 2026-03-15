"# n8n_pr_review_agent" 
-this is a n8n project for the main purpose of automating pr reviews ,fixes etc to help devs save time and automate stuff
-my n8n version is hosted on docker and exposed with ngrok (ngrok is required in order for the github trigger and other webhook nodes to work)
-install ngrok from ngrok.com
-follow the instructions and then in your cli open ngrok with the port running n8n mine is 5678
-after running the command you will see a message "forwarding <link> to n8n port copy that link and start n8n image specify the name and port and in the env variables type 
name:WEBHOOK_URL and the value is the link provided by ngrok 
THATS IT !

