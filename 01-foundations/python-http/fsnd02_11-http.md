# HTTP requests and responses

**Udacity Full Stack Web Developer Nanodegree program**

Part 02. Developer Tools

Lesson 11. HTTP requests and responses (from free course [HTTP & Web Servers](https://www.udacity.com/course/http-web-servers--ud303))

Brendon Smith

br3ndonland

Also see cs50 Lecture 06 HTTP

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Intro](#intro)
- [Your first web server](#your-first-web-server)
- [Parts of a URI](#parts-of-a-uri)
- [Hostnames and ports](#hostnames-and-ports)
- [HTTP GET requests](#http-get-requests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## Intro

* We will use bash, Python 3, Git, and the nmap network testing toolkit.
* Installed nmap with `brew install nmap`.
* Used the `ncat` command to create a local tcp connection.
  - Terminal tab 1: `$ ncat -l 9999` (server)
  - Terminal tab 2: `$ ncat localhost 9999` (client)
  - The two Terminals will be able to communicate.


## Your first web server

> * An HTTP transaction always involves a client and a server. 
> * HTTP was originally created to serve hypertext documents, but today is used for much more. As a user of the web, you're using HTTP all the time.
> * Applications that you use in a web browser, and mobile services, are both likely to use HTTP. But low-level network tests such as ping do not.

* We used the Python `http.server` module as a demo webserver.
  ```bash
  $ cd /Users/br3ndonland/Dropbox/Computing/udacity-fsnd/02-tools
  $ python -m http.server 8000
  ```
* Browsing to http://localhost:8000/ shows the directory structure.
* We reviewed common HTTP status codes like `200` and `404`.
  > 404 is the HTTP status code for "Not Found". On Highway 101, not far from the Udacity office in Mountain View, there's a sign that tells the distance to Los Angeles. As it happens, it's 404 miles from Mountain View to Los Angeles, so the sign says Los Angeles 404. And so, every web programmer in Silicon Valley has probably heard the "Los Angeles Not Found" joke at least once.


## Parts of a URI

* URI: Uniform Resource Identifier. Tells a web browser where to go.
* URL: URI for a network resource. [Full list](http://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml).
* URI has three parts:
  - Scheme: https
  - Hostname: en.wikipedia.org.
  - Path: /wiki/Fish


## Hostnames and ports

* Domain Name Service/Server/System (DNS) translates IP addresses into the text addresses we see. 
* IPv4 localhost is `127.0.0.1`. 
* IPv6 localhost is `::1`, which is short for `0000:0000:0000:0000:0000:0000:0000:0001`.
* Some URIs imply the port number. HTTP implies port 80, HTTPS 443
* Information sent over the internet is divided into packets. Each packet contains the sending and receiving computer.


## HTTP GET requests

* All HTTP requests have an action verb:
  - GET
  - POST
  - PUT
  - PATCH
  - DELETE
* They also have the resource path requested, the HTTP protocol used, and the status code.
  ```
  127.0.0.1 - - [05/Dec/2017 12:36:45] "GET /NotExistyFile HTTP/1.1" 404 -
  127.0.0.1 - - [05/Dec/2017 14:32:10] "GET /img/ HTTP/1.1" 200 -
  127.0.0.1 - - [05/Dec/2017 14:32:16] "GET /img/fsnd02_10-github-pr-screenshot01.png HTTP/1.1" 200 -
  ```
* We did a manual HTTP request to our Python Web Server, pressing enter twice after the `localhost` line. The response contains the status line, the headers, and the response body.:
  ```
  $ ncat 127.0.0.1 8000
  GET / HTTP/1.1
  Host: localhost

  HTTP/1.0 200 OK
  Server: SimpleHTTP/0.6 Python/3.6.3
  Date: Tue, 05 Dec 2017 19:57:38 GMT
  Content-type: text/html; charset=utf-8
  Content-Length: 1107

  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <title>Directory listing for /</title>
  </head>
  <body>
  <h1>Directory listing for /</h1>
  <hr>
  <ul>
  <li><a href="fsnd02_01-shell.md">fsnd02_01-shell.md</a></li>
  <li><a href="fsnd02_02-git-intro.md">fsnd02_02-git-intro.md</a></li>
  <li><a href="fsnd02_03-git-init.md">fsnd02_03-git-init.md</a></li>
  <li><a href="fsnd02_04-git-log.md">fsnd02_04-git-log.md</a></li>
  <li><a href="fsnd02_05-git-commit.md">fsnd02_05-git-commit.md</a></li>
  <li><a href="fsnd02_06-git-tag-branch.md">fsnd02_06-git-tag-branch.md</a></li>
  <li><a href="fsnd02_07-git-undo.md">fsnd02_07-git-undo.md</a></li>
  <li><a href="fsnd02_08-github-remotes.md">fsnd02_08-github-remotes.md</a></li>
  <li><a href="fsnd02_09-github-fork.md">fsnd02_09-github-fork.md</a></li>
  <li><a href="fsnd02_10-github-pull-rebase.md">fsnd02_10-github-pull-rebase.md</a></li>
  <li><a href="fsnd02_11-http.md">fsnd02_11-http.md</a></li>
  <li><a href="img/">img/</a></li>
  </ul>
  <hr>
  </body>
  </html>
  ```
* The server tells us a few pieces of information, including that the status is ok, and that the content returned is an HTML document written in UTF-8 text. The header ends with a blank line.
* We tried our own call and response. 
  - `$ ncat -l 9999` 
  - Browse to http://localhost:9999/
  - The request shows up in the terminal
  - Type the following text into the terminal window where you started the server with `ncat`. Hit enter once after each line, then hit enter twice at the end.
    ```
    HTTP/1.1 200 OK
    Content-type: text/plain
    Content-length: 50

    Hello, browser! I am a real HTTP server, honestly!
    ```
  - The message shows up in the browser window. Cool!