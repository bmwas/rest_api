# rest_api
A few steps to create a REST API with Python/FLASK, NGINX, UWSGI and the Swagger 2.0 standards (Instructions are for FEDORA LINUX)

1) Install python dev, gcc and nginx

dnf install python-pip python-devel gcc nginx

2) Install python's virtual env

sudo pip install virtualenv

3) create virtual environment

virtualenv myapi

4) activate virtual environment

source myprojectenv/bin/activate

5) Install flask (Python microframework) 

pip install flask

6) Install connexion (Swagger/OpenAPI First framework for Python) -https://github.com/zalando/connexion

pip install connexion --ignore-installed PyYAML

7) Install a web server gateway interface (uWSGI) - This example only works with uWSGI ( other options include gunicorn etc). 

conda config --add channels conda-forge

conda install uwsgi

8)Make sure you inset the following lines of code at the bottom of the main python/flask code (assumes function is called 'app')

app = connexion.App(__name__)
app.add_api('app.yaml')
application = app.app
if __name__ == '__main__':
    app.run(port=8080)

9) Execute app

uwsgi --http :8080 -w app


