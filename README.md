# PHD

**Pretty Handy Deployment™**

Have you ever wished deploying to a shared Apache/PHP host could be as easy as deploying to [Heroku](https://www.heroku.com)? Well, wish no more. Follow this simple guide and your life will instantly get better.

### Featuring...
* Environment variables
* Autamatic deployment with Git
* Using [Composer](https://getcomposer.org/) for dependency management

_This setup has been confirmed to work on DreamHost. Your mileage may vary._

## Installation

1. Make sure you have SSH enabled on your hosting user
2. SSH into your hosting account
3. CD into your domain directory and make sure it's empty
4. Clone this repo (`git clone https://github.com/stephendavis89/PHD.git .`)

## Deploying with Git

1. Create a git directory (`mkdir -p ~/git && cd ~/git`)
2. Create a project directory (`mkdir -p myproject.git && cd myproject.git`)
3. Create an empty remote Git repository (`git init --bare`)
4. Create the post-receive hook
  ```sh
  mv hooks/post-receive.sample hooks/post-receive
  chmod +x hooks/post-receive
  ```
5. Edit the post-receive hook (`nano hooks/post-receive`) and insert the following content:
  ```sh
  #!/bin/sh

  echo "Deploying to production..."
  GIT_WORK_TREE=~/myproject.com/deploy git checkout -f
  echo "Success!"
  ```

6. Add the remote repository to your project
  From your local project directory, run:
  ```sh
  git remote add production ssh://user@host/~/git/myproject.git
  ```

To set up an SSH key for password-less deployment, follow [this excellent guide](https://www.digitalocean.com/community/articles/how-to-set-up-ssh-keys--2).

## Using Composer

To get Composer working on DreamHost, follow [this guide](https://github.com/Braunson/dreamhost-composer-install).

To access the composer binary directly, run this from the directory you installed Composer into:

```sh
mkdir -p ~/bin
mv composer.phar ~/bin/composer
```

And add this to your .bash_profile:

```sh
export PATH=~/bin:$PATH
```

Don't forget to insert `composer install` into your post-receive hook for maximum good times.