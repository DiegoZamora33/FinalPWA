# LABORATORY

## General description.

My project uses the *theme of a clinical analysis laboratory.*

It consists of a *main web page visible to all public users*, in this main page you can see laboratory publications, image gallery, location on google maps, see news that the laboratory staff wishes to publish and if you are a client of the laboratory you can see and download your clinical analysis results with a unique code per client.

For the administrators of the site and the laboratory, they have an *authentication login to access the administrative functions of the site*, in my project I decided to establish 3 types of user that have access to the administration functions of the web system.

**Administrator User:** This user can create and delete users, manage patient folder files, manage the publications and images that are displayed on the main page of the site.

**Chemical User:** This user can only manage customer files.

**Publisher User:** This user will only manage the publications of the main page of the website and the images that are displayed on the main page.


## Requirements.

**Main Page:** This is the main page of the site, accessible to everyone, it must be able to show a gallery of images, all the publications and image for each publication, contact information, location on google maps and routine hours.

- If you are a lab patient, you should have a section on the home page to enter my unique code and you should be able to see my list of clinical results and download each one in pdf format.

- If you are a site administrator, the home page should have a button to redirect to the login page.

**Login Page:** The Administrator user, Chemical user and the publisher user should use a login page to authenticate with an email and password and then be redirected to the Administration page. If the user to authenticate is a new user who still has his temporary password, he must be redirected to a form where he can set a new password)

**Administrative Page:** Once the login authentication is completed successfully. The user should be redirected to this page, depending on the type of user the functions will be displayed according to the type of user that is authenticated. All users should have a section where they can see their personal data, and be able to change their email and password.

**Manage Publications:** These functions must be restricted or allowed, depending on the logged in user.

- ***New Post:*** The user with permission to this role should be able to create a new post with the following fields:
  1. Title
  2. Body (the body text has to be saved in markdown format and displayed on the main page converted to HTML format)
  3. An image to accompany the publication
  4. An image to display in the image gallery on the main page (optional)

- ***Edit Post:*** The user with permission to this role should be able to edit any field of a post.

- ***Delete Post:*** The user with permission to this role should be able to delete a post.

**Manage Patient, Folder and Files:** These functions must be restricted or allowed, depending on the logged in user.

- ***New Patient:*** The user with permission for this role should be able to create a new Patient File Folder with the following fields:
  1. Fist Name
  2. Last Name
  3. Age
  4. Cellphone Number
  5. **Unique Code:** The unique code should be 5-character alphanumeric and should be created automatically by the system. (Each patient has a unique code attached to their file folder)
  
- ***Upload file of clinical analysis results:***  The user with permission to this role should be able to upload a file, in pdf format, to a patient's files folder. It should have an interface to simply select the file and load it, the system should save the file in the corresponding Patient. 

- ***ReUpload file of clinical analysis results:*** The user with permission to this role should be able to ReUpload a file, in pdf format, to a patient's files folder. It should have an interface to simply select the new file and load it, the system should delete the old file and save the new file in the corresponding Patient.

- ***Delete file of clinical analysis results:*** The user with permission for this role should be able to delete a file in the corresponding folder of any patient.

- ***Edit Patient:*** The user with permission to this role should be able to edit any field of a Patient, except for the unique code, once a unique code has been assigned, these attributes cannot be changed.

- ***Delete Patient:***  The user with permission for this role should be able to delete a patient. This should remove from the database and patient's files. Later the unique code is moved to ***Bad List***, because when a unique code is assigned, never again to be assigned another patient.

**Manage Users:** Only an administrator user has permission to these functions.

- ***New User:*** The user should be able to create a new user with the following fields:
  1. Fist Name
  2. Last Name
  3. Email
  4. Cellphone Number
  5. Type of user
  6. Password (This password only is temporary for the new user, the new user must access the system and change it)

- ***Delete User:***  The user with permission for this role should be able to delete a User.

- ***Change Email/password of user:*** If the user to authenticate is a new user who still has his temporary password, he must be redirected to a form where he can set a new password). All users should be able to change their email or password.

