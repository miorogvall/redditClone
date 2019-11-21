# redditClone
This is a reddit clone for viewing subreddits built in Vue


# Steps to get it running:

You may start at step 3 if you have git and node installed on your machine.

1. install npm :https://nodejs.org/en/ install the lts version
2. install git: https://git-scm.com/download/win (may need restart)
3. go to your git folder
4. run `git clone https://github.com/miorogvall/redditClone.git`
5. go into the folder reddit-clone
6. switch branch to `bf-showcase`
7. `npm install` in the reddit-clone folder
8. run `npm run serve` inside the reddit-clone folder
9. output is served on http://localhost:8080/ usually, check your terminal otherwise

# Recommended browser
The app was build and tested on Chrome and Windows 10 OS. I recommend using this to view the app since i didnt have so much time adding browser support.

# Notes about functionality
1. Typing a subreddit in search will reload the page after you've stopped typing.<br>
2. Changing the amount of posts visible will reload the feed.<br>
3. Getting the previous page will be grayed out if you are on the first page already<br>
4. some videos can not be served(localhost will be denied due to CORS), in that case an image is loaded, or nothing at all.

# About
It was build in Vue using sass and components. sass for the components is in each component file. Each component file consists of the template, script and style tag for that individual component.
