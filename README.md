#Usage

##1.Generate a new pair of keys just for that 

It's not the safest deployment method (ssh private keys in environment variables !) , so you'd better generate a pair of deployment keys just for the occasion

	$ ssh-keygen -f ~/.ssh/heroku

##2.Setup your deploy keys on the private repos you're using

Configure the public deploy key (the one here  ~/.ssh/heroku.pub) within your gitlab / github / bitbucket (check the corresponding documentation)...

##3.Setup your deploy key within heroku

###a.Set your deploy keys with heroku toolbelt from within your app dir

	$ heroku config:set SSH_KEY="$(cat ~/.ssh/heroku)"
	$ heroku config:set SSH_KEY_PUBLIC="$(cat ~/.ssh/heroku.pub)"


###b.This buildpack is to be chained at the beginning of a multi-buildpacks buildpacks chain like this one 
	$ heroku buildpack:set https://github.com/heroku/heroku-buildpack-multi.git
	$ heroku config:add BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-multi.git

###c.create your .buildpacks file : here for a nodejs app

$ cat .buildpacks
https://github.com/levequej/heroku-buildpack-ssh-deploy-from-private-git-ssh-repos.git
https://github.com/heroku/heroku-buildpack-nodejs.git

##4 Deal with git management in your package manager
here for NPM

	$cat .npmrc
	git=${PWD}/.ssh/git-ssh-command.sh



#All comments & suggestions are welcome

There's must  a better way to solve this simple problem and maybe  I'd better just have forked the nodejs official buildpack  :  I don't know I did not read it, but I needed a quick solution for my first heroku app to work asap,a nd I came up with that

I guess if you use a github private repo you'd much better use a OAuth token.

About to be ... Tested on linux to be used with a private bitbucket repo (I do not think they have Oauth for non team repos : do they ?)


