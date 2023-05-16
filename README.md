# **MSO_E5_Dev_AutoRenew**

MSO_E5_Dev_AutoRenew is a Python application based on Git Actions that uses Microsoft Graph API to activate Microsoft Office 365 E5 Developer Trail membership auto-renewal automatically. This guide will provide you with easy-to-understand steps for setting up and running the application.

### Special Notes/Thanks ###
* Normal version address: [https://github.com/kylierst/MSO_E5_Dev_AutoRenew_REVISION_2](https://github.com/kylierst/MSO_E5_Dev_AutoRenew)
* Thanks to Ken5998 for code improvements and fix job skipping

## **Prerequisites**

- A GitHub account
- An existing or new Microsoft Developer E5 account with trial Subscription
- An Azure Portal account
- Basic knowledge of GitHub, Python, and Azure Portal

## **Setup Steps (Encrypted Secure Version)** (checkout [images](images/))

1. Fork the repository on GitHub. If your old/main GitHub account has GitHub Action jobs ban, create a new GitHub account.
2. Go to the Azure portal and login. If you have a developer subscription, there is no need to create an account. Azure will auto-login if you have already logged in with a Microsoft account of developer subscription.
    - Select the hamburger icon and go to Azure Active Directory > + Add > Select App registration. (https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/CreateApplicationBlade/isMSAApp~/false)
    - Select any app name, and "Supported account types" will be option 2 "Accounts in any organizational directory (Any Azure AD directory - Multitenant)".
    - Select "web" for the Redirect URI and set its value as "http://localhost:53682/"
    - Click "register".
    - After registering, the app dashboard will open. In the Essentials section, you can get the Application ID and make a secret from Client credentials. To create a secret, click "Add a certificate or secret", then "+ New client secret". Select the name expiration (I select 24 months/2-year expiration for now), and click "Add".
    - After that it show row with 2 column "Value" and "Secret ID", we are interested in "Value" column value
    - Save : Application Id, Value (Secret)
3. Set app/API permissions. Go to the API permission tag from the app dashboard/overview using the left-side panel. You will see Microsoft Graph. Click on it and add all the permissions that have been added (check ss) and update permissions.
4. Install rclone (you can get installation guide here : https://telegra.ph/Rclone-Guide-for-Beginners-04-15)
5. Use the below template command and run it in the terminal after rclone has been installed successfully.
`rclone authorize "onedrive" "{Application Id}" "{Secret Value}"`
6. The output of step 5 is JSON response data that has access_token, token_type, refresh_token, and expiry. We need the refresh_token only, so copy it. If it is hard to find, use any JSON formatter, which will make it easier to find it.
    - Store the refresh_token with other stored values. We will need them all in the future.
7. Go to your fork repo Settings > Security > Secrets and variables > Actions (see attachment for it).
    - If you can't find it, add "settings/secrets/actions" after your repo URL.
8. Add the following three variables:
    - CONFIG_ID = {Application Id}
    - CONFIG_KEY = {Secret Value}
    - REFRESH_TOKEN = {refresh_token}
9. Go to fork setting > actions > general. Scroll down and set Workflow permissions to read and write, then click save.
10. Create a GitHub account access token by going to the personal settings page on GitHub. Select Developer settings > Personal access tokens > Generate new token.
    - Name it anything you like that describes the use of this token.
    - Give the token 3 permissions: repo, admin:repo_hook, and workflow.
    - Set the expiration to "Never".
    - Generate it. It will show the token for only one time. There is no need to store the token; just generate it.
    - If you refresh page, token disappear and token name show there
11. Go to your forked repo and action tab. Select MSO_E5_Dev_AutoRenew_Educational and click "Run workflow." 

## **Additional Information**

- The default setting is to run three rounds every six hours from Monday to Friday. You can modify your own crontab to change the frequency and time.
- If you need to modify the API calls, you can check the Graph Explorer at **[https://developer.microsoft.com/graph/graph-explorer/preview](https://developer.microsoft.com/graph/graph-explorer/preview)**.
- The GitHub Action provides a virtual environment with 2-core CPU, 7 GB RAM memory, and 14 GB SSD hard disk space.
- Each repository can only support 20 concurrent calls.
