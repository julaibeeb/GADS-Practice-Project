___
#Getting Started with App Engine
___

---
## Objectives

* Initialize App Engine.

* Preview an App Engine application running locally in Cloud Shell.

* Deploy an App Engine application, so that others can reach it.

* Disable an App Engine application, when you no longer want it to be visible.
---


*** 
    -- Initialize App Engine 
***
# Steps
-- Initialize your App Engine app with your project and choose its region:
#
    gcloud app create --project=$DEVSHELL_PROJECT_ID
   > When prompted, select the __region__ where you want your App 
    Engine application located
-- Clone the source code repository for a sample application in the hello_world directory:
#
    git clone https://github.com/GoogleCloudPlatform/python-docs-samples
-- Navigate to the source directory:
# 
    cd python-docs-samples/appengine/standard_python3/hello_world
# 2. Run Hello World application locally
-- package list using this command
#
    sudo apt-get update
-- Set up a virtual environment in which you will run your application
# 
    sudo apt-get install virtualenv
> if prompted press [Y/n] press y and hit enter
#
    virtualenv -p python3 venv
-- activate virtual environment 
#
    Activate the virtual environment.
-- Navigate to your project directory and install dependencies. by using 
#
     cd python-docs-samples/appengine/standard_python3/hello_world

     pip install  -r requirements.txt
-- Run the application:
#
    python main.py
> In Cloud Shell, click Web preview (Web Preview) > Preview on port 8080 to preview the application.
> To access the Web preview icon, you may need to collapse the Navigation menu

-- the result will be 
> Hello World!
> press CTRl + C


# 3. Deploy and run Hello World on App Engine 
-- To deploy your application to the App Engine Standard environment:
1. Navigate to the source directory:
    ``` bash
        cd ~/python-docs-samples/appengine/standard_python3/hello_world
    ```
2. Deploy your Hello World application.
    ```
        gcloud app deploy
    ```
> If prompted "Do you want to continue (Y/n)?", press Y and then Enter.
> This app deploy command uses the app.yaml file to identify project configuration.
3. Launch your browser to view the app at http://YOUR_PROJECT_ID.appspot.com
    ```
        gcloud app browse
    ```
    > Copy and paste the URL into a new browser window.
-- result:
#
> Hello World!
#
> Using the Cloud Console, verify that the app is not deployed. In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Dashboard.
# 4. Disable the application
> In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Settings.

Click Disable application.

Read the dialog message. Enter the App ID and click DISABLE.

If you refresh the browser window you used to view to the application site, you'll get a 404 error.