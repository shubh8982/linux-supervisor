# linux-supervisor-setup-for-laravel-queue-jobs
this repo helps in installing and running supervisor on Linux server for Laravel application

Supervisor is a process manager that runs in the background and continues to manage your Laravel queue worker process even when your terminal is closed. 
Once you configure Supervisor to manage the php artisan queue:work command, it becomes a daemon process that runs independently of your terminal session.

**Here's how it works:**

When you start the Supervisor-managed queue worker using the **sudo supervisorctl start laravel-worker:***
command, Supervisor spawns the worker processes in the background.

Supervisor monitors these processes, automatically restarting them if they exit unexpectedly or crash.

Even if you close your terminal or log out, Supervisor continues to run as a system service, ensuring that your queue worker remains active.

**Install Supervisor** (if not already installed):

//bash
sudo apt-get install supervisor  # For Ubuntu/Debian

**Create a Supervisor Configuration File:**
Create a Supervisor configuration file for your Laravel queue worker. For example, create a file named laravel-worker.conf:

sudo nano /etc/supervisor/conf.d/laravel-worker.conf

Add the following content, adjusting the paths and configuration as needed:
//
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /path-to-your-laravel-project/artisan queue:work --sleep=3 --tries=3
autostart=true
autorestart=true
user=www-data  ; Adjust the user based on your server configuration
numprocs=8  ; Adjust based on the number of workers you want to run
redirect_stderr=true
stdout_logfile=/path-to-your-laravel-project/storage/logs/worker.log
//

**Update Supervisor and Start the Worker:**
Reload the Supervisor configuration and start the Laravel queue worker:

//bash
sudo supervisorctl reread
sudo supervisorctl update  
sudo supervisorctl start laravel-worker:*

Enjoy the seamless service of supervisor for your background jobs in laravel 
