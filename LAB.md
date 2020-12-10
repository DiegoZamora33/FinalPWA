# LABORATORY

## General description.

My project uses the *theme of a clinical analysis laboratory.*

It consists of a *main web page visible to all public users*, in this main page you can see laboratory publications, image gallery, leave comments, see news that the laboratory staff wishes to publish and if you are a client of the laboratory you can see and download your clinical analysis results with a unique code per client.

For the administrators of the site and the laboratory, they have an *authentication login to access the administrative functions of the site*, in my project I decided to establish 3 types of user that have access to the administration functions of the web system.

**Administrator User:** This user can create and delete users, manage customer files, manage the publications and images that are displayed on the main page of the site and manage the comments that customers write on the site.

**Chemical User:** This user can only manage customer files.

**Publisher User:** This user will only manage the publications of the main page of the website and the images that are displayed on the main page.


## Requirements.

**Main Page:** This is the main page of the site, accessible to everyone, it must be able to show a gallery of images, all the publications and image for each publication, contact information, ubicacion, hours and a section for public users to leave their comments for the laboratory staff .

- If I am a client of the laboratory, I should have a section on the main page to enter my unique code and I should be able to see my list of clinical results and download each one in pdf format.

- If I am a Site Administrator, the main page should have a button to redirect to the login page.

**Login Page:** The Administrator user, Chemical user and the publisher user should use a login page to authenticate with an email and password and then be redirected to the Administration page.

**Administrative Page:** Once the login authentication is completed successfully. The user should be redirected to this page, depending on the type of user the functions will be displayed according to the type of user that is authenticated.

**Manage Publications:** These functions must be restricted or allowed, depending on the logged in user.

- ***New Post:*** The user with permission to this function should be able to create a new post with the following fields:
  1. Title
  2. Body (the body text has to be saved in markdown format and displayed on the main page converted to HTML format)
  3. An image to accompany the publication
  4. An image to display in the image gallery on the main page (optional)

- ***Edit Post:*** The user with permission to this function should be able to edit any field of a post.

- ***Delete Post:*** The user with permission to this function should be able to delete a post.










