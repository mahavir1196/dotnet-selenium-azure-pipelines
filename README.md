<h2><strong>Getting Started with Azure Pipelines and CrossBrowserTesting</strong></h2>
<p><em>For this document, we provide complete example files in our <a href="https://github.com/crossbrowsertesting/selenium-azure-pipelines/tree/main">Azure Pipelines Github Repository</a>.</em></p>
<p><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops">Azure Pipelines</a> is a continuous integration tool that lets you automate your development process quickly, safely, and at scale. Through Azure Pipelines' integration with GitHub, GitHub Enterprise, Azure Repos Git &amp; TFVC, Bitbucket Cloud, and Subversion, every time you commit code, a build is created and automatically run in a clean container or virtual machine, allowing you to test every commit.</p>
<p>In this guide we will use Azure Pipelines with Github for testing using the Selenium Webdriver and the Python programming language.</p>
<h3>Setting up Azure Pipelines</h3>
<p>1. Sign into your Azure DevOps organization or follow the detailed guide <a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops">here</a> to create a new one.</p>
<p>2. Install the <a href="https://marketplace.visualstudio.com/items?itemName=CrossBrowserTesting.cbt-tasks">CBT for Azure DevOps</a> extension for your organization</p>
<p>3. Navigate to your GitHub account and <a href="https://github.com/new">create a new repository</a>.</p>
<h4><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline1.png" /></h4>
<p>4. Add file test_selenium.py</p>
<pre><code>
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
import os, time

username = os.getenv("CBT_USERNAME")
authkey = os.getenv("CBT_AUTHKEY")


caps = {
 'platform': 'Windows',
 'browserName': 'Chrome',
}

driver = webdriver.Remote(
    command_executor="http://%s:%s@hub.crossbrowsertesting.com/wd/hub"%(username, authkey),
    desired_capabilities=caps)



driver.get("http://www.google.com")
if not "Google" in driver.title:
    raise Exception("Unable to load google page!")
elem = driver.find_element_by_name("q")
elem.send_keys("CrossBrowserTesting")
elem.submit()
print(driver.title)
driver.quit()
</code></pre>
<p>5. Add file requirements.txt</p>
<pre>requests==2.22.0
selenium==3.141.0</pre>
<h4><strong>Building Your Pipeline</strong></h4>
<p>1. From the <a href="https://dev.azure.com/">Azure DevOps dashboard Dashboard</a>, create a new project and select Pipelines</p>
<p><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline2.png" /></p>
<p>2. Create a new pipeline  and set up your GitHub repo</p>
<p><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline3.png" /></p>
<p><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline4.png" /></p>
<p>3. Add a new Service Connection from the Project Settings page using the type CBT Credentials</p>
<p><img class="" src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline8.png" width="286" height="384" /></p>
<p>4. Add the CrossBrowserTesting Configuration task to your azure-pipelines.yml file</p>
<p><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline5.png" /></p>
<p><img src="http://help.crossbrowsertesting.com/wp-content/uploads/2020/11/azure_pipeline7.png" /></p>
<p>5. Save and Run</p>
<p>You should see your build start to run in Azure Pipelines and in the CrossBrowserTesting app <a href="https://app.crossbrowsertesting.com/selenium/results">here</a>.</p>
<p>If you have any questions or concerns, feel <a href="mailto:support@crossbrowsertesting.com">free to get in touch</a>.</p>
