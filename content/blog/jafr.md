---
title: "Adding, Listing and Managing Tasks with Python (JAFR)"
description: "How to create a terminal-based app that adds, lists and manages tasks with Python (JAFR)"
dateString: Aug 2023
draft: false
tags: ["Python", "Automation", "Bash", "Python programming"]
weight: 101
cover:
    image: "/blog/jafr/jafr.jpg"
---
# Introduction
During my escapades on Upwork, I stumbled upon a task that required me to create a basic application called Jafr (short for “Just a Friendly Reminder”) that would help various users manage their tasks and meetings on a Unix OS. It allows users to interact with it by typing commands in a command-line interface and assumes that all the tasks and meetings are stored in text files managed by the user.

# Methodology
Jafr is implemented in Python and comes with a simple start-up script in Bash. Additionally, end-to-end tests for Jafr are included to ensure that everything is working as expected.

The behaviour of Jafr is very straightforward. Users can add, delete, and view their tasks and meetings using the appropriate commands. Jafr also allows users to set reminders for their tasks and meetings.

To ensure that Jafr is working correctly, there are also error handling mechanisms in place, as well as instructions on how to write tests for the application. Let’s dive in on its behavior.

# JAFR Behavior

The application has the following basic capabilities:

Adding a Task
```python
$ jafr add "Finish project proposal" 2023-09-10
Task added: Finish project proposal by 2023-09-10
```

Listing tasks
```python
$ jafr list
1. [ ] Finish project proposal (2023-09-10)
```
Marking tasks as done
```python
$ jafr meet "Team meeting" 2023-09-15 14:30
Meeting added: Team meeting on 2023-09-15 at 14:30
```
Listing Meetings
```python
$ jafr meet list
1. [ ] Team meeting (2023-09-15 at 14:30)
```
# Bash Startup Script

Create a Bash script named ```jafr.sh``` to provide a convenient way to run Jafr from the command line. Make the script executable first by using ```chmod +x jafr.sh``` and then execute it.

```bash
#!/bin/bash
python /path/to/your/jafr.py "$@"
```
# I/O End-to-End Tests:

You can write tests to ensure Jafr behaves correctly. Consider using a testing framework like ```pytest```. Create a file named ```test_jafr.py``` for your tests. Remember to install pytest using ```pip install pytest```, and then run your tests with ```pytest test_jafr.py```

```python
from subprocess import Popen, PIPE

def test_add_task():
    process = Popen(["./jafr.sh", "add", "Finish project proposal", "2023-09-10"], stdout=PIPE, text=True)
    output = process.communicate()[0]
    assert "Task added" in output

# More test functions for other commands...

if __name__ == "__main__":
    test_add_task()
    # Call other test functions here
```
# Error Handling and Additional Features:

Implement error handling for cases like missing arguments, invalid dates/times, etc. You can also enhance Jafr by allowing users to edit existing tasks/meetings, delete tasks/meetings, and search for tasks/meetings.

The full source code can be found on my Github page and note that the provided code snippets are basic templates to get you started. Additionally, the code should be extended and refined based on your specific requirements and design choices.

Happy learning 😀