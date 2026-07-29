FROM    unbuntu:lastest

RUN 	apt-get update -y
RUN     apt-get install -y nginx

COPY    ./site/ /usr/share/nginx/html/

EXPOSE  81

CMD     ["nginx", "-g", "daemon off;"]