# LARAVEL QUEUE SUPERVISOR
Como configurar supervidor em um servidor linux

Laravel Queue Supervisor

To run Laravel's queue listener in production, you can use something like:

php artisan --env=production --timeout=240 queue:listen

However this process won't be restarted when the process ends (eg. after a restart). We can use Supervisor to keep this queue listener process active at all times. Below are instructions for Ubuntu 13.04.

Install Supervisor with sudo apt-get install supervisor. Ensure it's started with sudo service supervisor restart.

In /etc/supervisord/conf.d/ create laravel_queue.conf:

[program:laravel_queue]
command=/usr/local/bin/run_queue.sh
autostart=true
autorestart=true
stderr_logfile=/var/log/laraqueue.err.log
stdout_logfile=/var/log/laraqueue.out.log
Give it execute permissions: chmod +x laravel_queue.conf.

This file tells Supervisor to run /usr/local/bin/run_queue.sh and ensure the process is automatically started & restarted as required. Create that file now:

#!/bin/bash
php /path/to/AppName/artisan --env=production --timeout=240 queue:listen
Change /path/to/AppName/ to the path to your Laravel application on disk. Give this execute permissions, too: chmod +x run_queue.sh.

Now update Supervisor with: sudo supervisorctl reread. And start using those changes with: sudo supervisorctl update.

Supervisor will automatically run on boot and spin up your processes. You can see the "laravel_queue" process running with: sudo supervisorctl.

laravel_queue  RUNNING   pid 1161, uptime 11 days, 14:27:00  
supervisor>  
