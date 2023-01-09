# CapstoneProject-Lists-Functions-StringHandling
A revamp Python program from a previous capstone project, will read data from two text files called user.txt and tasks.txt will perform different operations to allow a small business to manage tasks assigned to each member of the team. This program has been modified to use functions and validations to improve modularity and prevent human error
## Installation
You will need PyCharm to run this project.\
Copy the files task_manager.py, tasks.txt and user.txt to the root file of your new project in PyCharm
## Usage
For the development of this software, datetime inport, 6 functions, and a menu were created. \
The components will be described below highlighting their purposes.
### reg_user function
```Python
def reg_user(logged_u):
    # while loop that will run as long as a condition is met and brakes out
    while True:

        # conditional statement to check if user is admin.
        if logged_u == "admin":

            # if user is admin, will be allowed to register new user. User input is converted to lower case
            # and no blank spaces.
            new_username = input("Enter the new username: ").lower().strip()
            new_password = input("Assign a password: ").lower().strip()
            confirm_password = input("Confirm password: ").lower().strip()

            # conditional statement to check if admin user has confirmed password entered, and print
            # the appropriate message
            if new_password != confirm_password:

                print("\nPasswords do not match.\n"
                      "Please try again from the beginning!!!\n")

            # conditional statement to check that admin user is not registering the same user twice,
            # and print the appropriate message
            elif new_username in user_credentials_dic:

                print("\nSorry...\n"
                      "User already registered!!!\n"
                      "Please try again from the beginning!!!\n")

            else:

                # if password is confirmed, print a message and write the data entered on user.txt file
                # on a new line
                print("\nNew user Registered!!!\n")
                with open('user.txt', 'a') as file:
                    file.write(str("\n" + new_username) + ", " + confirm_password)

                break

        else:

            # if user is not 'admin' print an appropriate message
            print("\nSorry, you need to log in as 'admin' to register new users!!!\n")

            break
```
### add_task function
```Python
def add_task(task_for_user):
    # conditional statement to check if user input is a key in dictionary
    if task_for_user in user_credentials_dic:

        # user input is store in a variable and converted to a title
        task_title = input("Enter the title of the task: ").title()

        # user input is store in a variable
        task_description = input("Describe the task been assigned: ")

        # user input will be converted to the correct date format, using date library
        task_due_date = input("Task due date dd/mm/yyyy: ")
        task_due_date = datetime.datetime.strptime(task_due_date, "%d/%m/%Y").strftime("%d %b %Y")

        # current system date will be obtained and converted to a user-friendly format using
        # datetime library
        current_date = date.today()
        task_current_date = current_date.strftime("%d %B %Y")

        # user input is store in a variable
        task_completion = "No"

        # print a message and write the data entered on tasks.txt file
        # on a new line
        print("\nNew task created!!!\n")
        with open('tasks.txt', 'a') as file:
            file.write(str("\n" + person_assigned_to_task) + ", " + task_title + ", " + task_description + ", " +
                       task_current_date + ", " + task_due_date + ", " + task_completion)

    else:
        # if user input not in dictionary, print an appropriate message and shows all available usernames
        print("\nSorry...\n"
              "This user is not register in our system!!!\n"
              "Tasks can only be assigned to the below users\n"
              f"{', '.join(user_credentials_dic.keys())}\n")
```
### view_all function
```Python
def view_all():
    #  opens and read tasks.txt file. For loop to iterate each line in the file and split it where a comma ',' is
    #  print an appropriate user-friendly message with the contents of tasks.txt file
    with open('./tasks.txt', 'r') as file:
        for num, lines in enumerate(file, start=1):
            tasks_list = lines.split(", ")

            print(f"""{'_' * 100}\n
Task number:       # {num}
Task:              {tasks_list[1]}
Assigned to:       {tasks_list[0].title()}
Date assigned:     {tasks_list[3]}
Due date:          {tasks_list[4]}
Task completed?:   {tasks_list[5]}
Task description:   
{tasks_list[2]}             
{'_' * 100}\n
""")
```
### view_mine function
```Python
def view_mine():
    # opens and read tasks.txt file. For loop to iterate each line in the file and split it where a comma ',' is
    # print all tasks assigned to the user in session in an appropriate user-friendly format
    with open('./tasks.txt', 'r') as file:
        no_task = 0
        task_num_list = []
        for num, lines in enumerate(file, start=1):
            tasks_list = lines.split(", ")

            # conditional statement that will print the tasks only for the user in session if any
            if user_session == tasks_list[0]:
                task_num_list.append(num)
                no_task += 1
                print(f"""{'_' * 100}\n
Task number:       # {num}
Task:              {tasks_list[1]}
Assigned to:       {tasks_list[0].title()}
Date assigned:     {tasks_list[3]}
Due date:          {tasks_list[4]}
Task completed?:   {tasks_list[5]}
Task description:   
{tasks_list[2]}             
{'_' * 100}\n
""")
        else:
            # if no tasks assigned an appropriate message will be displayed
            if no_task == 0:
                print("\nWe could not find any tasks assigned to you... yet!!!\n")
            else:

                try:

                    # while loop that will run as long as a condition is met and brakes out
                    while True:
                        print("\nSelect a task for editing by entering the task number\n"
                              "Enter -1 to return to main menu")
                        # take user input and store it in variable
                        task_number = int(input(">> "))
                        # conditional statement to check if user input is in user task list, else if user input is '-1'
                        # loop will break to previous menu
                        if task_number in task_num_list:

                            #  opens and read tasks.txt file, read all lines, split it where a comma ',' and strip any
                            #  blank spaces
                            #  print an appropriate user-friendly message with two options that will take user input and
                            #  store it in variable
                            with open('./tasks.txt', 'r') as fi:
                                task_number -= 1
                                content_task = fi.readlines()
                                edit_task = content_task[task_number].strip().split(", ")

                                print("\nWhat would you like to do?\n"
                                      "A) Mark task as completed?\n"
                                      "B) Edit the task?")
                                task_edition = input(">> ").lower()

                                # conditional statement to check if user selected input 'a' or 'b'
                                if task_edition == "a":

                                    # conditional statement that check if selected task has been completed or not
                                    # print an appropriate user-friendly message if task cannot be changed
                                    if edit_task[5].lower() == "no":

                                        # task will change to completed ('Yes') and will save the new information
                                        # in tasks.txt file
                                        # print an appropriate user-friendly message confirming action
                                        edit_task[5] = "Yes"
                                        content_task[task_number] = \
                                            edit_task[0] + ", " + edit_task[1] + ", " + edit_task[2] + ", " + \
                                            edit_task[3] + ", " + edit_task[4] + ", " + edit_task[5] + "\n"
                                        with open('./tasks.txt', 'w') as ft:
                                            ft.writelines(content_task)
                                        print("Task marked as completed!!!")

                                    else:

                                        print("\nTask cannot be changed as it was marked as completed!")

                                elif task_edition == "b":

                                    # conditional statement that check if selected task has been completed or not
                                    # print an appropriate user-friendly message if task cannot be changed
                                    if edit_task[5].lower() == "no":

                                        #  print an appropriate user-friendly message with two options that will
                                        #  take user input and store in variable
                                        print("\nWhat would you like to do?\n"
                                              "A) Change name of the person assigned to task?\n"
                                              "B) Change due date?")
                                        task_edit_name_or_date = input(">> ").lower()

                                        # conditional statement to check if user selected input 'a' or 'b'
                                        if task_edit_name_or_date == "a":

                                            # take user input and store it in a variable, the name of the user assigned
                                            # to task will be replaced and the new information will be saved
                                            # in tasks.txt file
                                            # print an appropriate user-friendly message confirming action
                                            new_name = input("Enter new name: ")
                                            edit_task[0] = new_name
                                            content_task[task_number] = \
                                                edit_task[0] + ", " + edit_task[1] + ", " + edit_task[2] + ", " + \
                                                edit_task[3] + ", " + edit_task[4] + ", " + edit_task[5] + "\n"
                                            with open('./tasks.txt', 'w') as ft:
                                                ft.writelines(content_task)
                                            print("\nName successfully changed!!!")

                                        elif task_edit_name_or_date == "b":

                                            # take user input and store it in a variable, the due date will be replaced
                                            # and the new information will be saved in tasks.txt file
                                            # print an appropriate user-friendly message confirming action
                                            # user input will be converted to the correct date format,
                                            # using date library
                                            new_due_date = input("Task due date dd/mm/yyyy: ")
                                            new_due_date = datetime.datetime.strptime(new_due_date,
                                                                                      "%d/%m/%Y").strftime("%d %b %Y")
                                            edit_task[4] = new_due_date
                                            content_task[task_number] = \
                                                edit_task[0] + ", " + edit_task[1] + ", " + edit_task[2] + ", " + \
                                                edit_task[3] + ", " + edit_task[4] + ", " + edit_task[5] + "\n"
                                            with open('./tasks.txt', 'w') as ft:
                                                ft.writelines(content_task)
                                            print("\nDue date successfully changed!!!")

                                        else:

                                            # print an appropriate user-friendly message if wrong option selected
                                            print("\nPlease select 'A' or 'B'\n"
                                                  "Try again!")

                                    else:

                                        print("\nTask cannot be changed as it was marked as completed!")

                                else:

                                    # print an appropriate user-friendly message if wrong option selected
                                    print("\nPlease select 'A' or 'B'\n"
                                          "Try again!")

                        elif task_number == -1:
                            break

                except ValueError as e:
                    print("\nPlease select a task number only!!!\n"
                          "Try again!\n")
```
### view_statistics function
```Python
def view_statistics():
    # calling of function 'generate reports' to generate reports if non-existent
    generate_reports()

    # opens and read task_overview.txt file, and prints it in a user-friendly format the report
    with open('./task_overview.txt', 'r') as file:
        for lines in file:
            print(lines)
    print()

    # opens and read user_overview.txt file, and prints it in a user-friendly format the report
    with open('user_overview.txt', 'r') as file:
        for lines in file:
            print(lines)
    print()
```
### generate_reports function
```Python
def generate_reports():
    # creation of variables with initialized value '0'
    task_count = 0
    completed_task = 0
    uncompleted_tasks = 0
    overdue_uncompleted_tasks = 0
    users_count = 0

    # opens and read tasks.txt file, and count all lines in the file
    # strip blank spaces and split each line in the file
    # get current date and string format due date in file
    with open('./tasks.txt', 'r') as f_task:
        for l_task in f_task:
            task_count += 1
            tasks = l_task.strip().split(", ")
            current_date = datetime.datetime.today()
            due_date = datetime.datetime.strptime(tasks[4], "%d %b %Y")

            # conditional statement to check and count tasks if they have been completed or not
            if tasks[5] == "Yes":
                completed_task += 1
            elif tasks[5] == "No":
                uncompleted_tasks += 1

            # conditional statement to check and count tasks that are overdue and have not been completed
            if tasks[5] == "No" and current_date > due_date:
                overdue_uncompleted_tasks += 1

    # declaring two variables that will store the percentage of task uncompleted and overdue
    percent_task_uncompleted = (uncompleted_tasks / task_count) * 100
    percent_task_overdue = (overdue_uncompleted_tasks / task_count) * 100

    # opens and reads task overview file if exist or create one if non-existent
    # write task report on the file in a user-friendly format
    with open('./task_overview.txt', 'w') as file_task_overview:
        file_task_overview.write(str("Tasks Report\n" + '=' * 50 + "\n")
                                 + "Total number of tasks: " + str(task_count) + "\n" +
                                 "Total number of completed tasks: " + str(completed_task) + "\n" +
                                 "Total number of uncompleted tasks: " + str(uncompleted_tasks) + "\n" +
                                 "Total number of uncompleted and overdue tasks: " + str(overdue_uncompleted_tasks)
                                 + "\n" "Percentage of tasks that are incomplete: " + str(percent_task_uncompleted)
                                 + "\n" "Percentage of tasks that are overdue: " + str(percent_task_overdue) + "\n"
                                 + '=' * 50 + "\n")

    # opens and read user.txt file, and count all lines in the file
    # declare a variable for a list
    # strip blank spaces and split each line in the file
    # append to list all users in file
    with open('./user.txt', 'r') as file:
        user_list = []
        for lines in file:
            users_count += 1
            users = lines.strip().split(", ")
            user_list.append(users[0])

    # opens and read tasks.txt file
    # declare a variables for lists
    # strip blank spaces and split each line in the file
    # append to list all users that have one or more assigned tasks in file
    # get current date and string format due date in file
    with open('./tasks.txt', 'r') as f_task:

        list_task_per_user = []
        list_task_per_user_uncompleted = []
        list_task_per_user_completed = []
        list_task_per_user_uncompleted_over = []
        for l_task in f_task:
            tasks = l_task.strip().split(", ")
            list_task_per_user.append(tasks[0])
            current_date = datetime.datetime.today()
            due_date = datetime.datetime.strptime(tasks[4], "%d %b %Y")

            # conditional statement to check if tasks have been completed or not
            # append to a list the users that have uncompleted/completed tasks
            if tasks[5] == "No":
                list_task_per_user_uncompleted.append(tasks[0])
            elif tasks[5] == "Yes":
                list_task_per_user_completed.append(tasks[0])

            # conditional statement to check if tasks are overdue and have not been completed
            # append to a list the users that have overdue and uncompleted tasks
            if tasks[5] == "No" and current_date > due_date:
                list_task_per_user_uncompleted_over.append(tasks[0])

        # declaring variables for dictionaries
        dict_task_per_user = {}
        dict_task_per_user_uncompleted = {}
        dict_task_per_user_completed = {}
        dict_task_per_user_uncompleted_over = {}

        # for loop to iterate all users registered
        # create dictionaries that will store the tasks assigned to each user, uncompleted tasks for each user,
        # completed tasks for each user and overdue and uncompleted tasks for each user
        for i in user_list:
            dict_task_per_user[i] = list_task_per_user.count(i)
            dict_task_per_user_uncompleted[i] = list_task_per_user_uncompleted.count(i)
            dict_task_per_user_completed[i] = list_task_per_user_completed.count(i)
            dict_task_per_user_uncompleted_over[i] = list_task_per_user_uncompleted_over.count(i)

        # declare a list variable to store the number of tasks assigned per user,
        # for loop to iterate the values of dictionary and append these to the list
        num_tasks_assigned = []
        for val1 in dict_task_per_user.values():
            num_tasks_assigned.append(str(val1))

        # declare a list variable to store the number of uncompleted tasks assigned per user,
        # for loop to iterate the values of dictionary and append these to the list
        num_tasks_assigned_uncompleted = []
        for val in dict_task_per_user_uncompleted.values():
            num_tasks_assigned_uncompleted.append(str(val))

        # declare a list variable to store the number of completed tasks assigned per user,
        # for loop to iterate the values of dictionary and append these to the list
        num_tasks_assigned_completed = []
        for val in dict_task_per_user_completed.values():
            num_tasks_assigned_completed.append(str(val))

        # declare a list variable to store the number of uncompleted and overdue tasks assigned per user,
        # for loop to iterate the values of dictionary and append these to the list
        num_tasks_assigned_uncompleted_over = []
        for val in dict_task_per_user_uncompleted_over.values():
            num_tasks_assigned_uncompleted_over.append(str(val))

        # declaring a dict variable that will store the percentage of tasks assigned to users
        percent_task_assigned = {key: value / task_count * 100 for key, value in dict_task_per_user.items()}

        # declaring a dict variable that will store the percentage of uncompleted tasks assigned to users
        percent_task_assigned_uncompleted = \
            {key: value / task_count * 100 for key, value in dict_task_per_user_uncompleted.items()}

        # declaring a dict variable that will store the percentage of completed tasks assigned to user
        percent_task_assigned_completed = \
            {key: value / task_count * 100 for key, value in dict_task_per_user_completed.items()}

        # declaring a dict variable that will store the percentage of uncompleted and overdue tasks assigned to user
        percent_task_assigned_uncompleted_over = \
            {key: value / task_count * 100 for key, value in dict_task_per_user_uncompleted_over.items()}

        # declare variables for lists
        percent_t_assigned = []
        percent_t_assigned_uncompleted = []
        percent_t_assigned_completed = []
        percent_t_assigned_uncompleted_over = []

        # for loop to iterate the values of the previous dictionaries and append these to the lists as strings
        # values rounded using round() function
        for val_percent in percent_task_assigned.values():
            percent_t_assigned.append(str(round(val_percent)))
        for val_percent in percent_task_assigned_uncompleted.values():
            percent_t_assigned_uncompleted.append(str(round(val_percent)))
        for val_percent in percent_task_assigned_completed.values():
            percent_t_assigned_completed.append(str(round(val_percent)))
        for val_percent in percent_task_assigned_uncompleted_over.values():
            percent_t_assigned_uncompleted_over.append(str(round(val_percent)))

    # opens and reads user overview file if exist or create one if non-existent
    # write user report on the file in a user-friendly format
    # print appropriate message to informed user that reports were generated
    with open('./user_overview.txt', 'w') as file_user_overview:
        file_user_overview.write(str("Users Report\n" + '=' * 100 + "\n")
                                 + "Total number of registered users: " + str(users_count) + "\n" +
                                 "Total number of tasks: " + str(task_count) + "\n" +
                                 "Users: " + " -  ".join(user_list).title() + "\n" +
                                 "NTA:    " + "         ".join(num_tasks_assigned) + "\n" +
                                 "PTA:   %" + "       %".join(percent_t_assigned) + "\n" +
                                 "PTAC:  %" + "        %".join(percent_t_assigned_completed) + "\n" +
                                 "PTAU:  %" + "       %".join(percent_t_assigned_uncompleted) + "\n" +
                                 "PTAUO: %" + "       %".join(percent_t_assigned_uncompleted_over) + "\n"
                                 + '=' * 100 + "\n" +
                                 "Acronym definitions:\n" +
                                 "NTA   = number of tasks assigned\n" +
                                 "PTA   = percentage of tasks assigned\n" +
                                 "PTAC  = percentage of tasks assigned completed\n" +
                                 "PTAU  = percentage of tasks assigned uncompleted\n" +
                                 "PTAUO = percentage of tasks assigned uncompleted and overdue\n"
                                 + '=' * 100 + "\n")
    print()
    print("Task report was generated")
    print("User report was generated")
    print()
```
## Contributing
Pull requests are welcome. For any changes, please open an issue first to discuss what you would like to change.
Please make sure to update tests as appropriate.
## Credits
© Angel De Sousa
