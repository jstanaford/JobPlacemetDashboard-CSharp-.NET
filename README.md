# JobPlacemetDashboard-CSharp-.NET
My contributions to a ASP .NET MVC and Entity Framework project during an internship with Prosper IT Consulting.


## Introduction 
This project is the interactive website for managing the content and productions for a theatre/acting company. It is designed to function as a content management service for users who are not technically saavy and want to manage the display on their website. It also includes a login capability for subscribers and maintains a wiki of past performances and peformers. To summarize, the project is intended to have: an Admin Area, Visitor Area, Subscriber area, Calendar, Company Wiki, and specific styling.  

I have worked on various front end and back end stories, with various levels of difficulty. This project utilzed the Agile/Scrum methodology of development. Stories and Version controlled was accomplished through Azure DevOps. General questions and team communication was carried out through Google hangout and Slack. During this project, I learned what it means to achieve clean version control, working with branches, and merging. 

Below I have a description of each Assignment/story I completed, along with code snippets attached to this README.


## Register Form

This story was mostly centered around debugging and styling. On one of our views, a page contained a form but the text was all white; so, none of it was visible. Additionally, the styling was not very pleasant and many elements were not in alignment. 

To remedy this I:
* fixed the text color to be visible
* Aligned text and text boxes for consitency
* Changed font sizing for visual appeal & readability
* Aligned register button
* Utilized spacing for a better general layout

Below is the CSS code I wrote to accomplish these goals in my HTML classes:

/*Styling for the register form page*/
.registerText {
	color: black;
	font-family: Lato;
	font-size: 120%;
}
.registerTextHead { 
	color: black;
	font-family: Lato;
	font-size: 160%
}
.registerInput {
	width: 80%;
}
.registerButton {
	width: auto;
}
/*End style for register form page*/


I won't show the HTML itself because of its length for this particular view; but, I more or less just utilized these classes in various div elements on an HTML form that also utilized some bootstrap. 


## JSON Admin Settings 

The purpose of this story was to create a JSON Object and add it to the project in the form of a JavaScript Dictionary. This was for the sake of the Admins being able to set values for certain factors within the site. 

So I created the JSON file with the JavaScript Dictionary for the admin with some dummy data assigned to it. This task was simple in and of itself, but it laid the groundwork for providing a file for future JSON/AJAX functions in the future. 

Below is a code snippet of the JSON file I created with very litle information to begin with. 


        {
          "current_season" : "2",
          "season_productions" : {
            "fall" : "1",
            "winter" : "2",
            "spring" : "3"
          },
          "Recent Definition" : {
            "span" : "12",
            "date" : "01/28/2020"
          },
          "onstage" :  "2"
        }


## Set Default Value for drop downs
In this project there was a view in correlated to particular Parts in a production. In the edit view for this, the dropdowns for the Cast Member an Production of a Part object would not initilize with the current value stored in the database. I was asked to set them to the current value so they would only be edited if needed. 

This solution came in two parts:
Firstly, I provided ViewData in a Get class in the controller for gathering the Edit View. This is a codesnipped of what that looked like below: 

      ViewData["Productions"] = new SelectList(db.Productions, "ProductionId", "Title", part.Production.ProductionId);
			
			ViewData["CastMembers"] = new SelectList(db.CastMembers, "CastMemberId", "Name", part.Person.CastMemberID);

Secondly, I simply had to call the data by name in my actual view file. Which was as simple as these lines of code below: 

            @Html.DropDownList("CastMembers")
            @Html.DropDownList("Productions")
            

## Date Time Nullable display formatting issue. 







