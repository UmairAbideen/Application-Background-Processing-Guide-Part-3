# ⚡ App Background Processing Guide (Part 3)

This project demonstrates how to use **Laravel Horizon** and **Supervisor** to monitor and manage background jobs in Laravel 10. These tools help ensure queue workers remain active, process jobs efficiently, and provide real-time monitoring of queued tasks.

The project covers:

- Laravel Horizon
- Supervisor

Without Horizon & Supervisor:

- Queue workers must be started manually.
- No visual monitoring of jobs.
- Jobs stop processing if the terminal closes or the server restarts.

With Horizon & Supervisor:

- Real-time queue monitoring.
- Automatic queue worker management.
- Queue workers restart automatically after crashes or server reboots.
- Improved reliability for production environments.

---

# ❓ Why Use Horizon & Supervisor?

Background processing is useful when:

- You need to process jobs asynchronously.
- You want to monitor queue performance.
- Your application handles large numbers of background jobs.
- You need automatic recovery if queue workers stop.
- You want production-ready queue management.

Examples:

- Sending Emails
- Processing Payments
- Image Processing
- Notifications
- Report Generation
- Background Imports & Exports

Instead of processing these tasks during a user request, Laravel executes them in the background.

---

# 🧩 Concepts Covered

- ✅ Laravel Horizon
- ✅ Supervisor

---

# 1️⃣ Laravel Horizon

## What is Horizon?

Laravel Horizon is a **dashboard and monitoring tool** for Laravel queues that use Redis.

Think:

> **"Monitor your queue workers in real time."**

Horizon provides:

- Active Jobs
- Pending Jobs
- Completed Jobs
- Failed Jobs
- Queue Throughput
- Processing Time
- Worker Status

---

## Install Horizon

Install Horizon using Composer.

```bash
composer require laravel/horizon
```

Publish Horizon assets.

```bash
php artisan horizon:install
```

Run migrations.

```bash
php artisan migrate
```

---

## Start Horizon

```bash
php artisan horizon
```

Visit:

```
/horizon
```

to monitor your queues.

---

## Configure Horizon

Open:

```
config/horizon.php
```

Configure the queue workers.

```php
'environments' => [

    'production' => [

        'supervisor-1' => [

            'connection' => 'redis',

            'queue' => ['default'],

            'balance' => 'auto',

            'processes' => 5,

            'tries' => 3,

        ],

    ],

];
```

---

## When to Use Horizon?

Use Horizon when:

- ✅ Monitoring queue jobs
- ✅ Redis-based queues
- ✅ Tracking failed jobs
- ✅ Managing queue workers
- ✅ Production applications

---

# 2️⃣ Supervisor

## What is Supervisor?

Supervisor is a **Linux process manager** that keeps Laravel queue workers running continuously.

Think:

> **"Automatically restart queue workers whenever they stop."**

Without Supervisor:

```text
Start Queue Worker

↓

Terminal Closed

↓

Queue Stops
```

With Supervisor:

```text
Queue Worker Stops

↓

Supervisor Detects It

↓

Worker Restarts Automatically
```

---

## Install Supervisor (Ubuntu)

```bash
sudo apt update

sudo apt install supervisor
```

---

## Create a Supervisor Configuration

Example configuration:

```ini
[program:laravel-worker]

process_name=%(program_name)s_%(process_num)02d

command=php /path/to/project/artisan queue:work

autostart=true

autorestart=true

stopasgroup=true

killasgroup=true

user=www-data

numprocs=1

redirect_stderr=true

stdout_logfile=/path/to/project/storage/logs/worker.log
```

---

## Reload Supervisor

```bash
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl start laravel-worker:*
```

---

## Check Worker Status

```bash
sudo supervisorctl status
```

---

## When to Use Supervisor?

Use Supervisor when:

- ✅ Production servers
- ✅ Queue workers
- ✅ Automatic process restart
- ✅ Long-running background jobs
- ✅ Reliable queue processing

---

# ⚖️ Comparison

| Feature | Purpose | Best Used For |
|----------|---------|---------------|
| Horizon | Queue Monitoring Dashboard | Monitoring Jobs & Workers |
| Supervisor | Process Manager | Keeping Queue Workers Running |

---

# 🔄 Background Processing Flow

```text
User Action
      │
      ▼
Job Dispatched
      │
      ▼
Redis Queue
      │
      ▼
Supervisor Keeps Worker Running
      │
      ▼
Horizon Monitors Worker
      │
      ▼
Job Processed Successfully
```
