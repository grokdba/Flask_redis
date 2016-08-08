# Flask_redis
flask and redis based web project try
###env install
>apt-get install python-pip
pip virtualenv
root@ubuntu-virtual-machine:/usr/local# which virtualenv
/usr/local/bin/virtualenv
root@ubuntu-virtual-machine:/var/wb# virtualenv venv
New python executable in /var/wb/venv/bin/python
root@ubuntu-virtual-machine:/var/wb/venv/bin# source activate
(venv) root@ubuntu-virtual-machine:/var/wb/venv/bin#
(venv) root@ubuntu-virtual-machine:/var/wb/venv/bin# pip list
pip (8.1.2)
setuptools (25.1.0)
wheel (0.29.0)
####flask 
changelog:

>(venv) root@ubuntu-virtual-machine:/var/wb/venv/bin# pip install flask
(venv) root@ubuntu-virtual-machine:/var/wb/venv/bin# pip list
click (6.6)
Flask (0.11.1)
itsdangerous (0.24)
Jinja2 (2.8)
MarkupSafe (0.23)
pip (8.1.2)
setuptools (25.1.0)
Werkzeug (0.11.10)
wheel (0.29.0)
apt-get install curl
root@ubuntu-virtual-machine:~# curl 127.0.0.1:5000
welcome wb
root@ubuntu-virtual-machine:~#
###
*run.py*
```python
(venv) root@ubuntu-virtual-machine:/var/wb# cat run.py
#! /usr/bin/
from wb  import app
app.run(debug=True)
```
tree
>(venv) root@ubuntu-virtual-machine:/var/wb/wb# tree
.
├── __init__.py
├── __init__.pyc
├── models.py
├── models.pyc
├── static
│   └── style.css
├── template
│   ├── home.html
│   ├── index.html
│   ├── profile.html
│   └── timeline.html
├── views.py
└── views.pyc
pthon run.py
curl 

####virtualmachine 

flask的app.run()方法运行服务器应用，默认是只能在本机访问的！！！
>Externally Visible Server
If you run the server you will notice that the server is only accessible from your own computer, not from any other in the network. This is the default because in debugging mode a user of the application can execute arbitrary Python code on your computer.

If you have debug disabled or trust the users on your network, you can make the server publicly available simply by changing the call of the run() method to look like this:

app.run(host='0.0.0.0')
This tells your operating system to listen on all public IPs.

####gunicorn 
pip install gunicorn 
apt-get install nginx
(venv) root@ubuntu-virtual-machine:/var/wb# which nginx
/usr/sbin/nginx
**conf**
> location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to index.html
                #try_files $uri $uri/ /index.html;

                try_files $uri @gunicorn_proxy;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
        location @gunicorn_proxy {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_redirect off;
                proxy_pass http://127.0.0.1:8000;
        }
###error handle
`[ERROR] Connection in use: ('localhost', 8000)`
reason:
>run.py unguardedly calls app.run - this actually calls werkzeug.serving.run_simple which starts a sub-process to handle incoming requests ... which you don't want to do when running under gunicorn (since gunicorn will handle the process management for you).
solution:
**modify** run.py 
```python
#! /usr/bin/
from wb  import app
if __name__ == '__main__':
    app.run(debug=True)
```
 Permission denied
nginx.conf:user root 
404:
nginx root /alias 




root@ubuntu-virtual-machine:/var/wb/wb/static# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful


####pip install supervisor


###redis install&connetction  
install tar.gz
tar zxvf tar.gz
make
cp redis* /usr/local/redis/
root@ubuntu-virtual-machine:/usr/local/redis# tree
>├── dump6379.rdb
├── dump6380.rdb
├── dump6381.rdb
├── dump.rdb
├── mkreleasehdr.sh
├── redis160726.aof
├── redis6379.cnf
├── redis6380.cnf
├── redis6381.cnf
├── redis-benchmark
├── redis-check-aof
├── redis-check-rdb
├── redis-cli
├── redis-sentinel
├── redis-server
├── redis-trib.rb
└── sentinel.conf
sentinel mode of server --single process 
python driver:
  pip install redis
ref:https://pypi.python.org/pypi/redis/
import: unable to open X server `' @ error/import.c/ImportImageCommand/366.

soloution:
#!/var/wb/venv/bin/python


###mysql table ==redis 
####user table 
>global:uid
userid:1:username
user:userid:1:password
user:username:xxx ID  冗余设计 
mysql--user: uid:username:password,primarykey(uid)
###post table 
>post
post:pid:1:time timestamp
post:pid:1:uid  xxxx
post:pid:1:content 'this is first post'
incr global:postid


unicode error:
?????????????????
UnicodeEncodeError: 'ascii' codec can't encode character u'\xe5' in position 37: ordinal not in range(1

          

















