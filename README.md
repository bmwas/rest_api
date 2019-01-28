# rest_api
A few steps to create a REST API with Python/FLASK, NGINX, UWSGI and the Swagger 2.0 standards (Instructions are for FEDORA LINUX)

1) Install python dev, gcc and nginx

sudo yum groupinstall "Development Tools" # or use dnf install below.

or

sudo dnf install python-pip python-devel gcc nginx

2) Install python's virtual env

sudo pip install virtualenv

3) create virtual environment

virtualenv myapi

4) configure nginx (can use gedit or nano)

sudo gedit /etc/nginx/nginx.conf

5) Add the following lines within the http or below the server block.Replace 'user1' with your username. server_name can be a domain or ip (i.e. server_domain_or_IP)
 server{
    listen	 80;
    server_name  domain_or_IP_Address;

    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}


6) Add  nginx user to your user group

sudo usermod -a -G user1 nginx

7) Test  Nginx configuration file for syntax errors

sudo nginx -t

8) If no syntax errors from # 7 above (start and enable nginx)

sudo systemctl start nginx

sudo systemctl enable nginx

9) activate virtual environment

source myprojectenv/bin/activate

10) Install flask (Python microframework) 

pip install flask


11) Install connexion (Swagger/OpenAPI First framework for Python) -https://github.com/zalando/connexion

pip install connexion --ignore-installed PyYAML

12) Note: May need to install connextion with swagger ui ( if need be - see 12 below).

pip install connexion[swagger-ui]

13) Install a web server gateway interface (uWSGI) - This example only works with uWSGI ( other options include gunicorn etc). 

conda config --add channels conda-forge

conda install uwsgi

14)Make sure you inset the following lines of code at the bottom of the main python/flask code (assumes function is called 'app')

app = connexion.App(__name__)

app.add_api('app.yaml')

application = app.app

if __name__ == '__main__':

    app.run(port=8080)

15) Execute app

uwsgi --http :8080 -w app
