# playwright_notes
A collection of commands to use playwright effectively in python projects. 

## Recording the code to open a website
In terminal run 'codegen'. 
Example for opening ManageBac:
```
playwright codegen bonnis.managebac.com
```
Now perform all the steps you want to be performed by your Python code. 
Copy and paste the recorded code into a Python program before closing the browser. 
Run the Python script and watch it replay all the steps. 

## Keep the website open for 5s before it is closed
Before the browser is closed use 
```
time.sleep(5)
```

## Record your login data and use them in your next run of codegen
Save the login data in a json file called auth.json
```
playwright codegen bonnis.managebac.com --save-storage=auth.json
```
Now run the same command again, this time loggin into the website using the stored data: 
```
playwright codegen bonnis.managebac.com --load-storage=auth.json
```

Troubleshooting: 
Should the auth.json file not be found by the program, then give the full path to file. 
Replace 
```
context = browser.new_context(storage_state="auth.json")
```
with
```
context = browser.new_context(storage_state="/Users/funky_coder/Downloads/auth.json")
```
**Remember to delete the auth.json file when you are done to be safe on the web!**


## CSS locators: 
[Here](https://youtu.be/UXLnkeYHZlY?si=UtZuqCxW0qt88L6g) is a helpful video for using css locators. 
Important points of information (from the video): 

Use the inspect function (right click/two finger click on your browser), then [use the arrow](https://youtu.be/UXLnkeYHZlY?si=JBqcVrhniYRmtOt0&t=401) to inspect a particular element on your screen like your username field. 

This would be the code for opening a website: 
```
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto('https://demo.automationtesting.in/Index.html')
```

### CSS locators using the id:
When your field has an id, the code is a little simpler. 
The inspector gives you the id of the email address field: <img width="544" alt="id" src="https://github.com/user-attachments/assets/18bd1bed-e5bd-4f90-83ba-57b909c81e72">

The generic code used for an id using CSS selectors is: 
```
page.wait_for_selector('#name_of_id')
```

So here this would become: 
```
page.wait_for_selector('#email')
```

Assign to a variable and type in the email address gives you: 
```
email_field = page.wait_for_selector('#email')
email_field.type('test@gmail.com')
```
Then you just click the arrow button on the right: 
```
enter_btn = page.wait_for_selector('#enterimg')
enter_btn.click()
page.wait_for_timeout(5000)
```

### CSS locators using attributes
Use the following URL for this example: https://opensource-demo.orangehrmlive.com/web/index.php/auth/login

This time there is no id for the username.
<img width="399" alt="no_id" src="https://github.com/user-attachments/assets/51da7e6d-b4db-46c1-8c2e-161bbe698dae">

The generic reference to the CSS locator using attributes is: 
```
tagname[attribute="value"]
```
Based on the image above this would become: 
```
page.goto('https://opensource-demo.orangehrmlive.com/web/index.php/auth/login')
username = page.wait_for_selector('input[name="username"]')
username.type('Admin')
```
You can then do the same for the password and for the login button. 

Note that the tagname for the login button is 'button': 
```
button = page.wait_for_selector('button[type="submit"]')
button.click()
```
Instead of time.sleep() you can use 
```
page.wait_for_timeout(1000)
```
The time is set in milliseconds (1000 --> 1s) 

## XPath locators: 
[This video](https://youtu.be/-KmfzY3P1vI?si=ymM1c1tPeLXqM2Yf) explains the use of xpath locators. 

The syntax for usiung relative xpaths is: 
```
//tagname[@attributename="value"]
```

Applied to the website used above you get the following code: 
```
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto('https://opensource-demo.orangehrmlive.com/web/index.php/auth/login')
    username = page.wait_for_selector('//input[@name="username"]')
    username.type('Admin')
    password = page.wait_for_selector('//input[@name="password"]')
    password.type('admin123')
    page.wait_for_timeout(1000)
    button = page.wait_for_selector('//button[@type="submit"]')
    button.click()
    page.wait_for_timeout(3000) # This is just so you can see what is happening. 
```

## XPath locators using displayed text: 

On the website above there are many links. One of them is for users forgetting their password.
<img width="483" alt="Forgot" src="https://github.com/user-attachments/assets/00b91a76-0400-49c4-8a36-a69a8b94a171">

While you can use any of the methods above to locate and then click that link, you may also locate the element by text. The underlined black text in the html needs to be copied. 
<img width="667" alt="Forgot_txt" src="https://github.com/user-attachments/assets/c39db551-5ec4-4127-8e4d-df4379ea1c62">

The syntax for using xpaths and text is: 
```
//tagname[text()="text"]
```
So here this would be: 
```
//p[text()="Forgot your password? "]
```
In context this would be: 
```
forgotton_pw = page.wait_for_selector('//p[text()="Forgot your password? "]')
forgotton_pw.click()
```

For more information see: 
https://playwright.dev/python/docs/api/class-locator
