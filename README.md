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
Use time.sleep(5) before the browser is closed
