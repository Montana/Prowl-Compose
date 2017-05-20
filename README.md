![documentations](http://getprowl.com/assets/images/documentation1.png)
<h1 align="center">Using Prowl with Docker Compose</h1>

Start the Prowl Docker container 

<pre>docker -t prowl</pre> 

One thing that hasn't been mentioned in the Prowl documentation is kernel compatibility, if you're having starting a contrainer, you can run

<pre>curl https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh > check-config.sh
bash ./check-config.sh</pre> 

Restart the Docker daemon 

<pre>sudo service docker restart</pre>

Verify that Docker can resolve external IP addresses by trying to pull an image

<pre>docker pull hello-world</pre>

Make sure UFW is up 

<pre>ufw status</pre> 

You may get this error

<pre>Your kernel does not support cgroup swap limit capabilities</pre> 

You can ignore this, but you do need edit your GRUB file in Ubuntu, so run 

<pre>nano /etc/default/grub</pre>

Then 

<pre>GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"</pre>

Then update GRUB 

<pre>sudo update-grub</pre>

Now let's create a test directory for a test project 

<pre>mkdir composetest
cd composetest</pre>

Create a file called app.py in your project directory and paste this in

<pre>from flask import Flask
from redis import Redis

app = Flask(__PROWL__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = redis.incr('hits')
    return 'Hello World! I have been seen {} times.\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)</pre>

Above, Redis is the hostname of the redis container on the application’s network. We use the default port for Redis, 6379. Create another file called requirements.txt in your project directory and paste this in

<pre>flask
redis</pre>

<p align="center">
  <img src="http://www.getprowl.com/assets/images/wewe.png">
</p>

<h1 align="center">Creating a Dockerfile</h1>

You can make a file called "Dockerfile" and make the following contents:

<pre>FROM python:3.4-alpine
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD ["python", "app.py"]</pre>

We are going to define services in the Docker Compose file. So create a YAML file called docker-compose.yml, in your project directory and paste the following

<pre>version: '2'
services:
  web:
    build: .
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: "redis:alpine"</pre>
    
From your project directory, start your app

<pre>docker-compose up
 Pulling image redis...
 Building web...
 Starting composetest_redis_1...
 Starting composetest_web_1...
 redis_1 | [8] 02 Jan 18:43:35.576 # Server started, Redis version 2.8.3
 web_1   |  * Running on http://0.0.0.0:5000/
 web_1   |  * Restarting with stat</pre>
 
 

