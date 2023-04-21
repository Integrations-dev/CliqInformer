# GitHub Informer for Zoho Cliq
The GitHub Action is used to integrate GitHub and Zoho Cliq, by notifying about the GitHub Events performed, to the Zoho Cliq Channels.

GitHub Informer requires the following inputs to integrate the **GitHub Actions** with your **Cliq** channels
- Cliq Webhook Token
- Cliq Channel API Endpoint or Unique Name
- Individual messages for each of the GitHub events (in the name of **event**-message)
- A default message that you want to send if the message is not specified for that event.
  
## GitHub Secret for Channel Endpoint 🔗
You must add GitHub Secret, which contains the channel endpoint in the format 

```
<Cliq Channel Endpoint>?zapikey=<Cliq Webhook Token>
```

You must create a GitHub Secret for providing the Channel Endpoint  by
  - Go to the Repository where the CliqInformer will be added and go to the '**Settings**' tab.
  - Select '**Secrets and variables**' and click on '**Actions**' in the dropdown.
  - Click on '**New repository secret**' and enter the name of your secret and also enter the Cliq channel endpoint  as the Secret (in above mentioned format)

and use the secret as the '**channel-endpoint**' input in the job of your workflow.

```yaml
  steps:
    - uses: Integrations-dev/GitHub-Informer@v1
      with:
        channel-endpoint: ${{ secrets.SECRET_NAME }}
```

## Custom Event Messages ⚙️

Suppose you need a notification in Cliq for only selected events or actions,
  - you may change the '**_on_**' key of the YAML File where the Action is called.
  - Or additionally, you can set all the required messages and declare the input '**set-message-if-none**' as '**false**' to avoid messages from events you don't want. 
  
You provide default messages to all kind of actions that triggers a workflow.

The messages can be customized by giving the input '**_event_-message**' (where '_event_' is the event for which you would like to customize the message).

For ex: To set a custom message for a Pull Request event, you must define the input as,

```yaml
  pull-request-message: 'A Pull Request has been Opened'
```

## Default Message 📓

If you wish to add a single custom message for all kinds of events, you may use the '**default-message**'. 

For ex: To set a default custom message , you must define the _default-message_ as

```yaml
  default-message: 'A (event) has been (action)'
```

## Shortcuts ⏩

We also provide several shortcuts to obtain the variables that you want to insert in the message, such as,
  - **(event)**: which will be replaced with the event that the workflow is triggered by
  - **(action)**: which will be replaced by the Action the Event is performing with
  - **(me)**: which will be replaced with the GitHub user performing the action.
  - **(repo)**: which will be replaced by the Repository where the GitHub action is performed
  - **(ref)**: which will be replaced by the Branch/Tag where the GitHub action is performed
  - **(workflow)**: which will be replaced by the workflow on which the GitHub Action is performed
  - **(rule)**: which will be replaced by the Branch Protection Rule (if the Event is Branch Protection Rule)
  - **(run)**: which will be replaced by the Check Run (if the Event is Check Run)
  - **(branch)**: which will be replaced by the Branch/Tag which is Created/Deleted (if the Event is Create / Delete)
  - **(deployment)**: which will be replaced by the deployment (if the Event is Deployment or Deployment Status)
  - **(discussion)**: which will be replaced by the discussion which is worked on
  - **(category)**: which will be replaced by the Category Name to which the discussion is changed to
  - **(issue)**: which will be replaced by the issue (if the Event is Issue or Issue Comment)
  - **(label)**: which will be replaced by the label that is being worked on or added to
  - **(milestone)**: which will be replaced by the milestone that is being worked on or added to
  - **(assignee)**: which will be replaced by the User Assigned to the Issue or Pull Request
  - **(pull)**: which will be replaced by the Pull Request that is worked on
  - **(package)**: which will be replaced the Registry Package that is being worked on
  - **(release)**: which will be replaced by the release that is being worked on
  - **(status)**: which will be replaced by the Status of the Event (if the Event is Deployment Status or Status)

Example:

A GitHub Action is triggered by (me) at (repo).

will change to 

A GitHub Action is triggered by [user_name](https://www.github.com/user_name) at [user_name/repository_name](https://www.github.com/user_name/repository_name).

Upon successfully providing the inputs as per criteria, the message will be successfully sent to the Cliq Channel.

The GitHub events that trigger a workflow are listed below, among which all events are supported by GitHub Informer

|    branch_protection_rule    |          check_run          |          check_suite         |            create            |           delete            |
|            :----:            |           :----:            |            :----:            |            :----:            |           :----:            |
| **deployment**               | **deployment_status**       | **discussion**               | **discussion_comment**       | **fork**                    |
| **gollum**                   | **issue_comment**           | **issues**                   | **label**                    | **milestone**               |
| **page_build**               | **public**                  | **pull_request**             | **pull_request_comment**     | **pull_request_review**     |
|**pull_request_review_comment**| **pull_request_target**    | **push**                     | **registry_package**         | **release**                 |
| **repository_dispatch**     | **schedule**                 | **status**                   | **watch**                    | **workflow_dispatch**       |

## Base YAML Code 🗒

Don't worry about remembering a lot of stuff. Here is the minimal code that's required to start with. 

```yaml
name : Communicating with Cliq
on:
  #you may add the events you like to get notified
  push:
    
jobs:
  test_name:
    runs-on: ubuntu-latest
    steps:
      - uses: Integrations-dev/GitHub-Informer@v1
        with:
          channel-endpoint: ${{ secrets.ENDPOINT }}
```

That's all! You will start getting notified for each event occurring in GitHub through the GitHub Action.

Go to the Actions tab of the repository to view the message status.

Here is a Template Repository for the GitHub Informer which you can use as a baseline to work with and customize to your usage.
https://www.github.com/Integrations-dev/RepositoryTemplate
