![documentations](http://getprowl.com/assets/images/documentation1.png)
<h1 align="center">Using Prowl with Docker Compose</h1>

Start the Prowl Docker container 

<pre>docker -t prowl</pre> 

One thing that hasn't been mentioned in the Prowl documentation is kernel compatibility, if you're having starting a contrainer, you can run

<pre>curl https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh > check-config.sh
bash ./check-config.sh</pre> 

Restart the Docker daemon 

<pre>sudo service docker restart</pre>

