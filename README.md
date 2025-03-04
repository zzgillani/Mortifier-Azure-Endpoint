# Mortifier Endpoint
This API endpoint acts as a wrapper around the Mortifier CLI tool, allowing for faster and concurrent text extractions when working with large file batches. 


### Method
    GET http://mortifier-endpoint.azurewebsites.net/api/blob_extraction/{project_id}/{guid}
#### Parameters
    - **project_id**: The GUID of the project, as defined as the container ID of the required file's at-rest Azure Blob storage location.
    - **guid**: The GUID of the file as defined in the Azure blob.




# Quickstart
## Pre-requisites
> install python

### Clone Repo
    git clone https://github.com/pclindustrial/github-template.git
    cd github-template.git
    POST http://mortifier-endpoint.azurewebsites.net/api/blob_extraction/{project_id}/{guid}

### Setup venv
    py -m venv env
    .\env\Scripts\activate
    py -m pip install -r .\requirements.txt

### Run Basic Command
    cd src\
    py sample.py

## Formatting, Linting, and Static Analysis 
- **Formatting** is the practice of converting formatting conventions to a standard form for things like whitepaces, quoting, comments, and newlines. In python, it’s dictated by PEP-8 and the Black formatter.  
- **Linters** are tools that analyzes your code to find programming errors, bugs, and inconsistencies (ie. Unused import statements and variables)
- **Ruff** is a Python linter and formatter
  
### Ruff installation
    py -m pip install ruff

### Ruff as a linter
- To run Ruff as a linter, enter the following command into your terminal/Powershell: 
- Uses ‘check’ as it’s keyword
  
      ruff check . note:This command lints ALL files in the directory
      ruff check path/to/examplecode.py #this command lints a specific python file (in this case, a python file called “examplecode”)
      ruff check path/to/folder1 #note this command lints all the files in the folder titled “directory1”'
  
### Ruff as a formatter
- To run Ruff as a format, enter the following command into your terminal/Powershell: 
- Uses ‘check’ as it’s keyword
  
      ruff format .  #note:This command formats ALL files in the directory
      ruff format path/to/examplecode.py #this command formats a specific python file (in this case, a python file called “examplecode”)
      ruff format path/to/folder1 #note this command formats all the files in the folder titled “directory1”

### Pre-commit checks - automatic formatting/linting
-  A pre-commit check looks at your code when you commit our code/changes to GitHub and makes alterations to it before you push your code to the GitHub repo. 
-  You can add a pre-commit hook to your GitHub repo that automatically lints and formats your code.
- There is one already set up in this repo, to run it, try:
  
      py -m pip install pre-commit  #you can also try "pre_commit"
    
      py -m pre-commit install

- Check your pre-commit hook by running ‘git commit’, the pre-commit hooks will display a message on your terminal.
##  Github Actions - integrating checks into the repo after a commit
- Github Actions is a platform that allows you to automate your Github repo’s build,test, and deployment pipeline. When an event occurs (ie. When changes are pushed onto a branch), a GitHub Actions workflow will be triggered, and a series of events will run.
- To create a workflow: https://docs.github.com/en/actions/using-workflows/creating-starter-workflows-for-your-organization

## Static analysis with CodeQL manually:
- Static analysis for code is the process of analyzing code for potential errors without needing to running the code. 
- We can perform static analysis using CodeQL to find errors and vulnerabilities in our code.
- CODEQL is a command-line tool that can be used to analyze code. CodeQL can be used to perform the following task:
    - Perform academic research
    - Demonstrate software
    - Test CodeQL queries
      
**Documentation**
- https://docs.github.com/en/code-security/codeql-cli/getting-started-with-the-codeql-cli/setting-up-the-codeql-cli
- https://docs.github.com/en/code-security/codeql-cli/getting-started-with-the-codeql-cli
      
## CodeQL in precommits:
- CodeQL can be integrated with GitHub’s pre-commit hooks. We can combine git’s precommit hooks with CodeQL to format code to CodeQL’s styling guidelines:
    - https://github.com/github/codeql/blob/main/docs/ql-style-guide.md
    - https://github.com/github/codeql/blob/main/docs/pre-commit-hook-setup.md

    

 


            

 

 
