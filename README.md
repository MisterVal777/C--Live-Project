# C--Live-Project

## Introduction
During my 2 week internship for Prosper IT consulting through The Tech Academy, I worked with my peers in a team developing a full scale MVC/MVVM Web Application in C#.  The team was tasked with building software designed for a theater company to manage its website content without needing any technical knowledge. Working on a legacy codebase was a great learning oppertunity for fixing bugs, cleaning up code, and adding requested features.  Here are the back end stories I was tasked with[back end stories](#back-end-stories).  Here are front end stories I was tasked with.[front end stories](#front-end-stories)  [skills](#other-skills-learned) 
  
Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.


## Back End Stories
* [Sorting Network Table](#sorting-network-table)
* [Meetup API](#meetup-api)



### Creat BlogPhoto model and CRUD pages.
This page had two tables, one as part of the view, and one was a partial view. My task was to make the table on the partial view sortable by several different catagories. I wanted to do this without reloading the page. 
    
 
### Photo Storage and Retrieval for BlogPhotos.  
We want to have images for the BlogPhoto model.  To accomplish this, we want to allow users to upload files from their file system.  Then, in the controller, the uploaded image should be converted into a byte array (byte[]) and stored in the BlogPhoto table in the database.  The byte[] representing the photo should be able to be retrieved from the database and converted back into an image where it can be displayed on View.

Create a method in the BlogPhoto Controller that has a parameter for an uploaded photo and converts that photo into a byte[].  Add a new file input field to the Create and Edit Views so that images can be uploaded to the controller.  Use this method in the BlogPhotos Create and Edit methods to convert the uploaded image to a byte[].  Assign the byte[] to the BlogPhotos model being created or edited and save it to the database.

There should also be a way to retrieve the byte[] from an entity in the BlogPhoto table.  Create a method in the BlogPhoto controller that accepts an Id.  This method should use the Id to find that BlogPhoto, get its stored byte[], and return the file of that converted photo.  Use this method to display the photos of BlogPhotos on all of the BlogPhotos CRUD pages.


### Style CRUD Pages Part 2: Index Page for BlogPhoto

### Style CRUD Pages Part 1: Style the Create and Edit Pages for BlogPhoto model

### Style CRUD Pages Part 3: BlogPhoto Details & Delete Pages

*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*


## Front End Stories
* [Button Sizing Bug](#button-sizing-bug)

 



*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*

## Other Skills Learned
* Working with a group of developers to identify front and back end bugs to the improve usability of an application
* Improving project flow by communicating about who needs to check out which files for their current story
* Learning new efficiencies from other developers by observing their workflow and asking questions  
* Practice with team programming/pair programming when one developer runs into a bug they cannot solve
    
    
  
*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*
