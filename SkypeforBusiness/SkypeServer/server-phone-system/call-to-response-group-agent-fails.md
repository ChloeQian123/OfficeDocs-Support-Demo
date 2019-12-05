---
title: Call to Response Group Agent fails
description: Provides a solution for call failure to Response Group Agent.
author: simonxjx
manager: dcscontentpm
localization_priority: Normal
search.appverid: 
- MET150
audience: ITPro
ms.prod: skype-for-business-itpro
ms.topic: article
ms.author: v-six
ms.reviewer: miadkins
ms.custom: CSSTroubleshoot
appliesto:
- Microsoft Lync Server
---

# Call to Microsoft Lync Server Response Group Agent fails

## Symptoms

Consider the following scenario. A Lync user makes a call to a Lync Server Response Group agent who is listed on his or her Lync contact list. However, the Lync Server Response Group agent does not receive the call as expected. 

## Cause

This issue occurs if one or more of the following conditions are true: 
- The Lync Server-enabled user account for the Lync Server Response Group agent was assigned to a different SIP domain that is registered with the Lync Server pool or server.   
- The update to the Lync Server Response Group agent SIP URI did not occur in the Lync Server response groups that the Lync Server Response Group agent is a member of.   

## Resolution

To resolve this issue, follow these steps: 

1. Use the Lync Server Control Panel to access the **Response Groups** menu option.   
2. On the **Response Group** menu, click **Group**.   
3. Select the specific Lync Server response group that the updated Response Group agent is a member of. 
    > [!NOTE]
    > The Lync Server Response Group agent can belong to more than one Lync Server response group.
4. On the menu bar, click **Edit Details**.   
5. Scroll down to locate the original SIP URI for the Lync Server Response Group agent in the **Agent** list.   
6. Select the specified Lync Server Response Group agent, and then click the **Remove** option on the menu bar.   
7. On the top-most menu bar, click the **Commit** option.   
8. Repeat steps 3 and 4.    
9. Scroll down to locate the Lync Server **Response Group Agent** list.   
10. Click **Select** on the menu bar.   
11. In the **Select Agents** dialog box, use the Find feature to locate the Lync Server -enabled user who will be re-added to the selected Lync Server response group.   
12. Select the agent that has the correct SIP URI, and then click **OK**.   
13. On the top-most menu bar, click **Commit**.   
14. At the top of the Response Group pane, click **Refresh**.   
15. Sign out and then sign back in to the Lync clients that are experiencing the issue.   
16. Make a call to a Lync Server Response Group agent from the Lync client contact list.   

## More Information

By default, the following error is generated by the application:

```adoc
Windows Powershell returned the following errors: Cannot insert duplicate key row in object dbo.Agents with unique index IX_Agents_UserSid. 
The statement has been terminated.
```

This error occurs because the removed Lync Server response group agent information has been marked for deletion in the dbo.Agents table of the rgsconfig database. There is some latency associated with the deletion process for the removed Lync Server response group agent record in the dbo.Agents table of the rgsconfig database. Only after the row that contains the unwanted Lync Server response group agent information is completely removed from the dbo.Agents table the Lync Server Response Group Agent Group commit process can be completed.

Therefore, the behavior that is described in the "Symptoms" section is by design.