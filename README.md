# Debug
X-DEBUG

Edit the .env file in the Laradock folder:
--------------------------------------------
```
nano .env
Search for WORKSPACE_INSTALL_XDEBUG and replace the value with true
Search for PHP_FPM_INSTALL_XDEBUG and replace the value with true
```
Edit the xDebug configuration files:
-------------------------------------
```
nano workspace / xdebug.ini
nano php-fpm / xdebug.ini
```
Replace the contents of both files with:
```
xdebug.remote_host = host.docker.internal 
xdebug.remote_connect_back = 1 
xdebug.remote_port = 9000 
xdebug.idekey = VSCODE
xdebug.remote_autostart = 1 
xdebug.remote_enable = 1 
xdebug.cli_color = 1 
xdebug.profiler_enable = 1 
xdebug.profiler_output_dir = "~ / xdebug / vscode / tmp / profiling"
xdebug.remote_handler = dbgp 
xdebug.remote_mode = req
xdebug.var_display_max_children = -1 
xdebug.var_display_max_data = -1 
xdebug.var_display_max_depth = -1
```
Rebuild and restart the workspace and php-fpm containers:
----------------------------------------------------------
```
docker-compose build workspace php-fpm
docker-compose up -d workspace php-fpm
docker-compose restart workspace php-fpm
```
Enable xDebug in Visual Studio Code:
---------------------------------------
In the Visual Studio Code menu, click Run and then Add Configuration
Add the following configuration:
```
{ 
    "name": "Listen for XDebug", 
    "type": "php", 
    "request": "launch", 
    "port": 9000, 
    "log": true, 
    "pathMappings": { 
        "/var/www" : "${workspaceRoot}", 
    }, 
    "ignore": [ 
        "**/vendor/**/*.php" 
    ] 
}
```
Start / Stop xDebug:
-----------------------
This is active to run on startup by default.
To control the behavior of xDebug (in the php-fpm container), you can run the following commands from the Laradock root folder, (at the same prompt you run docker-compose):
Stop xDebug from starting by default: 
```./php-fpm/xdebug stop.
```
Start xDebug by default: 
```./php-fpm/xdebug start.
```
See the status of xDebug: 
```./php-fpm/xdebug status.```

That's it, now you can start using xDebug in Visual Studio Code

##### Reference:
https://luiscoutinh.medium.com/configurar-visual-studio-code-laradock-xdebug-f3abc9339080

