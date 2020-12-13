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

- If you are a lab customer, you should have a section on the home page to enter my unique code and you should be able to see my list of clinical results and download each one in pdf format.

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
  5. **Unique Code:** The unique code should be 5-character alphanumeric and should be created automatically by the system in addition to preventing the generation of bad words. (Each patient has a unique code attached to their file folder)
  6. **Folder Files:** Every time a new patient is created, a folder named equal to the unique code generated for the patient must be created, the folder must be saved in the system to store a list of the files referring to this patient.
  
- ***Upload file of clinical analysis results:***  The user with permission to this role should be able to upload a file, in pdf format, to a patient's files folder. It should have an interface to simply select the file and load it, the system should save the file in the corresponding folder. 

- ***ReUpload file of clinical analysis results:*** The user with permission to this role should be able to ReUpload a file, in pdf format, to a patient's files folder. It should have an interface to simply select the new file and load it, the system should delete the old file and save the new file in the corresponding folder.

- ***Delete file of clinical analysis results:*** The user with permission for this role should be able to delete a file in the corresponding folder of any patient.

- ***Edit Patient:*** The user with permission to this role should be able to edit any field of a Patient, except for the unique code and the file folder, once a unique code has been assigned, these attributes cannot be changed..

- ***Delete Patient:***  The user with permission for this role should be able to delete a patient. This should remove from the database and the file folder with all the patient's files

**Manage Users:** Only an administrator user has permission to these functions.

- ***New User:*** The user should be able to create a new user with the following fields:
  1. Fist Name
  2. Last Name
  3. Email
  4. Cellphone Number
  5. Type of user
  6. A temporary password (This password is temporary for the new user, the new user must access the system and change it)

- ***Delete User:***  The user with permission for this role should be able to delete a User.

- ***Change Email/password of user:*** If the user to authenticate is a new user who still has his temporary password, he must be redirected to a form where he can set a new password). All users should have a section where they can see their personal data, and be able to change their email and password.
