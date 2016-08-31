# jsWeb - Simple Web Server with Javascript

jsWeb is a program to simplify web api based on express development.
After installed jsweb, just write the express request handle js file and put them to the correctly directlry. Start with command:

```
  > jsweb [/path/to/site/home]
```

# Features

* Everythings of the site in single folder.
* Multiple port support, each port is a standalone process
* Auto reload, Add/Modify/Delete api file doesn't need restart server
* Init script can be load before real api method load to prepare system like database object or connection
* https support
* virtual host support
* Mustache template support buildin
* Cluster support


# Run the demos

```
    $ sudo npm install jsweb -g
    $ git clone https://github.com/jsplne/jsweb-demo
    $ jsweb jsweb-demo/static
    $ jsweb jsweb-demo/api-server
    $ jsweb jsweb-demo/vhost
```

# Namings

```
    name[(tag[;tag[...]])]
```

## tags for port folder

* `https`  This is a https port

## tags for root folder

* `api`   Root folder `.js` file treat as server side script, if `index.js` exists, website homepage will be the result of `index.js`

## tags for 1st level inside root folder 

* `api`  Folder contain api script. all `.js` inside this folder will be called as api call, include subfolders.

## tags for api `.js` file

* GET    api will called for correspond http method, if omit, all method will called.
* POST
* PUT
* ... all valid HTTP method


# Runtime data structure

System information share with request callback via  : `req.jw`

```
    req.jw = {
        app : <express object>  // system object
    }
```


# Enable/Disable file

Set/clear file execute flag of files to enable/disable init file or web api file to be load.

# Directory structure (single site)

```
      <HOME> +
             |
             +--"8000"                         // port to listen
             |     +------ config
             |     |        +-- jsweb-config.json
             |     |        +-- [jsweb.key]    // for https
             |     |        +-- [jsweb.cert]
             |     |        +-- [jsweb.ca]
             |     |
             |     +------ init
             |     |        +-- 01-db-pg.js     // for Postgresql
             |     |        +-- 02-db-mssql.js  // for Sql server
             |     |        +-- 02-db-mysql.js  // for Mysql
             |     |
             |     +------ views
             |     |        +-- login.hjs       // hjs template
             |     |        +-- xxx.hjs
             |     |
             |     +------ root
             |              +-- api::test           // in windows, use 'api#test', linux work's for both
             |              |      +-- hello.js     // http://localhost:8000/test/hello
             |              |      +-- echo.js      // http://localhost:8000/test/echo
             |              |      +-- db-query.js  // http://localhost:8000/test/db-query
             |              |      +-- other
             |              |          +-- hello.js     // http://localhost:8000/test/other/hello
             |              |          +-- echo.js      // http://localhost:8000/test/other/echo
             |              +-- index.html
             |              +-- others
             |                    +-- others
             +--<port 2>
             | 
             ...

```

# Directory structure (virtual host)

```
      <HOME> +
             |
             +--"8000"                         // port to listen
             |     +------ [config]
             |     |        +-- [jsweb.key]    // for https
             |     |        +-- [jsweb.cert]
             |     |        +-- [jsweb.ca]
             |     |
             |     +------ www.host1.com
             |     |           |
             |     |           +...
             |     |
             |     +------ www.host2.com
             |     |           |
             |     |           +...
             |     |
             |     +------ www.host3.com
             |                 |
             |                 +------ init
             |                 |        +-- 01-db-pg.js     // for Postgresql
             |                 |        +-- 02-db-mssql.js  // for Sql server
             |                 |        +-- 02-db-mysql.js  // for Mysql
             |                 |
             |                 +------ views
             |                 |        +-- login.hjs       // hjs template
             |                 |        +-- xxx.hjs
             |                 |
             |                 +------ root
             |                          +-- api::test           // in windows, use 'api#test', linux work's for both
             |                          |      +-- hello.js     // http://localhost:8000/test/hello
             |                          |      +-- echo.js      // http://localhost:8000/test/echo
             |                          |      +-- db-query.js  // http://localhost:8000/test/db-query
             |                          |      +-- other
             |                          |          +-- hello.js     // http://localhost:8000/test/other/hello
             |                          |          +-- echo.js      // http://localhost:8000/test/other/echo
             |                          +-- index.html
             |                          +-- others
             |                                +-- others
             +--<port 2>
             | 
             ...

```

# https certificate file generate

```
    cd /path/to/https-port/config
    $ openssl genrsa -out jsweb.key 1024
    $ openssl req -new -key jsweb.key -out jsweb.ca
    $ openssl x509 -req -in jsweb.ca -signkey jsweb.key -out jsweb.cert
```

# Cluster support

Set port-config.json under the port folder with key/value: `"cluster" : true`

## port-config.json
```
{
    "cluster" : true
}
```
