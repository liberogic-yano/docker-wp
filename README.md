# docker-wp
Nginx+MySQL+WordPress(WIP)  
Example image:)

## Install

### Step.1
Clone repository.
```
$ git clone https://github.com/liberogic-yano/docker-wp.git
```

### Step.2
go to the directory.
```
$ cd docker-wp
```

### Step.3
Create .env file.
```
$ nano .env
```

Edit `.env` file as below.
```
WP_WORKING_THEME_DIR=./wp-content/themes/custom
WP_THEME_DIR=/wp-content/themes/custom
```

## Run
```
$ docker-compose up -d
```

You can open `http://localhost/` in a web browser.


## Stop
```
$ docker-compose down
```

## Stop/Remove Volumes
```
$ docker-compose down --volumes
```