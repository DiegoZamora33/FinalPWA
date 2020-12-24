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
