<br>

## Documentation

### Two Django App's
I decided to declare 2 Django applications in my final project, because for me it is easier to have a django application just for site administration and another application only for public use.
Both applications share the unique database model.

The project is called * final_project *, the website administration application is called * adminLab * and the public use application is called * laboratory *.
In the file `final_project / urls.py` the url references for each application are declared.

### Data Base (Models)
I have used a total of 6 models in the file `adminLab / models.py` this file contains all the models used for the entire website, it is also used by the laboratory application.
The list of models I used is as follows.
1. * Type_User *: This model only serves to save the user types **Admin**, **Chemist Doctor** and **Publisher**. ***It is important*** that this model is not null, it is important that the model always has the following registers `{" Admin "," Chemist Doctor "," Publisher "}`.

2. * User *: This model extends from `from django.contrib.auth.models import AbstractUser` and I have added the necessary attributes to make a one-to-one relationship with * Type_User * model, cell phone and a boolean to know if even It is under the temporary password as well as date fields for creation and update.

3. * Patient *: This model contains the fields required in the requirements and I have added fields for creation and update date. It also has a `serialize ()` function to convert to JSON format.

4. * File_Analysis *: This model contains a foreign key reference for the ** Patient ** model, it contains a text field and the important thing contains a field of the type `file = models.FileField (upload_to = f" MEDIA / Patients_Files / ", null = False, blank = False)`, what this field does is save a file to the specified path. it also contains fields for creation and update date information. It also has a `serialize ()` function to convert the fields except the file field into JSON format.

5. * Post *: This model contains 2 very interesting fields, it contains a field of the type `image_post = models.ImageField (upload_to =" MEDIA / Post_Images / ", null = True, blank = True)`, this field saves an image in the specified path and also the field `image_gallery = models.ImageField (upload_to =" MEDIA / Gallery_Images / ", null = True, blank = True)` which also saves an image in the specified path, the difference is that an image It is shown in the image gallery of the laboratory website and another image is shown within the publication.
For the above fields to work, it is necessary to have installed *** Pillow *** libraries, since I am working with images in my model.

6. * Bad_List *: I use this model to store all the codes that have already been used as required by the project requirements.

<br>

### Application ** AdminLab **
I have decided to make a totally dynamic application with JavaScript, that is, I am always making fetch requests to my adminLab application and then I render the data in the graphical interface.

##### urls.py
This file contains a total of 26 url entries.

My index url is the main url for my ** adminLab ** application.

I have decided to use 3 urls to serve my pdf files and my images.
`path (" ImageGallery / <int: id> ", views.image_gallery_api, name =" ImageGallery ")` refers to a function that returns the gallery image corresponding to the id of a Post.
`path (" ImagePost / <int: id> ", views.image_post_api, name =" ImagePost ")` refers to a function that returns the post image corresponding to the id of a Post.
`path (" FileFolder / <str: code> / <int: id> ", views.fileFolder_api, name =" FileFolder ")` refers to a function that returns the pdf file corresponding to a patient and the id of the record in the database.

I also have different urls for the CRUD of ** Patient **, I have a url for each manipulation such as ** newPatient **, ** updatePatient **, ** deletePatient ** and the urls to obtain information about all the patients ** patients ** and about a specific patient ** patient / <str: code> **

I also have several urls for the CRUD of ** Publication **, I have a url for each manipulation such as ** newPublication **, ** updatePublication **, ** deletePublication ** and the url to obtain information about all the publications ** publications ** and about a specific publication ** publication / <int: id> **.

The urls I use to manage patient files are as follows.
`path (" folder / <str: code> ", views.folder_api, name =" folder ")`.

`path (" newFileFolder / <str: code> ", login_required (views.newFileFolder_api), name =" NewFileFolder ")`.

`path (" updateFileFolder / <str: code> / <int: id> ", login_required (views.updateFileFolder_api), name =" UpdateFileFolder ")`.

