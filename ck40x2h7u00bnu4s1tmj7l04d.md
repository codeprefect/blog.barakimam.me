## VS Code 101 for Remote Developers

I started using Visual Studio Code around 2016, while it was not a smooth experience at first, VS Code has managed to endear itself to the heart of developers, especially the polyglots.

## Top VS Code Extensions for Remote Teams:

Irrespective of what technologies your remote team uses, there are numerous VS Code plugins you will find useful, but this article will only cover those I have used extensively.

### [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)

Live Share Extension Pack is a suite of four awesome extensions which include the following;

#### [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)

![Live Share](https://visualstudio.microsoft.com/wp-content/uploads/2018/11/liveshare-hero-optimized.jpg)

LiveShare is Microsoft's solution to enable a truly collaborative development experience. It supports real-time collaborative editing for multiple users in a single workspace (I have had up to six users on a session before).
Being an early adopter of Live Share, the stability and improvements we have witnessed are quite impressive.
LiveShare has given me that privilege to have memorable remote pair programming sessions with many members of my team spread across the continent with unbelievable ease.

Some amazing feature of Live Share that makes it so awesome include
        
- Shared Terminal
- Shared Localhost
- and even a shared debugging experience.

#### [Live Share Chat](https://marketplace.visualstudio.com/items?itemName=karigari.chat)

Communication is key, especially for distributed teams.
How do you provide immediate feedback on the function your colleague just refactored. Here comes Live Share Chat

![Live Share Chat](https://raw.githubusercontent.com/karigari/vscode-chat/master/readme/Live%20Share%20Chat.gif)

Live Share Chat provides you with a chat interface that allows you to have realtime communication will all connected users in a Live Share session.

#### [Live Share Audio](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-audio)

Live Share Audio gives you the capability to make a conference call with every user in a session. It comes with exciting features like the ability to mute and unmute yourself. With Live Share Audio, Guests who join your session are automatically prompted to join the call, and right in the extension tab, you can see a list of all the people in the call, their mute status, and a button icon to mute yourself or otherwise.

![Live Share Audio](https://user-images.githubusercontent.com/116461/49332032-0c2e7380-f55b-11e8-8e77-a53013f689e3.png)


### [Live Share WhiteBoard](https://marketplace.visualstudio.com/items?itemName=lostintangent.vsls-whiteboard)

Sometimes during pair programming sessions, you feel the need to sketch some things, maybe to provide more clarity to your partner.

Live Share has got you covered with this extension.

![Live Share WhiteBoard](https://user-images.githubusercontent.com/116461/50567457-dddaba00-0cf9-11e9-840b-1b0a984d5ad9.gif)

### [Github Pull Request](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)

GitHub pull-request has been around for a long time but recently received massive support after Microsoft acquired Github. This extension is a great productivity hack as it allows you to review PRs right inside your favorite code editor.

GitHub PR brings the entire Github web experience into VS Code, you can do basically everything, except the new multi-line comments which is being tracked [here](https://github.com/microsoft/vscode-pull-request-github/issues/1376).

We have been using Github web for years without this feature, so I don't believe it would be a blocker to adopting this extension

![GitHub PR](https://github.com/Microsoft/vscode-pull-request-github/raw/master/.readme/demo.gif)

### [JIRA and Bitbucket (Official)](https://marketplace.visualstudio.com/items?itemName=Atlassian.atlascode)

If your team is covered by an Atlassian subscription, and you are using any of JIRA cloud and/or BitBucket cloud, this extension has got you covered.

The JIRA part provides you with a lot of features to manage your tasks from inside VS Code, the powerful JQL language also provides you the opportunity to create a near web experience for yourself.

![JIRA and BitBucket Official](https://bitbucket.org/atlassianlabs/atlascode/raw/master/.readme/dev-workflow.gif)

The BicBucket part, on the other hand, provides functionalities similar to what is obtainable on the GitHub PR extensions, and even more. One of the notable additional features is an opportunity to monitor your BitBucket pipeline right inside VS Code.

### [Browser Preview](https://marketplace.visualstudio.com/items?itemName=auchenberg.vscode-browser-preview)

Built on top of the same Headless Chrome that powers Cypress, Browser Preview gives you an almost full browser right inside VS Code, it comes handy when you need to get immediate feedback from front-end code changes.
With recent updates, it brings debugging support.

![Browser Preview](https://github.com/auchenberg/vscode-browser-preview/raw/master/resources/demo.gif)


## Best Practices for a great VS Code Experience

One of the issues developers often face with VS Code is high memory usage, this is due to the fact that VS Code is built on Electron, this can often make VS Code unusable and sometimes your entire machine may be impacted. Here are ways I have been able to avoid sad experiences with VS Code.

### Use Workspaces

Workspaces give you the opportunity to customize your VS Code experience, for a particular project, or set of projects. It gives you the opportunity to set workspace specific settings and even extensions.

If you had ever experience memory issues, and extension conflict, workspace gives you the flexibility to enable just the extensions you want.

### Disable Unused Extensions

Many developers work with different technologies, and may usually have some technology particular extensions.
You can improve your VS Code experience by disabling all extensions that do not relate to any project in your current workspace and/or folder. One method I employed to achieve this is by disabling all extensions by default, and only enable those extensions that are usable in the current project.

### One Extension Per Feature

Have you ever run into a scenario where ESLint removed the brackets around an anonymous function, and prettier added it back immediately. This is what happens when you have multiple extensions for the same set of needs.


> Work on your team's workflow, decide which set of rules you are adopting, and get the extensions necessary to enforce those rules.


VS Code was created to be lightweight, but installing too many extensions without proper separation of concern may bring you close to a Visual Studio experience.

It is almost a year I have incorporated these practices into my workflow, and I am pretty happy with this experience.

Thank you for reading, and kindly share if you feel the need.

Thanks.