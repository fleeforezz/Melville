# Melville Design
We'll cover the following
+ [System Requirements](#System Requirements)
+ [[#Use Case Diagram]]
+ [[#Class Diagram]]
+ [[#Activity Diagrams]]
+ [[#Database]]
+ [[#Server Infrastructure]]
+ [[#CI/CD]]
+ [[#Code]]

Melville is an online reading website, which allow everyone to read all books from the internet. Users can upload their own book and share it for everyone, also they can save favorite book and read it later.

![[Pasted image 20250304131928.png]]
## System Requirements

We will focus on the following set of requirements while designing Melville:

#### Main Function

1. User should be able to login or signUp, also login and signup with google account.
2. User should be able to reset the password if forgot.
3. User should be able to deactivate their own account or delete user account and all data related to that account.
4. User should be able to search books.
5. User should be able to upload new books, view all the books that uploaded by them, edit uploaded books, remove uploaded book, download books, rating books and add book to favorite.
6. Only system admin can have full control over users, books includes (view, create, update, delete users, books, category, authors, etc...).
7. All uploaded images and book files will be stored to MinIO Server
8. All user password have to encrypted using Bcrypt.
9. The website should be able to send notifications to users whenever a reservation is made/modified or there is an update.

#### Deployment 

1. A separate MSSQL Server using docker
2. MinIO server to store our resources (user_image, author_image, book_file , etc....)
3. Use Gitlab as version control, our git repository will have 2 branch (1 for development, 1 for main)
4. Use Jenkins for CI
5. Use Dockerhub for docker registry
6. A git repository to store Kubernetes Manifest file
7. Use ArgoCD for CD
8. Deployment environment will be a kubernetes cluster with 2 namespaces (1 for testing, 1 for production)

## Use Case Diagram

We have 3 main Actors in our system:

+ **Admin:** Responsible for full control over users, books, categories
+ **Users:** Will be able to uploads books, view uploaded books, modified books, remove books, add favorite book, download book.
+ Guest: Can only view book and download books, but cannot add favorite, uploads books, view uploaded books, modified books, remove books,

Here are the top use cass of Melville website:

+ Search books: To search for books that user want to read
+ Create/Modify/View books: To upload a book, remove it, or view details about book.
+ Add favorite: To save book and read it later.
+ Download book: To download the book that user want to read it on their device.

Here is the use case diagram of our Melville website:

## Database

Our system will have total 12 table:
+ Users
+ Sex
+ Country
+ City
+ Books
+ BookCategory
+ BookCategoryMap
+ BookFormats
+ Authors
+ Favorite
+ UserBookMap
+ Rate

In this database design there are 2 key concept to notice: 
1. Users table:
	+ User can upload as many books as they like 
	+ User can add many favorite books to their bookshelf as they like
	+ User can rate many books as they like
2. Books table:
	+ 1 book can have many category
	+ 1 book can only have 1 author

![[Pasted image 20250304215114.png]]

## CI/CD

We will use CI/CD to automatic build test and deploy to server. Jenkins will automatic detect changed branch, then it will start run build steps, after run complete jenkins will start dockerize the webapp and push to DockerHub.

Here's the flow chart for Jenkins build steps:

![[Pasted image 20250304230750.png]]

## Code

All source code will store in gitlab, you can access with [this link](https://gitlab.fleeforezz.site/jso/melville.git)