`path (" deleteFileFolder / <str: code> / <int: id> ", login_required (views.deleteFileFolder_api), name =" DeleteFileFolder ")`.
They are urls for a complete crud.
In addition to 4 urls for the administration of users and the first login.

***Note:*** All my urls have the function login_required (views.funcion) where the function of the view is stopped as a parameter, because that way it is easier for me to manage the access of a required login.

##### views.py
This file is the hard part of my back-end it contains all the functions for the CRUDs of my models and the functions of index, login and the functions for changing the password and email.

- **def index**: receives a request as a parameter and by get method I return my index.html with all the information corresponding to the patients, publications, user and types of users.

- **def publications_api**: receives a request as a parameter and only for a POST request it returns a list in JSON format of the information of all the publications. I have decided to use the post method for the security of the token.

- **def publication_api**: receives as a parameter a request and the corresponding id of a publication and only by a POST request what it does is search for the publication by the database id and also searches the text in .md format of the publication with the function `util.delJump (util.get_publication (publication.id))` for each publication and returns its information in JSON format if the id does not exist, it returns an error message in JSON format. I have decided to use the post method for the security of the token.

- **def newPublication_api**: it receives a request as a parameter and only for a POST request what it does is save the form data sent by POST method to create a new publication, within the function we use `util.save_publication ( newPublication.id, text) `which is a function of the ** util.py ** file that does is save in` .md` format the body of the publication text with the name of the publication id. Returns a success or fail message corresponding to the event of the function.

- **def updatePublication_api**: receives as a parameter a request, id and only for a POST request what it does is save the form data sent by POST method to update the information of a publication is specific by id. If the publication already has images assigned, what it does is delete the old image `util.delete_image (oldImage)` and save the new image. then what it does is save the body text of the publication in `.md` format. Returns a success or fail message corresponding to the event of the function.

- **def deletePublication_api**: it receives as a parameter a request, id and only by a POST request what it does is delete a publication from the database by the specific id but before that it deletes all the images associated with the publication with `util.delete_image (oldImage)` and also delete the .md file `util.delete_MD (id)`. Returns a success or fail message corresponding to the event of the function.

- **def image_gallery_api**: it receives as a parameter a request, id and only for a POST request what it does is return the gallery image of the corresponding publication, this function returns the image as such with a FileResponse ().

- **def image_post_api**: it receives as a parameter a request, id and only for a POST request what it does is return the post image of the corresponding publication, this function returns the image as such with a FileResponse ().

- **def fileFolder_api**: it receives as a parameter a request, code, id and only by a POST request what it does is that it searches for a patient by the code, then with the patient it obtains the patient's file with the id of the record of the file, return the pdf file corresponding to the patient, this function returns the file as such with a FileResponse ().

- **def folder_api**: this function receives as parameters a request and a code, the code is the patient's code and what it does is return in json format a whole list of the File_Analysis records that the patient has in relation to That model. It only receives the request by post method because I like the security of the token.

- **def newFileFolder_api**: this function receives a request and a code as parameters, the code is the patient's code and what it does is create a new record in File_Analysis in relation to the patient model obtained by the code. It only receives the request by post method because I like the security of the token. returns a success or fail message depending on the time that occurs in the function.

- **def updateFileFolder_api**: this function receives a request, a code and an id as parameters, the code is the patient's code and the id is the id of the reference record in the File_Analysis model and what it does is update the data From the record related to the patient in File_Analysis, delete the old pdf file with the function `util.delete_PDF (oldFile)` and save the new file. It returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

- **def deleteFileFolder_api**: this function receives as parameters a request, a code and an id, the code is the patient's code and the id is the id of the reference record in the File_Analysis model and what it does is delete the data of the record made to the patient in File_Analysis, delete the pdf file with the function `util.delete_PDF (oldFile)` and delete the record in the database. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

- **def patients_api**: this function receives a request as parameters and what it does is return a list of JSON formats of all the information of a patient from the Patient model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

