# Continuous Delivery

Now that we've leveled up our applications' code, let's level up our deployment workflow and infrastructure.

## Heroku Pipelines

We're still going to deploy to Heroku, but in a more robust way than we [first learned how to](https://chapters.firstdraft.com/chapters/775).

This time, we're going to take advantage of [Heroku Pipelines](https://devcenter.heroku.com/articles/pipelines) to easily manage _multiple_ deployment targets.

We'll have an app for our customers just like before, which we'll now call `production`; but we'll also have other apps: for beta testers, for demonstrating unreleased features, for quality assurance, for code review, etc

This will unlock powerful workflows wherein you can give and get continuous feedback on in-progress features every time you push a commit, even from non-technical stakeholders. Let's get started!

If you want to follow along, you can set up a workspace with any application that's ready to be deployed, even if it's a brand new one. I'll be using a repository that I created using the [minimal-heroku template](https://github.com/appdev-projects/minimal-heroku/generate).

## A deeper dive into Git remotes

First, let's dive deeper into Git "remotes".

Whenever we've been pushing code, we've been doing something like:

```
git push origin main
```

or just `git push` for short; which by default uses `origin` for the third part, and whichever branch you have checked out for the fourth part.

The third part is **the location that we want to send the code to**, known as **the "remote"**. You can list out all of the remotes that you've got with the `git remote` command:

```
gitpod /workspace/minimal-heroku:(main) $ git remote

origin
```

I happen to have two; you might have the same, or you might only have one, or you might have more. `origin` and `upstream` are just nicknames; we can see the actual URLs of the locations with `git remote -v`:

```
gitpod /workspace/minimal-heroku:(main) $ git remote -v

origin  https://github.com/raghubetina/minimal-heroku.git (fetch)
origin  https://github.com/raghubetina/minimal-heroku.git (push)
```

This lets us know for sure where our code is going to and coming from when we `push` and `pull`. Typically, `origin` is our primary repository on GitHub.com.

Most importantly, we can add new remotes with the `git remote add` command.

