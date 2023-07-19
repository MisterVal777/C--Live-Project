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
Header to display "Create BLog Photo".
style the submit and BAck to List buttons.
Add place holders in the input fields.
Place the form in a centered container.
When the user selects a file, display the file in the form so that the user can see the imade that is uploaded.


#Creat page

      <body style="background-color: #000000">
          <div class="blogphoto-create-h4container">
              <h4>Blog Photo Create</h4>
          </div>
          <div class="blogphoto-create-container">
              @using (Html.BeginForm("Create", "BlogPhotos", FormMethod.Post, new { enctype = "multipart/form-data" }))
              {
                  @Html.AntiForgeryToken()
                  <div class="form-horizontal">
                      <hr />
                      @Html.ValidationSummary(true, "", new { @class = "text-danger" })
                      <div class="form-group">
                          @Html.LabelFor(model => model.Title, htmlAttributes: new { @class = "control-label col-md-2" })
                          <div class="col-md-10">
                              @Html.EditorFor(model => model.Title, new { htmlAttributes = new { @class = "form-control p-3 mb-2 bg-secondary text-white", @placeholder = "Title" } })
                              @Html.ValidationMessageFor(model => model.Title, "", new { @class = "text-danger" })
                          </div>
                      </div>          
                          <div class="form-group">
                              @Html.LabelFor(model => model.Photo, htmlAttributes: new { @class = "control-label col-md-2" })
                              <div class="col-md-10">
                                  <input type="file" name="ImageFile" id="ImageFile" onchange="document.getElementById('blah').src = window.URL.createObjectURL(this.files[0])"/>
                                  <img id="blah" alt="" width="100" height="100" />
      
                                  
                                  @Html.ValidationMessageFor(model => model.Photo, "", new { @class = "text-danger" })
                              </div>
                          </div>
                          <div class="form-group">
                              <div class="blogphoto-create-backbtn">
                                  @Html.ActionLink("Back to List", "Index", null, new { style = "text-decoration:none;" })
                              </div>
                              <input type="submit" value="Create" class="blogphoto-create-submitbtn" />                       
                          </div>             
                  </div>
              }
          </div>
          @section Scripts {
              @Scripts.Render("~/bundles/jqueryval")
          }
      </body>

  #Edit Page

      <body style="background-color: #000000">

    <div class="blogphoto-edit-h4container">
        <h4>Blog Photo Edit</h4>
    </div>

    <div class="blogphoto-edit-container">


        @using (Html.BeginForm("Create", "BlogPhotos", FormMethod.Post, new { enctype = "multpart/form-data" }))
        {
            @Html.AntiForgeryToken()

        <div class="form-horizontal">
            <h4>BlogPhoto</h4>
            <hr />
            @Html.ValidationSummary(true, "", new { @class = "text-danger" })
            @Html.HiddenFor(model => model.BlogPhotoId)

            <div class="form-group">
                @Html.LabelFor(model => model.Title, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.Title, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.Title, "", new { @class = "text-danger" })
                </div>
            </div>



            <div class="form-group">
                @Html.LabelFor(model => model.Photo, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    <input type="file" name="ImageFile" id="ImageFile" onchange="document.getElementById('blah').src = window.URL.createObjectURL(this.files[0])" />
                    <img id="blah" alt="" width="100" height="100" />
                    @Html.ValidationMessageFor(model => model.Photo, "", new { @class = "text-danger" })
                </div>
            </div>

            <div class="form-group">
                <div class="blogphoto-edit-backbtn">
                    @Html.ActionLink("Back to List", "Index", null, new { style = "text-decoration:none;" })
                </div>
                <input type="submit" value="Confirm Edit" class="blogphoto-edit-submitbtn" />
            </div>

        </div>
        }
        </div>

        

        @section Scripts {
            @Scripts.Render("~/bundles/jqueryval")
        }
    </body>

### Style CRUD Pages Part 2: Index Page for BlogPhoto
#Custumer requirements.
Use Boot-strap Card column layout.
Images should have a subtle hover animation/transition.  Also should display the title of the photo.
Add bootstrap badge-pill buttons.
User should beable to click on the photo to go to the photo's details page.

    <style>
        .card-title {
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            position: absolute;
            top: 10px;
            left: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 5px;
            color: #fff;
        }
    
        .card-img-top {
            transition: opacity 0.3s ease-in-out;
        }
    
        .card:hover .card-title {
            opacity: 1;
        }
    
        .card:hover .card-img-top {
            opacity: 0.7;
        }
        .smaller-image {
            width: 200px;
            height: 150px;
        }
        .smaller-card {
            width: 210px;
            height: 225px;
        }
    </style>
    
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <p>
            @Html.ActionLink("Create New", "Create", null, new { @class = "btn btn-primary" })
          </p>
    
          @foreach (var item in Model)
          {
            string text = "";
            if (item.Photo != null)
            {
              text = Convert.ToBase64String(item.Photo);
            }
    
            <div class="card bg-dark text-white mb-3 smaller-card">
              <a href="@Url.Action("Details", "BlogPhotos", new { id = item.BlogPhotoId })" class="card-link">
                <img src="data:jpeg;base64,@text" class="card-img-top smaller-image" alt="">
              </a>
              <div class="card-body">
                <h5 class="card-title">@Html.DisplayFor(modelItem => item.Title)</h5>
                <div class="btn-group" role="group">
                  @Html.ActionLink("Edit", "Edit", new { id = item.BlogPhotoId }, new { @class = "btn btn-dark" })
                  @Html.ActionLink("Delete", "Delete", new { id = item.BlogPhotoId }, new { @class = "btn btn-danger" })
                </div>
              </div>
            </div>
          }
        </div>
      </div>
    </div>






### Style CRUD Pages Part 3: BlogPhoto Details & Delete Pages
#Custumer requirements.
Center the content on the page.
Style the buttons with Font Awsome Icons.


#Details page.

    <style>
        .details-page {
            text-align: center;
            margin-top: 50px;
        }
    
        .details-page h2 {
            margin-bottom: 20px;
        }
    
        .details-page h3 {
            font-weight: normal;
            margin-bottom: 30px;
        }
    
        .details-page .blog-photo-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 30px;
        }
    
        .details-page .blog-photo-container .blog-photo {
            margin-bottom: 10px;
        }
    
        .details-page .blog-photo-container .blog-title {
            text-align: center;
        }
    
        .details-page .blog-photo-container .blog-title h4 {
            margin: 0;
            color: #fff;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 5px;
        }
    
        .details-page .blog-photo-container img {
            display: block;
            max-width: 100%;
            height: auto;
            margin-bottom: 10px;
        }
    
        .details-page .form-actions {
            text-align: center;
            margin-top: 30px;
        }
    
        .details-page .delete-btn {
            color: #fff;
            background-color: #BD1A11;
            border-color: #ff0000;
        }
    </style>
    
    <div class="details-page">
        <h2>Blog Photo Details</h2>
    
        <div class="blog-photo-container">
    
            <div class="blog-title">
                <h4>BlogPhoto</h4>
                @Html.DisplayFor(model => model.Title)
                <div class="blog-photo">
                    <img src="data:image/jpeg;base64,@(Model.Photo != null ? Convert.ToBase64String(Model.Photo) : string.Empty)" alt="Blog Photo" />
                </div>
            </div>
        </div>
    
        @using (Html.BeginForm())
        {
            @Html.AntiForgeryToken()
    
            <div class="form-actions no-color">
                <a href="@Url.Action("Index")" class="btn btn-dark"><i class="fas fa-arrow-left"></i> Back to List</a>
                <a href="@Url.Action("Edit", new { id = Model.BlogPhotoId })" class="btn btn-dark" role="button">
                    <i class="fas fa-edit"></i> Edit
                </a>
            </div>
        }
    </div>

  #Delete Page.
  
      <style>
        .delete-page {
            text-align: center;
            margin-top: 50px;
        }
    
        .delete-page h2 {
            margin-bottom: 20px;
        }
    
        .delete-page h3 {
            font-weight: normal;
            margin-bottom: 30px;
        }
    
        .delete-page .blog-photo-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 30px;
        }
    
        .delete-page .blog-photo-container .blog-photo {
            margin-bottom: 10px;
        }
    
        .delete-page .blog-photo-container .blog-title {
            text-align: center;
        }
    
        .delete-page .blog-photo-container .blog-title h4 {
            margin: 0;
            color: #fff;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 5px;
        }
    
        .delete-page .blog-photo-container img {
            display: block;
            max-width: 100%;
            height: auto;
            margin-bottom: 10px;
        }
    
        .delete-page .form-actions {
            text-align: center;
            margin-top: 30px;
        }
    
        .delete-page .delete-btn {
            color: #fff;
            background-color: #BD1A11;
            border-color: #ff0000;
        }
    </style>
    
    <div class="delete-page">
        <h2>Delete this Blog Photo?</h2>
    
        <div class="blog-photo-container">
    
            <div class="blog-title">
                <h4>BlogPhoto</h4>
                @Html.DisplayFor(model => model.Title)
                <div class="blog-photo">
                    <img src="data:image/jpeg;base64,@(Model.Photo != null ? Convert.ToBase64String(Model.Photo) : string.Empty)" alt="Blog Photo" />
                </div>
            </div>
        </div>
    
        @using (Html.BeginForm())
        {
            @Html.AntiForgeryToken()
    
    <div class="form-actions no-color">
        <a href="@Url.Action("Index")" class="btn btn-dark"><i class="fas fa-arrow-left"></i> Back to List</a>
        <a href="@Url.Action("Edit", new { id = Model.BlogPhotoId })" class="btn btn-dark" role="button">
            <i class="fas fa-edit"></i> Edit
        </a>
        <button type="submit" class="btn btn-default delete-btn"><i class="fas fa-trash"></i> Delete</button>
    </div>
        }
    </div>



 [Page Top](#live-project)*


## Front End Stories
* [Button Sizing Bug](#button-sizing-bug) [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*

## Other Skills Learned
* Working with a group of developers to identify front and back end bugs to the improve usability of an application
* Improving project flow by communicating about who needs to check out which files for their current story
* Learning new efficiencies from other developers by observing their workflow and asking questions  
* Practice with team programming/pair programming when one developer runs into a bug they cannot solve
    
    
  
*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*
