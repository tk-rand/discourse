# Discourse Developer Install Guide

If you'd like to set up a development environment for Discourse, the easiest way is by using a virtual machine.

The following instructions will automatically download and provision a virtual machine for you to begin hacking
on Discourse with:

### Getting Started

1. Install VirtualBox: https://www.virtualbox.org/wiki/Downloads
2. Install Vagrant: https://www.vagrantup.com/
3. Open a terminal
4. Clone the project: `git@github.com:discourse/discourse.git`
5. Enter the project directory: `cd discourse`

### Using Vagrant

When you're ready to start working, boot the VM:
```
vagrant up
```

Vagrant will prompt you for your admin password. This is so it can mount your local files inside the VM for an easy workflow.

(The first time you do this, it will take a while as it downloads the VM image and installs it. Go grab a coffee.)

Once the machine has booted up, you can shell into it by typing:

```
vagrant ssh
```

### Keeping your VM up to date

Now you're in a virtual machine is almost ready to start developing. It's a good idea to perform the following instructions
*every time* you pull from master to ensure your environment is still up to date. 

```
bundle install
bundle exec rake db:migrate
bundle exec rake db:seed_fu
```

### Starting Rails

Once your VM is up to date, you can start a rails instance using the following command:

```
bundle exec rails server
```

In a few seconds, rails will start server pages. To access them, open a web browser to http://localhost:4000 - if it all worked you should see discourse! Congratulations, you are ready to start working!

You can now edit files on your local file system, using your favorite text editor or IDE. When you reload your web browser, it should have the latest changed.

### Guard + Rspec

If you're actively working on Discourse, we recommend that you run Guard. It'll automatically run our unit tests over and over, and includes support
for live CSS reloading.

To use it, follow all the above steps. Once rails is running, open a new terminal window or tab, and then do this:

```
vagrant ssh
bundle exec rake db:test:prepare
bundle exec guard -p
```

Wait a minute while it runs all our unit tests. Once it has completed, live reloading should start working. Simply save a file locally, wait a couple of seconds and you'll see it change in your browser. No reloading of pages should be necessary for the most part, although if something doesn't update you should refresh to confirm.


### Sending Email

Mail is sent asynchronously by Sidekiq, so you'll need to have sidekiq running to process jobs. Run it with this command:

```
bundle exec sidekiq
```

Mailcatcher is used to avoid the whole issue of actually sending emails: https://github.com/sj26/mailcatcher

To start mailcatcher, run the following command in the vagrant image:

```
mailcatcher --http-ip 0.0.0.0
```

Then in a browser, go to http://localhost:4080

Sent emails will be received by mailcatcher and shown in its web ui.

### Shutting down the VM

When you're done working on Discourse, you can shut down Vagrant like so:

``` 
vagrant halt
```

