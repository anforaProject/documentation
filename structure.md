# Structure of the project

In this list I'll try to explain how the project works and what is where.

## What are you using?

The server side is running [Falcon](http://falconframework.org/) a framework to
build APIs using python. I've chosen Falcon because of how fast it is and because
of it extensibility.

As ORM we run [peewee](http://docs.peewee-orm.com/en/latest/). It's a mature
library, well manteined and documentated.

To manage different asyncronous task we run [huey](https://github.com/coleifer/huey).
It's small, simple and easy to setup. It runs over redis so we also need redis.


## DB structure

The main objects of the project are users and photos. We implement different
classes to represent each type of object in the db in the folder named `models`.

In addition one can find the representation for:

- Albums
- Hashtags
- liked_posts
- Following/Follower relations
- Tokens
- Users
- Photos

## API endpoints

The api endpoints are available under the `api`. Different subfolders represent
different versions of the API.

### Users

In this file we implement all the method relative to actions for users.

- Authenticate an user.
- Get the followers for an user.
- Get the different statuses of an user.
- Logout the current user.
- Get basic information of the current user.

### Statuses

## Security

To keep users passwords safe we only keep a hashed version of the passwords
using the `Aragon2` algorithm by a third-party library implementing it for
python.

To give access to different apps we use the token auth method. Over the falcon
library we use `falcon_auth` that implements several auth methods and we only
have to track the validity of the tokens and not about the logic to allow or not
access to different api endpoints.

## Some pipelines

### Upload images

When one starts the process to upload an image the steps are the following:

- First we call `/api/v1/statuses` with a `POST` http request following the info
in the API documentation. (`main.py`)
- An object representating the image is created in the db assigning a unique filename to the image uploaded. (`api/v1/photos.py`)
- Then the request to store the image is send to the corresponding queue and
a 200 HTTP response is created. (`tasks/tasks.py`)
- When the queue is ready it dispatches the process and stores the image
