# Best practices for developing Salesforce Flows
## A. Prerequisites – before you start building your Salesforce Flow:
###	1. Analyze requirements
What is the business case? Which are the specific use cases? Think critically and look outside of the box. Hence do not be afraid to ask your client many questions to get a clear and complete understanding.
### 2.	Write User Stories
Identify all user stories and write them down in the format (2 sentences maximum): 
*As (a user) I want to (do something) by (achieving something).*
In this way you make sure to identify the role(s), describe the functionality and explain the reason. Also write down all corresponding user acceptance criteria.
### 3.	Design your Flow
Write each step of the flow in pseudocode before attempting to implement the flow itself. It is easier to do changes at this stage and breakdown the flow into chunks and possible subflows. For a better understanding you should also design a process by using BPMN technique to visualize the Big Picture. This may be one example for an After-Save-Flow:
## B. Implementation – now, it is time to work in your Salesforce Flow:
### 1. Build your Flow in a test environment
Always set up a test environment for developing Flows. This can be a Developer Edition, a sandbox or scratch org. This seperate environment ensures that your development does not do any harm to your productive environment of course. That way, you can insert fake data and test various use cases without changing or deleting valuable data of your users. This saves you a lot of trouble in daily business. Your users will be grateful ;-).
### 2.	Set Start Node entry conditions
Set entry conditions wisely in your Start Node. Be as specific as possible to minimize the occasions to run the flow if it is triggered for example. It should not run if it is not necessary for your use case. Also have a good understanding of your Salesforce org and corresponding automations. Since the Spring 2022 release you can open the Flow Trigger Explorer to identify interdependencies between other flows. Always be familiar and aware of the typical Salesforce [order of execution](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm).
### 3.	Save your Flow early and often
It is really annoying if you have forgotten to save your flow and your work has disappeared all of a sudden. Yes, it has happened to me already. So after the first configurative steps you want to save the Flow. Think about a good naming for your Flow. Other Administrators should know right away what the Flow does. Here is one example: ‘Set the country-specific entitlement processes and record types’.
### 4.	Test while building your Flow
Save as you go then use the run and debug function to provide example input to see each scenario executes as expected. By testing early and testing often, you maintain a working flow that you can restore and branch off from to test other scenarios. Think about validating user inputs before passing them to the flow. What input data are required? By reflecting and validating your input data, missing objects are being detected and misconfiguration can be prevented.
### 5.	Arrange your Flow
Try to align similar elements vertically and have your flow run from left to right and top to bottom. The flow should almost read like a sentence or a paragraph. Do not be afraid to use explicit names for the elements to describe what they do.
### 6. Find a good naming convention (for all team members)
In your Flow you use variables and elements among others. Come up with a good and easy naming convention for all of your team members. There is no right or wrong, as long as you keep your naming consistent and readable. For example it is very common to capture records by using recordId variables. Normally you set up a variable of datatype 'text', you name it 'recordId' and you activate this variable for input data. Other variables can be named by using the prefix 'var', followed by the type of variable (T=>text, R-> single record, Coll=> collection or list) and the function of the variable. For example, a variable named 'varRColl_OpposStatusClosed' tells you that it is a list of Opportunity records of status = closed. We could have skipped the word status as well because it is almost clear that Closed refers to Status. Here are other examples:
- varR_CustomerInputDate
- formula_TODAY
- sObjCollection_QueriedCases
After all it is up to you how to describe them as long as you are consistent and clear. So take some time for finding a helpful name when you create new elements and variables.
### 7.	Document your Flows
Apart from finding a good naming convention for your Flows, please make sure to document your Flows in the main description and preferably in all element descriptions. In the main description include what your Flow does, which objects your Flow touches and where it is invoked from: Which layout is used for Screen Flows? Which other automation processes invoke your Flow and will trigger other automations? So while building or editing your flow, ensure to write down a meaningful description because it is inevitable that your work will be viewed by other Administrators. Write what you do in short sentences and WHY you do it. Especially the reasons for your flow can be very helpful when debugging it. This easy task reflects your good work quality instantly. Documentation increases transparency and avoids misunderstanding. Additionally it saves time for everybody, even yourself, when you need to do any adjustments or bugfixing in the future.
### 8.	Consider Using Subflows
A Subflow is an Auto-launched Flow that is called from one or more parent Flows. They often have at least one variable that has been made available for input by activating the corresponding checkbox on the variable. In this way the parent Flow can pass data into the Subflow. If creating new objects during your Flow, use dedicated Subflows that receive the necessary inputs and handle the object creation and the fields assignments. Subflows are code snippets that they can be re-used easily and keep your parent Flow clean and readable. Also they can size down complexity and make your declarative programming easier and faster. Besides they reduce maintenance and testing time.
### 9.	Never Hard Code IDs
Please do yourself a favor and never hard code IDs! The beauty of Flows is that it reaches out and gets specific information. Use variables as placeholders for IDs. Hard Coded IDs will not work when shifting from one environment to another by deploying your Flow. For example, when you build and test your Flow in a sandbox by including a specific record type ID, then your hard coded Flow will probably create an error because in the new environment this record type will get a new ID.
### 10.	Make queries selective
When you query data by using the GET Element, use selective filters/conditions and hence minimize the possible outcome. If your Flow queries thousands of records in your org, make sure to use [index fields](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_SOQL_VLSQ.htm) for your conditions as well. The following fields are indexed by default: 
- Primary keys (Id, Name, OwnerId fields),
- foreign keys (lookup or master-detail relationships)
- audit dates (createdDate and SystemModstamp)
- recordType fields (indexed for all standard objects)
- custom fields that are maked as Externa ID or Unique
### 11. After Get-Element check for no results
When performing a data query in the Get-Element be sure to check 'When no records are returned, set specified variables to null.' when you want to store record data by choosing fields and assigning variables manually (advanced). After the Get-Element add a Decision Element to check for Null in the variable. 
### 12. Set correct permissions for access
In the past a Screen Flow was run in user-context whereas Auto-launched Flows were triggered by processes and run in system-context. Today you may select between 3 different modes in Screen Flows:
1. User or system context - depends on how Flow is launched
2. System context with Sharing - Enforces record-level access
3. System context without Sharing - Access all data
When designing a Screen Flow always have in mind that the Flow might operate using the permissions of the user running it. So you need to decide beforehand how much access the Screen Flow actually needs, e.g. by bypassing restrictions. This depends on your requirements of cours. [So which context does your Flow run in?] (https://help.salesforce.com/s/articleView?id=sf.flow_distribute_context.htm&type=5)
## C. Testing and Debugging – test your Flow after building it:
### 1.	Always test your Flow! 
Of course, you develop your Flow in a sandbox or scratchorg. So testing thoroughly is as important as using this testing environment before pushing your development into Production. Flow Builder has a build-in debug tool that has been improved in the recent Salesforce releases. It simplifies testing by using different users and records in a roll-back mode for example. Make sure to test different use cases, with different user roles, profiles and permission sets. Apart from positive test scenarios to get the expected result, also try to come up with negative tests: They are also called provocation tests, robustness tests or falsifying tests. The negative test checks whether the application reacts as expected (i.e. without program termination) to (incorrect) input or operation that does not meet the requirements of the application. Invalid values are intentionally entered, masks are not or only incompletely filled out, interfaces are supplied with incorrect values or the database is disconnected. The test case therefore checks for ‘correct’ processing in the event of incorrect handling.
### 2. Test as many permutations as you possibly can
This is especially true if your Flow uses complex logic. Before distributing the Flow you shall test all use cases you had planned in your concept. Also try to come up with other permutations for testing.
### 3. Use the debugger
Instead of saving multiple versions the debugger makes your life easier. You may simulate running the flow as a different user and by inserting input data for almost any variable. Also a roll-back function does not deteriorate your data.
### 3.	Create fault paths and handle error messages
Fault and errors are bound to happen when working with Flows. Two common uses are sending an email alert containing the error message to a group of people and showing an error screen in Screen Flows. So make sure that your users are presented with detailed error messages when unexpected actions occur. Instead of showing the Salesforce standard message of “an unhandled error has occurred”, rather show them how to handle the error properly. So a fault path is designed to handle when a Flow encounters an error and you tell it what it should do in that case. To create a fault path, connect your element to the 'next' element in line(in particular after the GET, UPDATE, CREATE or DELETE Element), then drag another line from the same element to your fault action. This line is in red color and considered to be the Fault Connector. It is a very common mistake that Flowmasters forget about the error handling; so do not be one of them.
## D. Deploying - distribute your Flow after testing:

## Resources:
- https://help.salesforce.com/s/articleView?id=000325257&type=1
- https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_SOQL_VLSQ.htm
- https://www.salesforceben.com/salesforce-flow-best-practices/
- https://admin.salesforce.com/blog/2021/the-ultimate-guide-to-flow-best-practices-and-standards
- https://salesforcecodex.com/salesforce/top-10-best-practice-for-lightning-flow/
- https://medium.com/@mituldpatel/first-few-things-i-check-in-a-salesforce-flow-85034363b11e
- https://www.youtube.com/watch?v=bENuHB7YQG0
- https://www.captechconsulting.com/blogs/9-tips-for-using-salesforces-lightning-flow-the-right-way

<!-- The text has been written by Michael Hellmann -->