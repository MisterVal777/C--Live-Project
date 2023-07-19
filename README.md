# C--Live-Project

## Introduction
During my 2 week internship for Prosper IT consulting through The Tech Academy, I worked with my peers in a team developing a full scale MVC/MVVM Web Application in C#.  The team was tasked with building software designed for a theater company to manage its website content without needing any technical knowledge. Working on a legacy codebase was a great learning oppertunity for fixing bugs, cleaning up code, and adding requested features.  Here are the back end stories I was tasked with [back end stories](#back-end-stories).  Here are front end stories I was tasked with [front end stories](#front-end-stories)  [skills](#other-skills-learned) 
  
Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.


## Back End Stories
* [Creat BlogPhoto model and CRUD pages](#creat-blogPhoto-model-and-CRUD-pages)
* [Meetup API](#meetup-api)



### Creat BlogPhoto model and CRUD pages.
Starting my part in the project, I needed to create a model and add it to the database named BlogPhoto that would represent all of the images used in the blog area of this project.  After the model was created I than scaffolded the CRUD pages.
  
        public class BlogPhoto
    {
        public int BlogPhotoId { get; set; }
        public string Title { get; set; }
        public byte[] Photo { get; set; }
    }
CRUD views include create.cshtml delete.cshtml details.cshtml edit.cshtml index.cshtml.


### Photo Storage and Retrieval for BlogPhotos.  
Story description- We want to have images for the BlogPhoto model.  To allow users to upload files from their file system.  Then in the controller, the uploaded image should be converted into a byte array (byte[]) and stored in the BlogPhoto table in the database.  The byte[] representing the photo should be able to be retrieved from the database and converted back into an image where it can be displayed.
My solution code here.

      public ActionResult Create(BlogPhoto blogPhoto, HttpPostedFileBase ImageFile)
        {
            if (ModelState.IsValid)
            {
                if (ImageFile != null)
                {
                    blogPhoto.Photo = ConvertToBytes(ImageFile);
                }

                db.BlogPhotos.Add(blogPhoto);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(blogPhoto);
        }

        // GET: Blog/BlogPhotos/Edit/5
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            BlogPhoto blogPhoto = db.BlogPhotos.Find(id);
            if (blogPhoto == null)
            {
                return HttpNotFound();
            }
            return View(blogPhoto);
        }

        // POST: Blog/BlogPhotos/Edit/5
        // To protect from overposting attacks, enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit(BlogPhoto blogPhoto, HttpPostedFileBase ImageFile)
        {
            if (ModelState.IsValid)
            {
                if (ImageFile != null)
                {
                    blogPhoto.Photo = ConvertToBytes(ImageFile);
                }

                db.Entry(blogPhoto).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(blogPhoto);
        }

      public byte[] ConvertToBytes(HttpPostedFileBase file)
        {
            byte[] bytes;
            using (BinaryReader br = new BinaryReader(file.InputStream))
            {
                bytes = br.ReadBytes(file.ContentLength);
            }
            return bytes;
        }

### Style CRUD Pages Part 1: Style the Create and Edit Pages for BlogPhoto model.
Custumer requirements.
Header to display "Create BLog Photo"
style the submit and BAck to List buttons,
Add place holders in the input fields.



### Style CRUD Pages Part 2: Index Page for BlogPhoto



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
