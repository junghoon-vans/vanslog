---
title: "Build Web Ide Using Code Server"
translationKey: posts/infra/build-web-ide-using-code-server
date: 2020-12-03T04:34:19Z
series:
  - General
categories:
  - General
tags:
  - General
  - Code-server
draft: false
summary: >
  Create your own web IDE
---

Introduction
---

Recently, `Web IDE`, a development environment accessed via the web, is being actively used. Representative examples include `Cloud9`, `Codeanywhere`, and `CloudIDE`, and GitHub is also preparing a similar service under the name [Codespace](https://github.com/features/codespaces). I liked that VSCode was available right on the web, so I signed up for a preview, but unfortunately I didn't win.

So, while looking for a method, I found an open source called `code-server`. This service is a project that allows VS Code to run on a web server. Another advantage is that it can be used without conditions as it is open source provided as `MIT License`. It was the best option for me who wanted to code without restrictions in the military.

Building a development environment
---

If you think that the installation process will be as long as the introduction was, you would be greatly mistaken. Installation is very simple. I tried installing it by referring to other blog posts, but the installation method shown in [GitHub repo](https://github.com/cdr/code-server) is easier and faster than other methods.

### Install and Run
```bash
curl -fsSL https://code-server.dev/install.sh | sh
sudo systemctl enable --now code-server@$USER #
```

If you run the above code, you will install `code-server` and complete the process of registering it to run automatically when the system boots. At the same time, `code-server` will already be in operation, but cannot be used immediately, because by default, only `localhost(127.0.0.1)` is allowed to access. Therefore, settings to enable access from outside are required.

### Allow external access and set password
```bash
vi ~/.config/code-server/config.yaml
```

Basically, the configuration file for `code-server` is the `config.yaml` file located in the above path.

```yaml
bind-addr: 0.0.0.0:8080
auth: password
password: #
cert: false
```

- `bind-addr`: Corresponding service address
- `auth`: Whether to set a password or not
- `password`: Specify password
- `cert`: Certificate settings

> Changing the `bind-addr` value from `127.0.0.1` to `0.0.0.0` means allowing access from outside.

```bash
sudo systemctl restart code-server@$USER
```When all settings are complete, rerun `code-server`. You can now access `ip-address:8080` in your browser. If you want to connect with just the address without entering the port number, please follow the steps below.

### Building Nginx

`Nginx` is the same web server software as `Apache`. While Apache was widely used in the past, many of the recently created projects tend to be composed of Nginx. It is lightweight and has the advantage of being able to handle more processes than Achapi.

This Nginx provides a function called `forward proxy`, which allows you to access `code-server`, whose service port is 8080, using only the server IP address.

#### bind-addr initialization

```bash
vi ~/.config/code-server/config.yaml # bind-addr: 127.0.0.1:8080
sudo systemctl restart code-server@$USER
```

For this process, `bind-addr` of `code-server` must be reset to localhost.


#### Install Nginx

```bash
sudo apt install -y nginx
```

#### Add the settings below to the `/etc/nginx/sites-available/code-server` file (sudo permissions)

```nginx
server {
 listen 80;
 listen [::]:80;
 server_name mydomain.com; #

 location / {
 proxy_pass http://localhost:8080/;
 proxy_set_header Host $host;
 proxy_set_header Upgrade $http_upgrade;
 proxy_set_header Connection upgrade;
 proxy_set_header Accept-Encoding gzip;
 }
}
```

> This setting listens on port 80 and passes the proxy to port 8080 of localhost.

#### Apply settings

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s ../sites-available/code-server /etc/nginx/sites-enabled/code-server
sudo systemctl reload nginx.service
```

If the settings have been applied correctly, you will be able to use `code-server` in the browser using only the IP address from now on.

### Version update

Even while writing this post, the code server version was updated from `v3.7.3` to `v3.7.4`. It was a small update with a few bug fixes and feature additions, but `code-server` notified me that a new version had been released every time I accessed it. Updating the version is very simple.

```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

If you run the same code used during installation, it will be applied immediately. Continuous and rapid updates like this are a big advantage because this project is open source. I am grateful that I can use such a great development environment for free.

Additional settings
---

In this way, I installed `code-server` on a personal server and created an environment that can be accessed through a web browser. The official documentation also recommends encrypting traffic with HTTPS through tools such as `Nginx`. You can find more details in [the official guide](https://github.com/cdr/code-server/blob/v3.7.4/doc/guide.md). This method requires a domain name, so you can skip it if you do not want to pay for one.
