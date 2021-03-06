# blog.marcopeg.com

I'm going to use this repository to:

- practice with Wordpress theme development
- practice with Wordpress Docker environment
- practice with Docker and DockerHumble project

## Release Codename

[each release is an italian type of dry pasta](https://en.wikipedia.org/wiki/List_of_pasta)

## Tech Stack

This is a WordPress website that is wrapped with Docker.  
In order to work with it you need:

- [Docker](https://www.docker.com/get-docker)
- [HumbleCLI](https://github.com/marcopeg/humble-cli)

## Boot for Development

`cd` into the project directory and run `humble boot` to start the development environment.

This will get you:

- The project running on port 8080
- PhpMyAdmin running on port 8081
- A file system listener that will rebuild the styles automatically

## Work on the theme

The custom theme and plugins are located in `services/wordpress/wp-content`.

In development mode (default when you clone the repo) the theme and plugins are
mounted as volumes so you can change the files and simply reload the page to see changes.

## Work on the styles

The styles SASS sources for the custom theme are located in `services/assets/src`.

The style project is based on _TwitterBootstrap_ and is automatically compiled into
`services/wordpress/wp-content/themes/docker/style.css` every time you change a file.

## Test for Production

The production version of the project builds a custom Docker image and runs it.

First you need to create a local enviroment file `.env.local` where to write the
production configuration:

```
HUMBLE_ENV=prod
WP_MIGRATE_FROM=https://marcopeg.com
```

At this point you can build the images:

```
humble build --no-cache
```

And run the project:

```
humble boot
```

## Data Folder

All the volatile data is store in `data/${environment-name}`.

If you want to completely reset the project's state you can do:

```
humble down
rm -rf data/development
humble boot
```

## Backup / Restore

You can also create a backup of your project's state at any point in time:

```
humble backup
```

this will generate a new folder like: `data/backup/development.20180502.082724`

To restore from a specific backup you can run:

```
humble restore development.20180502.082724
```

## Git Strategy

The project management is done with GitHub issues visualized as a Waffle board:

https://waffle.io/marcopeg/marcopeg.com

1. **Create a Task**

2. **Pick a Task**

When you start to work on a task move it under the `In Progress` column and assing it
to yourself.

3. **Create a Freature Branch**

In your local copy of the project create a new branch named `feature/${taskId}` using
the issue id generated by GitHub.

Push this branch so that is visible in the branches page in GitHub.

4. **Work and Commit**

When you are ready to commit please test your solution in production mode so to be sure
that it will be working on the server as well.

When everything works well prepare a commit a prefix the message with `#${issueId}` so to
link it to the issue in GitHub:

```
git add .
git commit -m "#22 Changed a color"
git push
```

5. **Prepare a Pull Request**

When your iteration over a task is completed make one more production test just to be on the
safe side then open a new Pull Request.

The title should briefly explain the business value that the PR brings to the project.

The body of the PR should:

- link to the issue/issues that the PR is meant to solve
- describe how to test the code

6. **Name a Peer for Review**

Then the PR is ready just select a collaborator that will review, test and merge the PR.