- **def patient_api**: this function receives a request and a code as parameters, the code is the patient code to show their information, what it does is return a JSON of all the information of a patient from the Patient model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

- **def newPatient_api**: this function receives a request as parameters and what it does is create a new Patient from the Patient model. To generate the unique code use the function `code = util.generateCode (listCodes)` where `listCodes` is the union of the assigned code list of the Patient model and the codes found in the Bad_List model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def updatePatient_api**: this function receives a request and a code as parameters, the code is the patient's code to update their information. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def deletePatient_api**: this function receives a request and a code as parameters, the code is the patient's code to delete their information from the Patient model, when a patient is deleted we look for records referenced in the File_Analysis model, if it has references We also eliminate their assigned files, when the patient is eliminated, his code becomes added to the Bad_List model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def users_api**: this function receives a request as parameters and what it does is return a list of JSON formats of all the information of a user of the User model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def deleteUser_api**: this function receives as parameters a request and an email, the email is the user's email to delete their information from the User model. returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def changePassword_api**: this function receives a request as parameters and by POST method, it receives current password, new password and password confirmation, it makes user validations to verify that it is the same user in the request to change the password . returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def changeEmail_api**: this function receives a request as parameters and by POST method, receives current email, new email and email confirmation, performs user validations to verify that it is the same user in the request to change the email . returns a success or fail message according to the event that occurs in the function. It only receives the request by post method because I like the security of the token.

**def firstLogin_api**: this function receives a request as parameters and by POST method, receives current password, new password and password confirmation, makes user validations to verify that it is the same user in the request to change the password . returns a success or fail message according to the event that occurs in the function, returns it directly to render the Login view or the index view. It only receives the request by post method because I like the security of the token.

**def login_view**: this function receives a request as parameters and by POST method, receives email and password, performs user validations to verify that it is a user in the database and creates a login. returns a fail message according to the event that occurs and redirects to index if the login is correct. It only receives the request by post method because I like the security of the token. and by get method renders an html path of login.html

**def logout_view**: this function receives a request as parameters, what it does is close the instance of a user. return a redirect to index using GET method

**def register_api**: this function receives a request as a parameter, and by POST method receives the corresponding information to create a user of the User model, but it creates the instance of the user with `user = User.objects.create_user (email , email, password) `after we update the data corresponding to the created user to fill the data that was added to the user model. returns a success or fail message according to the event that happens in the function. it only receives by POST method because I like the security of the token.

***Note:*** almost all functions are restricted by the type of user that is being received in the request.

##### util.py

This file contains functions to manipulate files on the disk, I made it based on the util.py file of the wiki project, it has different functions, to save .md files corresponding to a publication, save or delete images, to save or delete pdf files and to generate the unique codes.

