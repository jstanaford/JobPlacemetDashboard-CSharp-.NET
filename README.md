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
In one of the classes called, Productions, an attribute called Showtime was not displaying the time entered properly. In one section of the website, we wrote up a view for the Productions hosted at the theatre. Productions had its own index view as well as create, delete and details capabilities. Additionally, there is a controller assigned to all of the functions taking place with the productions at work. When someone would create a new Production the website would function as designed, however, upon returning to the index page, the user would not see the time they entered for the variables of ShowtimeMat and ShowtimeEve. Instead, they would just see the desired format of hh:mm tt. So, to remedy this solution, I added code to the Model class for Productions rather than altering the view or controller. 

I'm showing the methdod I used below on the Production.cs file to debug the issue:

	[DataType(DataType.Time)]
	//[DisplayFormat(DataFormatString = "hh:mm tt", ApplyFormatInEditMode = true)] 			//before
	[DisplayFormat(DataFormatString = "{0:hh:mm tt}", ApplyFormatInEditMode = true)]		/after
        [Display(Name = "Evening Showtime")]
        public DateTime? ShowtimeEve { get; set; }
        
        [DataType(DataType.Time)]
        //[DisplayFormat(DataFormatString = "hh:mm tt", ApplyFormatInEditMode = true)]			/before
        [DisplayFormat(DataFormatString = "{0:hh:mm tt}", ApplyFormatInEditMode = true)]
        [Display(Name = "Matinee Showtime")]
        public DateTime? ShowtimeMat { get; set; }


In this case, I could have gone to the Production view and chosen to edit the html using some type of ".Value.ToString()?" approach, but I decided to make my model do the "heavy lifting" for the sake of keeping the views simple. Additionally, I had to ensure that each of these values could be nullable. 

## Seed the Production Database

At this point in the project, most of the classes were very empty of any data. The point of this assignment was for the sake of creating a lot of "dummy data" to be able to test the functionality of the Productions part of the website. Utilizing the Startup.cs file, I created a sample of dummy data involving various, popular productions. This was done by creating a list of Productions and adding them all at once. I needed to ensure I used the prpoper method as to avoid duplicating data everytime the website was launched. 

First things first, I needed to add the name I assigned to my Seed method to the Configuration method in the project's startup. 


	public void Configuration(IAppBuilder app)
		{
		    ConfigureAuth(app);
		    createRolesandUsers();
		    SeedCastMembers();
		    SeedProductions();
		}

From there, it was just a matter of seeding the database. The context for the project had already been declared so this assignment in itself was easy as creeating the data, writing the command to update the database, and saving the changes.

//Seeding database with dummy Productions
		private void SeedProductions()
		{
			var productions = new List<Production>
			{
				new Production{Title = "Hamilton", Playwright = "Lin-Manuel Miranda", Description = "This is a musical 					inspired by the biography " +
				"Alexander Hamilton by historian Ron Chernow. This musical tells the story of American Founding Father 					Alexander Hamilton through music " +
				"that draws heavily from hip hop, as well as R&B, pop, soul, and traditional-style show tune. ", 					OpeningDay = new DateTime(2020, 01, 02, 19, 30, 00),
				ClosingDay = new DateTime(2020, 01, 30, 19, 30, 00), ShowtimeEve = new DateTime(2020, 01, 02, 19, 30, 00) 				  , ShowtimeMat = new DateTime(2020, 01, 02, 22, 30, 00),
				TicketLink = "ticketsforyou.com", Season = 1, IsCurrent = true},
				new Production{Title = "Phantom of the Opera", Playwright = "Andrew Lloyd Webber & Charles Hart", 					Description = "Based on a French " +
				"novel of the same name by Gaston Leroux, its central plot revolves around a beautiful soprano, Christine 				  Daae, who becomes the obesession " +
				"of a mysterious, disfigured musical genius living in the subterranean labyrinth beneath the Paris Opera 				 House.", OpeningDay = new DateTime(2020, 01, 04, 17, 30, 00),
				ClosingDay = new DateTime(2020, 02, 02, 17, 30, 00), ShowtimeEve = new DateTime(2020, 01, 04, 17, 30, 00), 				   ShowtimeMat = new DateTime(2020, 01, 04, 19, 30, 00),
				TicketLink = "ticketsforyou.com", Season = 1, IsCurrent = true},
				new Production{Title = "The Book of Mormon", Playwright = "Trey Parker, Robert, Lopez, & Matt Stone", 					Description = "The Book of Mormon " +
				"follows two Latter-Day Saints missionaries as they attempt to preach the faith of the Church of Jesus 					Christ of Latter-Day Saints to the " +
				"inhabitants of a remote Ugandan village.", OpeningDay = new DateTime(2020, 04, 02, 19, 30, 00), 					ClosingDay = new DateTime(2020, 04, 27, 19, 30, 00),
				ShowtimeEve = new DateTime(2020, 04, 02, 19, 30, 00), ShowtimeMat = new DateTime(2020, 04, 02, 22, 30, 					00), TicketLink = "ticketsforyou.com", Season = 2,
				IsCurrent = true},
				new Production{Title = "Wicked", Playwright = "Stephen Schwartz", Description = "This musical is told from 				   the perspective of the witches of " +
				"the Land of Oz; its plot begins before and continues after Dorothy Gale arrives in Oz from Kansas, and 				includes several references to the 1939 film.",
				OpeningDay = new DateTime(2019, 10, 01, 19, 30, 00), ClosingDay = new DateTime(2019, 10, 25, 19, 30, 00), 				  ShowtimeEve = new DateTime(2019, 10, 01, 19, 30, 00),
				ShowtimeMat = new DateTime(2019, 10, 01, 23, 30, 00), TicketLink = "ticketsforyou.com", Season = 4, 					IsCurrent = false},
				new Production{Title = "How to Succeed in Business Without Really Trying", Playwright = "Frank Loesser", 				  Description = "This story concerns young, " +
				"ambitious J. Pierrepont Finch, who, with the help of the book How to Succeed in Business Without Really 				  Trying, rises from window washer to chairman of " +
				"the board of the World Wide Wicket Company.", OpeningDay = new DateTime(2019, 12, 01, 19, 30, 00), 					ClosingDay = new DateTime(2019, 12, 22, 19, 30, 00),
				ShowtimeEve = new DateTime(2019, 12, 01, 19, 30, 00), ShowtimeMat = new DateTime(2019, 12, 01, 23, 30, 					00), TicketLink = "ticketsforyou.com", Season = 4,
				IsCurrent = false},
			};
			productions.ForEach(Production => context.Productions.AddOrUpdate(d => d.Title, Production));
			context.SaveChanges();
		}


I know this code above isn't the easiest to look at (for some reason I can't get it to look nice outside of editing mode); but due to its repetitive nature, one only needs to see one or two of the lines to understand what is happening, just with different data

## Other Skills Learned
* Worked with a team of developers to identify front end and backend end improvements for the theatre's public and admin portions of the website.
* Created an environment for clear communication between team members while checking out files for the project. 
* Learned from one on one guidance from the scum master & benefited from group programming/discussion.
* Problem solved by asking questions to completely understand expectations of assigned stories/work. 










