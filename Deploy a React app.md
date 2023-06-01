#devops #programming #frontend #practical #react #nodejs #fedora

# Deploy to a cloud VPS

## Setup the local dev environment
Install `npm` and `noejs`
```
$ sudo dnf install nodejs npm
```

Get a source code of a react app. I will use `create-react-app` to make a simple boilerplate
```
$ sudo npm install -g create-react-app
$ cd ~/Documents
$ cd create-react-app react-app
```

Run the development server open it in the web browser. Server is running in localhost:3000
```
$ cd react-app
$ npm start
```
![](https://i.imgur.com/mTjlP7u.png)

Build the project. This will create a build directory under the project root. It contain all the static content needed to host the site
```
$ npm run build
```

Now host the build directory using a static web server like [[Apache]] or [[Nginx]]
>[!important]- Host the above generated build directory
>[[Host a static site using Apache web server]]

>[!faq]- Why npm start is not used in production?
> `npm start` script create a development server which includes features like hot reloading and error reporting. Therefor, assets are served directly from the memory, instead of writing to the disk. It makes it resourse intensive. On the other hand, it is not designed to handle high traffic loads.
> 
> `npm run build` script generates a build directory which is optimised for production. Since the assets are read from the disk, them can be hosted with any web server. Those severs are ligh weight and designed to handle high traffic loads.

## Setup the remote dev environment
Set up [[SSH]] acces into remote instance as explained below
- [[Access a remote VM using SSH]]

Install [[Apache]] or [[Nginx]] web server as explained below
- [[Host a static site using Apache web server]]

Copy the build directory to the remote instance
```
$ scp -r ~/Documents/react-app/build methmal@web.methmal.lk:/var/www/html/react-app
```

Finally reload the server. Your web app should be running in the remote IP address
```
$ sudo systemctl restart httpd
```

![](https://i.imgur.com/Je3Q4F4.png)

After that, you can obtain a SSL certificate as explained below
- [[Obtain a SSL certificate]]