# TehMusic

TehMusic is a simple collaborative music listening app. You set up
playlists, and it will cycle through everyone who's currently DJ'ing,
playing their songs.

Yes, this was created in response to turntable.fm's pivot to something
not as cool.

Right now, this only supports uploading your own music. So, make sure
you're good with copyrights and stuff. Eventually we'd like to add
support for things like SoundCloud and YouTube (ick ads). Turntable was
awesome cause it had a whole library, but something like that is
out-of-scope for a little project like this.

You have to host your own instance of TheMusic, you can't use mine.

## Architecture

TehMusic uses Firebase as a backend database and S3 for media storage.
There's a small server component used for uploads, authentication, and
cycling between DJ's. You can easily run it on a single Heroku dyno.

The front-end is all Ember, of course.

## Getting Started (for development)

First clone the repo, (or fork and clone) and install deps:

```bash
$ git clone http://github.com/zwily/tehmusic.git
$ cd tehmusic
$ npm install
```

### Configuration time!

TehMusic uses dot-env style configuration, for easy local and easy
heroku config:

```bash
$ cp .env-example .env
```

#### Choose a "join code"

Since this is only intended for casual use between friends, you need to
choose a "join code" people need to enter to sign up. Set this as
*JOIN_CODE* in `.env`.

#### Set up Firebase

Sign up for a new Firebase at http://firebase.com/. Set the root url of
your new Firebase as *FIREBASE_ROOT* in `.env`.

Then, go to the "Auth" tab of your firebase, and scroll down to the
"Firebase Secrets" section. Generate a secret, and set that in `.env` as
*FIREBASE_SECRET*.

Then, go to the "Security" tab of your firebase, and copy the contents
of `firebase-security-rules.json` into the provided editor, and click
"Save Rules".

#### Set up S3

If you don't already have an AWS account, create one at
https://aws.amazon.com/. Then create an S3 bucket, and enter the bucket
name as *S3_BUCKET*. Then, go to IAM in your AWS console and create an
IAM user that has read access to that bucket. Enter the credentials for
that IAM user as *S3_ACCESS_KEY_ID* and *S3_SECRET_ACCESS_KEY* in `.env`.

### Fire it up.

You can now do some local testing by starting your development server:

```bash
$ grunt server
```

Now open http://localhost:8000/ in a browser.

## Deploying to Heroku (for production)

Create yourself an account at http://heroku.com/ and install the
`heroku` command line tool. Then, inside your tehmusic checkout:

```bash
$ heroku apps:create
```

You'll get back a URL to hit your new app at, and it will add it as a
git remote in your git repository.

Now you need to configure you app. For every line in your `.env` file,
do `heroku config:set <KEY>=<VALUE>`.

Now you can push up your app:

```bash
$ git push heroku master
```

And then you should be live!

Heroku dynos will shut down after a few minutes of inactivity, which is
kinda sad for this app since it has a backend component that's always
talking to firebase. To get around that, you can sign up for Pingdom and
have it ping your app once a minute to keep the dyno running.

# Dedication

TehMusic is dedicated to the memory of our friend Benji, who was taken
too early by cancer. He had many passions, but one of his biggest was
music, and sharing it with his friends. RIP Benji.

