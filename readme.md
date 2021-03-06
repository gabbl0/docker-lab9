## Laboratorium 9 Gabriela Błaszczak
Projekt składa się z pliku .yml docker compose i folderu /src w którym znajduje się kod strony php oraz dwa pliki konfiguracyjne dla nginx i mysql.
### Plik docker-compose.yml
version: '3.8'  
  
services:  
#------NGINX-------  
    nginx:  
        image: nginx:1.21  
        ports:  
            - 80:80  
        #wystawiony port 6666  
        expose:  
            - "6666"  
        volumes:  
            - ./src:/var/www/php  
            #plik konfiguracyjny nginx  
            - ./src/conf.d:/etc/nginx/conf.d  
        #sieci backendu i frontendu  
        networks:  
            - front-tier  
            - back-tier  
#--------PHP---------  
    php:  
        image: php:8-fpm  
        volumes:  
            -  ./src:/var/www/php  
        networks:  
            - back-tier  
#---------MYSQL---------  
    mysql:  
        image: mysql/mysql-server:8.0  
        volumes:  
            #plik konfiguracyjny  
            - ./src/my.cnf:/etc/mysql/conf.d/my.cnf  
            #wolumen  
            - mysqldata:/var/lib/mysql  
        environment:  
            MYSQL_ROOT_PASSWORD: root  
            MYSQL_ROOT_HOST: "%"  
            MYSQL_DATABASE: test  
        networks:  
            - back-tier  
#-------PHPMYADMIN-------  
    phpmyadmin:  
        image: phpmyadmin/phpmyadmin  
        environment:  
            PMA_HOST: mysql  
        ports:  
            - 6001:80  
        networks:  
            - back-tier  
  
volumes:  
    mysqldata:  
  
networks:  
    back-tier:  
    front-tier:  