- **def generateCode**: this function receives as a parameter a list of codes which are excluded in the code generation, with the line `` code = ''. join ([choice (ascii_uppercase + digits) for i in range ( 5)]) `I generate a new random alphanumeric code of 5 characters and verify if this code is in the listCodes, if it exists in listCodes I generate one more until I obtain a code that is not in listCodes.

**def list_publications**: this function returns a list of existing file names in the path `MEDIA / Post_Data /` with the extension .md, in that path is where all my .md files corresponding to a publication are stored .

**def save_publication**: this function receives as parameters a text and a title, what it does is save the content of the text in an .md file with the title specified in the path `MEDIA / Post_Data /`

**def delete_image**: what this function does is delete the file specified in the url that it receives as a parameter. Normalmnete this cundion I use it to eliminate the images corresponding to a publication. These images are stored in the paths `MEDIA / Gallery_Images /` and `MEDIA / Post_Images /`

**def delete_PDF**: what this function does is delete the file specified in the url that it receives as a parameter. Normally I use this to delete the pdf file corresponding to a File_Analysis. These pdf files are stored in the path `MEDIA / Patients_Files /`

**def delete_MD**: what this function does is delete the specified file with the title it receives as a parameter. Normally I use this function to delete the .md file corresponding to a Post. these .md files are stored in the path `MEDIA / Post_Data /`

**def get_publication**: what this function does is obtain the specified file with the title it receives as a parameter. normally I use this function to get the text from the .md file corresponding to a Post. these .md files are stored in the path `MEDIA / Post_Data /`

**def delJump**: this function receives a Markdown format text with a parameter, which is to eliminate an excess of `/ r` because when displaying my markdown text it adds an extra of those operators that are line breaks and creates a conflict with my saved text. returns that same text but debugged from `/ r`

##### Templates, html, CSS and JS

for my html templates I have decided to use bootstrap because it is a design library that I have mastered very well, in addition to a ** myStyles.css ** file that contains some classes of custom functions, as well as a FadeIn nimation that I made and that I like it.

I have also decided to use the JS libraries required by Bootstrap to be able to use modals and a pretty responsive Navbar.

- **layout.html**: this file contains the references to all the complete Boostrap to its Boostrap CSS and JS. And it also refers to my custom CSS file and a very special and important File called **dinamico.js**. In addition to a block that will be extended by index.html. the arhic layout also contains a Bootstrap Modals system which I use as a formulation and as alerts to show and hide dynamically. This modal files are assigned by the type of user.

- **index.html**: this file is very informative since in this file I make all my dynamic website possible, it receives all the information that the viewsd.index function passes to it and depending on the user it shows the corresponding administrative modules, it extends from the layout.html file, this file together with dinamico.js are for each other.

- **login.html**: this file contains the form to login a user.


##### dinamico.js

This file is of vital importance, it is the most important file for the front-end of my adminLab web application to work.

This file contains various functions to manage Navbar button events, manage when and how to hide or show modals, make requests to my django backend and to render new elements in HTML DOM.

It has a total of 1511 lines, explaining all the content of my file is very extensive so I will explain them in a general way.

Each function in my file has a name which is very descriptive with the action it performs and also before each function contains a comment that describes what the function does.

This file has `fetch` functions to send and receive requests from my Django server and depending on the response it has, it does actions to render more elements in the DOM and / or show
Bootstrap Modals.

To submit image and pdf files, I have used the `$ ajax` function because it is more powerful than the` fetch` function and it is better for me just to submit a form containing binary data of images and files.

### Application ** AdminLab **

##### urls.py
This file only contains an index url needed to display the main page of the website. and a url to redirect to login when it is an unwanted login.

##### views.py
This file contains 2 functions and makes an import from `from adminLab.models import Post` to import the publicacoin model from my shared database by the 2 applications.

- **def index**: render my index.html and pass it as a parameter the list of all publications, in addition to obtaining the .md text of each publication and converting it to HTML format with the Markdown functions.

- **def redirecLogin**: This function only redirects the login when it comes to an unwanted login.

##### Templates, HTML, CSS and JS 
This application only contains a total of 2 templates, ** layout.html ** and ** index.html **.

- **layout.html**: this file contains the references to all the complete Boostrap to its Boostrap CSS and JS. And it also refers to my custom CSS file and a very special and important File called **dinamico.js**. In addition to a block that will be extended by index.html. the layout file also contains a Bootstrap Modal, said modal helps me to ask for the unique code of the patient and show his list of analsis. also in the footer it contains a frame that refers to a google map.

- **index.html**: this file renders all the publications that are stored in my database.

- **dynamic.js** this file is only to send the patient's code to my django server and obtain the list of analysis results and depending on the server's response, show a modal with the patient's information or error messages depending on the case.


***NOTE:*** FOR THE WEBSITE TO WORK IT IS NECESSARY TO HAVE THE RECORDS OF "Type_User" {"Admin", "Chemist Doctor", "Publicher"} AND HAVE THE "Markdown2" AND "Pillow" LIBRARIES INSTALLED.

***MY WEBSITE IS TOTALLY RESPONSIVE, TO GET THIS I HAVE DECIDED TO USE ALL THE FUNCTIONALITY OF THE "Bootstrap"***

