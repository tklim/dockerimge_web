# dockerimge_web
Dockerfile to build an web server image

vi Dockerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
EXPOSE 80
RUN yum -y install httpd procps-ng && yum clean all
COPY src/ /var/www/html
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]

mkdir src
cd src
vi index.html (put "Simple index html")
cd ..
podman build -t web:1.0 .
# podman images
REPOSITORY                           TAG         IMAGE ID      CREATED         SIZE
localhost/web                        1.0         7c834bfd84b1  10 minutes ago  242 MB
registry.access.redhat.com/ubi8/ubi  latest      27e761650ada  2 weeks ago     215 MB
podman run -d --name web -p 8080:80 localhost/web:1.0
podman ps
podman stop web

podman login https://quay.io/user/sainstklim/
podman push localhost/web:1.0 quay.io/sainstklim/web:1.0
