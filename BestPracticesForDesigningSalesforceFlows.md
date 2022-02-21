# Best practices for developing Salesforce Flows
## A. Prerequisites - before you start using your Salesforce Flow:
1.	Analyze requirements
What is the business case? Which are the specific use cases? Think critically and look outside of the box. Hence do not be afraid to ask your client many questions to get a clear and complete understanding.
2.	Write User Stories
Identify all user stories and write them down in the format (2 sentences maximum): 
*As (a user) I want to (do something) by (achieving something).*
In this way you make sure to identify the role(s), describe the functionality and explain the reason. Also write down all corresponding user acceptance criteria.
3.	Design your Flow
Write each step of the flow in pseudocode before attempting to implement the flow itself. It is easier to do changes at this stage and breakdown the flow into chunks and possible subflows. For a better understanding you should also design a process by using BPMN technique to visualize the Big Picture. This may be one example for an After-Save-Flow:
## B. Implementation – now, it is time to work in your Salesforce Flow:
1.	Set Start Node entry conditions
Set entry conditions wisely in your Start Node. Be as specific as possible to minimize the occasions to run the flow if it is triggered for example. It should not run if it is not necessary for your use case. Also have a good understanding of your Salesforce org and corresponding automations. Since the Spring 2022 release you can open the Flow Trigger Explorer to identify interdependencies between other flows. Always be familiar and aware of the typical Salesforce [order of execution](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm).
2.	Save your Flow
It is really annoying if you have forgotten to save your flow and your work has disappeared all of a sudden. Yes, it has happened to me already. So after the first configurative steps you want to save the Flow. Think about a good naming for your Flow. Other Administrators should know right away what the Flow does. Here is one example: ‘Set the country-specific entitlement processes and record types’.