You don't need to do this part, but for demonstration purposes, I am going to add a remote. Let's say I want to make a redundant copy of the repository on [Gitlab.com](https://gitlab.com/), a GitHub alternative, and `push` to it from time to time for safekeeping.

First, I need to go to my Gitlab dashboard and create a repository. Then, they will assign the repository a Git URL, such as the following:

```
https://gitlab.com/raghubetina/minimal-heroku.git
```

Now I'm ready to add my remote and nickname it `gitlab`:


```
gitpod /workspace/minimal-heroku:(main) $ git remote add gitlab https://gitlab.com/raghubetina/minimal-heroku.git

gitpod /workspace/minimal-heroku:(main) $ git remote -v

gitlab  https://gitlab.com/raghubetina/minimal-heroku.git (fetch)
gitlab  https://gitlab.com/raghubetina/minimal-heroku.git (push)
origin  https://github.com/raghubetina-appdev/minimal-heroku.git (fetch)
origin  https://github.com/raghubetina-appdev/minimal-heroku.git (push)
```

Now I've got two remotes. I can `git push gitlab` whenever I choose, and my branch will be sent to Gitlab for safekeeping (provided [I set up authentication](https://docs.gitlab.com/ee/integration/gitpod.html#enable-gitpod-in-your-user-settings)).

The point is: we're not limited to just one storage location for our repositories; we can `add` as many `remote`s as we like, and sending our code to them is as simple as `git push`.

One last thing: you can see an overview of all of your remotes as well as some other Git configuration by opening the config file within the hidden `.git` folder:

```
open .git/config
```

Take a look at it, but be careful with this file; you don't want to make any errant keystrokes in here by mistake. I will sometimes edit the URLs of remotes directly in this file, but only very cautiously.

## Heroku is just another git remote

Now, back to Pipelines.

The thing that made Heroku revolutionary when [they stormed the scene in 2009](https://www.infoq.com/news/2009/05/heroku-provisionless-revolution/) was that they said "Okay, as long as you're sending code around with `git push`, send it to us that way too — and we'll provision a server for you, put your code on it, spin up a database, set up a connection pool, and do the 1001 other things needed to get your app up and running. You just need to add us as an additional `remote` and `git push` a commit whenever it's ready to ship."

So, when we run the `heroku create my-app-name` command, it actually does two things:

 1. Through Heroku's API, it creates an app and retrieves the assigned Git URL. If the app name we chose is available the Git URL would be `https://git.heroku.com/my-app-name.git`.

    We could do the same thing by going to our Heroku dashboard in the browser and creating a new app there.
 1. It adds a Git remote called `heroku`:
 
    ```
    git remote add heroku https://git.heroku.com/my-app-name.git
    ```

    We could do the same thing ourselves at the command line.

The presence of this remote is what enables us to deploy using:

```
git push heroku main
```

## Set up production app

From now on, we're going to stop using the default remote nickname of `heroku`. For our primary app, the one that our customers interact with, lets use the remote name `production`.

For the Heroku app name itself, my convention is end the name in `-production` or `-prod` — e.g., `my-app-name-production`.

When initially creating an app using the `heroku` command-line tool, you can choose a remote name using the `-r` option. 
    
```
gitpod /workspace/minimal-heroku:(main) $ heroku create minimal-heroku-production -r production

Creating ⬢ minimal-heroku-production... done
https://minimal-heroku-production.herokuapp.com/ | https://git.heroku.com/minimal-heroku-production.git
```

If you already have a remote named `heroku`, you can rename it:

```
git remote rename heroku production
```

If you don't already have a `production` app, create one now and deploy your `main` branch to it:

```
git push production main
```

Visit your application with `heroku open`. Is it working? If so, yay!

If not, [recall your old Heroku skills](https://chapters.firstdraft.com/chapters/775#seeing-error-messages), read the error messages with `heroku logs --tail`, and debug.

You probably need to `heroku run rails db:migrate`, for one thing. Possibly also `heroku run rails sample_data` if that makes sense for your application.

Woohoo! Even 10+ years later, it still brings a tear to my eye how much Heroku has simplified deployment 😢

## Set up staging app

Let's now add a second Heroku app. This one is going to be for testing purposes — let's say we're working on a feature, but it's not ready for customers yet, so we can't deploy it to `production`. However, we want to deploy it _somewhere_ so that non-technical teammates — say, usability or QA testers — can kick the tires and give us feedback.

Non-technical teammates aren't about to set up Gitpod workspaces, `bin/server`, `rails db:create`, `rails db:migrate`, `rails sample_data`, etc; so we're going to need to deploy another Heroku app for them to interact with the experimental branch. We'll call this one `staging`:

```
heroku create minimal-heroku-staging -r staging
```

Let's deploy to this one, too:

```
git push staging main
```

If we look at our list of remotes now, we should see `origin`, `production`, and `staging`:

```
gitpod /workspace/minimal-heroku:(main) $ git remote

origin
production
staging
```

If you try to open your staging application with `heroku open`, you should see an error:

```
gitpod /workspace/minimal-heroku:(main) $ heroku open

 ›   Error: Multiple apps in git remotes
 ›     Usage: --remote staging
 ›        or: --app minimal-heroku-staging
 ›     Your local git repository has more than 1 app referenced in git remotes.
 ›     Because of this, we can't determine which app you want to run this command against.
 ›     Specify the app you want with --app or --remote.
 ›     Heroku remotes in repo:
 ›     minimal-heroku-production (production)
 ›   minimal-heroku-staging (staging)
 ›
 ›     https://devcenter.heroku.com/articles/multiple-environments
```

The issue is: now that we have more than one Heroku app, we have to be more specific when we run our `heroku` commands about which location we want them performed on.

```
heroku open -r staging
```

And if you have errors like before, to run the same commands on `staging`, add the `-r staging` flag:

```
heroku logs --tail -r staging
heroku run rails db:migrate -r staging
heroku run sample_data -r staging
```

Adding the `-r production` or `-r staging` flag to every `heroku ...` command is a pain, but I'll show you some shortcuts soon.

## Create pipeline

Now that we have our two apps up and running, let's create a Heroku Pipeline for them. Head over to your Heroku dashboard and, first, confirm that your two new apps appear in the list there. Create a new pipeline from the dropdown in the top-right:

![](/assets/continuous-delivery-1-create-pipeline.png)

![](/assets/continuous-delivery-2-choose-name.png)

![](/assets/continuous-delivery-3-stages.png)

![](/assets/continuous-delivery-5-connect-github.png)

![](/assets/continuous-delivery-6-configure-review.png)
