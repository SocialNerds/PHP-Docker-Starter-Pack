# Docker for PHP

This is a development environment for PHP.
It contains,
- Apache
- PHP-FPM
- GD
- Composer
- MariaDB
- phpMyAdmin

# Installation
Requirements
- You need to have [Docker](https://docs.docker.com/engine/installation/) and Docker Compose installed

Run in root folder,
~~~~
cp .env.example .env
docker-compose build && docker-compose up -d
~~~~

Go to your browser on 127.0.0.1/test to check that everything is working correctly.
You should see,
~~~~
Hello world!
~~~~

Start developing your PHP application in ./data/www/[NEW_FOLDER]

You may add new alias for this folder in ./http/vhosts/main.conf

Login to the container,
~~~~
docker exec -it app_fpm /bin/bash -c "TERM=$TERM exec bash"
~~~~

Exit the container,
~~~~
exit
~~~~

Shutdown containers
~~~~
docker-compose down
~~~~

# By SocialNerds
* [SocialNerds.gr](https://www.socialnerds.gr/)
* [YouTube](https://www.youtube.com/SocialNerdsGR)
* [Facebook](https://www.facebook.com/SocialNerdsGR)
* [Twitter](https://twitter.com/socialnerdsgr)
