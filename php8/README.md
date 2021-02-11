# Docker-PHP

Run app
---

### Creating docker image
This need to be created directly in folder of this project
```bash
docker build . --no-cache -t php-dev
```

### Ubuntu Linux
#### Alias php-docker machine
```bash
echo 'alias php-docker="docker run --rm --interactive --tty --volume ~/.gitconfig:/etc/gitconfig --volume \$PWD:/usr/src/app --user \$(id -u):\$(id -g) -p 0.0.0.0:80:80 php-dev"' >> ~/.bashrc
```

#### Alias php-docker-cmd machine for running commands
```bash
echo 'alias php-docker-cmd="docker run --rm --interactive --tty --volume ~/.gitconfig:/etc/gitconfig --volume \$PWD:/usr/src/app --user \$(id -u):\$(id -g) php-dev"' >> ~/.bashrc
```

### MacOS
#### Alias php-docker machine
```bash
echo 'alias php-docker="docker run --rm --interactive --tty --volume ~/.gitconfig:/etc/gitconfig --volume \$PWD:/usr/src/app -p 80:80 php-dev"' >> ~/.profile
```

#### Alias php-docker-cmd machine for running commands
```bash
echo 'alias php-docker-cmd="docker run --rm --interactive --tty --volume ~/.gitconfig:/etc/gitconfig --volume \$PWD:/usr/src/app php-dev"' >> ~/.profile
```

### Prepare server
```bash
php-docker-cmd composer create-project symfony/skeleton my_project
```

### Run server
```bash
php-docker
```


### Check running in browser
```bash
http://0.0.0.0:80
```
