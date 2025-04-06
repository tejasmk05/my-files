Create new repository, Click on actions, click on simple workflow configure

name: my-workflow
on: workflow_dispatch    (pre-defined workflow event, this will run workflow manually from github)
jobs:
  first-job:
    runs-on: ubuntu-latest    (pre-defined github runners)
    steps:
    - name: Print greeting
      run: echo "Hello Tejas"
    - name: Print good bye
      run: echo "Byee Tejas"
  
1. Events
Events in GitHub Actions refer to specific activities or triggers that occur in your GitHub repository. These events tell GitHub Actions when to start running a workflow. Events are actions that occur on GitHub, such as pushing code, opening an issue, or creating a pull request.
Common GitHub Events:
	• push: Triggered when code is pushed to a branch or tag.
	• pull_request: Triggered when a pull request is opened, synchronized, or closed.
	• workflow_dispatch: Manually triggered by the user via the GitHub interface
	• Etc:

2. Actions
Actions are reusable units of code that perform a specific task in a GitHub Actions workflow. An action can be a single step, such as checking out code, setting up a programming environment, or deploying code to a server. Actions are like "building blocks" of workflows.

Common GitHub Actions Examples:
	• actions/checkout: Checks out your repository’s code so that the workflow can work with it.
	• actions/setup-node: Sets up a Node.js environment.
	• Etc:



To deploy from local, first open code in vscode, create below folder structure



Jobs to Run parallelly:

name: my-workflow
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: checkout code
        uses: actions/checkout@v4
      - name: Installing node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - run: npm ci
      - run: npm test
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: checkout code
        uses: actions/checkout@v4
      - name: Installing node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Installing dependencies
        run: npm ci
      - name: build Project
        run: npm run build


Jobs run in sequentially:
Just we need to add one line, needs: test(other dependent job name)


name: my-workflow
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: checkout code
        uses: actions/checkout@v4
      - name: Installing node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - run: npm ci
      - run: npm test
  deploy:
    needs: test                  #Here needs refer another job need to be completed, test refer other job name.
    runs-on: ubuntu-latest
    steps:
      - name: checkout code
        uses: actions/checkout@v4
      - name: Installing node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Installing dependencies
        run: npm ci
      - name: build Project
        run: npm run build



Multiple Events:
In below example if we push the code then workflow wil re-trigger and also in UI we will get option to run workflow.
name: my-workflow
on: [push, workflow_dispatch]
jobs:
…
…

Expression and context:

	- name: output
	Run: echo "${{toJSON(github.event)}}"
	
This GitHub Actions step prints the entire github context as a JSON string. Here's a breakdown:
	• - name: output: Names the step as "output."
	• Run: echo "${{toJSON(github)}}": 
		○ github: A predefined context in GitHub Actions that contains metadata about the workflow run (e.g., repository info, actor, event name).
![image](https://github.com/user-attachments/assets/163b284a-5c1d-41e1-9bbc-76656690a5eb)
