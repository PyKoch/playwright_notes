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

