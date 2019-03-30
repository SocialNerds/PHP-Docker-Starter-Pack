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
- You need to have [Docker](https://docs.docker.com/engine/installation/) installed

Run in root folder,
~~~~
cp .env.example .env
docker-compose build && docker-compose up -d
~~~~

Go to your browser on 127.0.0.1 to check that everything is working correctly.
You should see,
~~~~
Hello world!
~~~~

Start developing your PHP application in ./data/www

Login to the container,
~~~~
docker exec -it app_server /bin/bash -c "TERM=$TERM exec bash"
~~~~

# By SocialNerds
* [SocialNerds.gr](https://www.socialnerds.gr/)
* [YouTube](https://www.youtube.com/SocialNerdsGR)
* [Facebook](https://www.facebook.com/SocialNerdsGR)
* [Twitter](https://twitter.com/socialnerdsgr)