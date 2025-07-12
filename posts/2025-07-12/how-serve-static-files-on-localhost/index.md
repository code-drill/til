<!--
.. title: how serve static files on localhost
.. slug: how-serve-static-files-on-localhost
.. date: 2025-07-12 21:51:27 UTC+02:00
.. tags: development,localhost,http,server,python,npx,docker
.. category: development
.. link: 
.. description: 
.. type: text
-->

How spin up http server to serve static files in a few seconds.


### Using `python`

```shell
python -m http.server 8000 --bind 127.0.0.1
python -m http.server --directory /path-to-dir-with-static-files/
python -m http.server 8000 --directory /path-to-dir-with-static-files/
python -m http.server 9106 --directory /path/to/dir/with/static/files
```

### Using `npx`

```shell
npx http-server -p 7890 --cors="*" /path/to/dir/with/static/files
```

### Using `docker`

- and `nginx`
	- on windows
	```shell
	# Serve files from current directory
	docker run -it --rm -p 8080:80 -v %cd%:/usr/share/nginx/html nginx:alpine 
	 
	# Serve files from specific directory
	docker run -it --rm -p 8080:80 -v /path/to/your/files:/usr/share/nginx/html nginx:alpine
	```
    - on linux
	```shell
	# Serve files from current directory
	docker run -it --rm -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx:alpine

	# Serve files from specific directory
	docker run -it --rm -p 8080:80 -v /path/to/your/files:/usr/share/nginx/html nginx:alpine
	```
- or `apache`
    - on windows
	```shell
	# Serve files from current directory
	docker run -it --rm -p 8080:80 -v %cd%:/usr/local/apache2/htdocs httpd:alpine

	# Serve files from specific directory
	docker run -it --rm -p 8080:80 -v /path/to/your/files:/usr/local/apache2/htdocs httpd:alpine
	```
	- on linux
	```shell
	# Serve files from current directory
	docker run -it --rm -p 8080:80 -v $(pwd):/usr/local/apache2/htdocs httpd:alpine

	# Serve files from specific directory
	docker run -it --rm -p 8080:80 -v /path/to/your/files:/usr/local/apache2/htdocs httpd:alpine
	```


