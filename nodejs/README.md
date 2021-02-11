# Docker-Node

Run app
---

### Creating docker image
This need to be created directly in folder of this project
```bash
docker build . --no-cache -t node-js
```

### Alias node-docker machine
```bash
echo 'alias node-docker="docker run --rm --interactive --tty --volume \$PWD:/app -p 0.0.0.0:80:4000 --user \$(id -u):\$(id -g) node-js"' >> ~/.bashrc
```

### Alias angular-docker machine
```bash
echo 'alias angular-docker="docker run --rm --interactive --tty --volume \$PWD:/app -p 0.0.0.0:80:4200 --user \$(id -u):\$(id -g) node-js"' >> ~/.bashrc
```

### Alias ionic-docker machine
```bash
echo 'alias ionic-docker="docker run --rm --interactive --tty --volume \$PWD:/app -p 0.0.0.0:80:8100 --user \$(id -u):\$(id -g) node-js"' >> ~/.bashrc
```

### Alias ionic-docker-ssl machine
```bash
echo 'alias ionic-docker-ssl="docker run --rm --interactive --tty --volume \$PWD:/app -p 0.0.0.0:443:8100 --user \$(id -u):\$(id -g) node-js"' >> ~/.bashrc
```

### Alias react-docker machine
```bash
echo 'alias react-docker="docker run --rm --interactive --tty --volume \$PWD:/app --user \$(id -u):\$(id -g) -p 0.0.0.0:80:3000 node-js"' >> ~/.bashrc
```

### Alias vue-docker machine
```bash
echo 'alias vue-docker="docker run --rm --interactive --tty --volume \$PWD:/app --user \$(id -u):\$(id -g) -p 0.0.0.0:80:8080 node-js"' >> ~/.bashrc
```

### Alias node-docker-cmd machine for running commands
```bash
echo 'alias node-docker-cmd="docker run --rm --interactive --tty --volume \$PWD:/app --user \$(id -u):\$(id -g) node-js"' >> ~/.bashrc
```

### Prepare server
```bash
node-docker-cmd npm install
```

### Run server
```bash
node-docker npm start
```

### Check running in browser
```bash
http://0.0.0.0
```

### Angular specific commands
#### Installing my-app
Command can be used in any folder, but will create project in subfolder of that location
Ex. if you are in folder my-app, application will be in my-project/my-project folder
```bash
node-docker-cmd ng new my-project
```

#### Run server after installing my-project
Go to my-project folder and run the server
```bash
angular-docker ng serve --host 0.0.0.0 --port 4200 --disable-host-check
```

#### Run unit tests
```bash
node-docker-cmd ng test
```

#### Run end-to-end tests
```bash
node-docker-cmd ng e2e
```

### Ionic specific commands
#### Creating myApp
Command can be used in any folder, but will create project in subfolder of that location
Ex. if you are in folder my-app, application will be in my-project/my-project folder
```bash
node-docker-cmd ionic start myApp sidemenu
```

#### Run server after installing my-project
Go to my-project folder and run the server
```bash
ionic-docker ionic serve --host=0.0.0.0
```
for SSL version
```bash
ionic-docker-ssl ionic serve --host=0.0.0.0 --ssl
```

### React specific commands
#### Installing my-app
Command can be used in any folder, but will create project in subfolder of that location
Ex. if you are in folder my-app, application will be in my-app/my-app folder
```bash
node-docker-cmd create-react-app my-app
# or
node-docker-cmd yarn create react-app my-app
```

#### Run server after installing my-app
Go to my-app folder and run the server
```bash
react-docker yarn start
```
