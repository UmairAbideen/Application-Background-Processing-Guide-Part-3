# ⚡ App Background Processing Guide (Part 3)

This project demonstrates how to manage and monitor **background jobs** in **Laravel 10** using **Queues**, **Laravel Horizon**, and **Supervisor**. These tools ensure queued jobs are processed efficiently, monitored in real time, and continue running reliably in production environments.

The project covers:

- Laravel Queue
- Queue with Cron Jobs
- Laravel Horizon
- Supervisor

Without queue monitoring and process management, background jobs may stop if the worker crashes or the server restarts.

With Horizon and Supervisor, queued jobs are continuously processed, monitored, and automatically restarted when needed.

---

# ❓ Why Use Horizon & Supervisor?

Background processing is useful when:

- You want to process jobs asynchronously.
- You need to monitor queue performance.
- Queue workers should automatically restart if they stop.
- You want to keep queues running after server reboots.
- You need production-ready queue management.

Examples:

- Sending Emails
- Notifications
- Invoice Generation
- Image Processing
- Report Generation
- Scheduled Jobs

---

# 🧩 Background Processing Concepts Covered

- ✅ Queue
- ✅ Queue with Cron Jobs
- ✅ Laravel Horizon
- ✅ Supervisor

---

# 1️⃣ Laravel Queue

## What is a Queue?

Laravel Queues allow time-consuming tasks to run in the background instead of making users wait for the request to finish.

Think:

> **"Process later, respond faster."**

### Example

Dispatch a job:

```php
SendEmailJob::dispatch($user);
```

Run the queue worker:

```bash
php artisan queue:work
```

### When to Use Queues?

Use queues for:

- ✅ Sending Emails
- ✅ Notifications
- ✅ PDF Generation
- ✅ Image Processing
- ✅ Background Tasks

---

# 2️⃣ Queue with Cron Jobs

## What is Queue with Cron?

Cron Jobs schedule recurring tasks, while Queues process those tasks asynchronously.

Think:

> **"Cron schedules the job, Queue executes it."**

### Example

Schedule a job inside:

```
app/Console/Kernel.php
```

```php
protected function schedule(Schedule $schedule)
{
    $schedule->job(new SendReminderEmailJob())
             ->daily();
}
```

Run Laravel Scheduler:

```bash
php artisan schedule:work
```

Run Queue Worker:

```bash
php artisan queue:work
```

### When to Use?

- ✅ Daily Reports
- ✅ Reminder Emails
- ✅ Scheduled Notifications
- ✅ Automatic Backups

---

# 3️⃣ Laravel Horizon

## What is Horizon?

Laravel Horizon is a **dashboard** for monitoring Redis queues.

Instead of monitoring queues from the terminal, Horizon provides a web interface showing:

- Running Jobs
- Completed Jobs
- Failed Jobs
- Queue Throughput
- Processing Time
- Active Workers

Think:

> **"A real-time dashboard for Laravel queues."**

### Install Horizon

```bash
composer require laravel/horizon
```

Install Horizon assets:

```bash
php artisan horizon:install
```

Run migrations:

```bash
php artisan migrate
```

Start Horizon:

```bash
php artisan horizon
```

Visit:

```
/horizon
```

### When to Use Horizon?

Use Horizon for:

- ✅ Queue Monitoring
- ✅ Failed Job Tracking
- ✅ Worker Statistics
- ✅ Redis Queue Management
- ✅ Production Monitoring

---

# 4️⃣ Supervisor

## What is Supervisor?

Supervisor is a Linux process manager that keeps Laravel queue workers running continuously.

Without Supervisor:

```
Terminal Closed

↓

Queue Stops
```

With Supervisor:

```
Queue Stops

↓

Supervisor Restarts Worker Automatically
```

Think:

> **"Keeps queue workers alive."**

### Example Configuration

Create a Supervisor configuration file:

```ini
[program:laravel-worker]

process_name=%(program_name)s_%(process_num)02d

command=php /path-to-project/artisan queue:work

autostart=true

autorestart=true

numprocs=1

redirect_stderr=true

stdout_logfile=/path-to-project/storage/logs/worker.log
```

Reload Supervisor:

```bash
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl start laravel-worker:*
```

### When to Use Supervisor?

Use Supervisor for:

- ✅ Production Servers
- ✅ Queue Worker Management
- ✅ Auto Restart
- ✅ Long Running Jobs
- ✅ High Availability

---

# 📋 Background Processing Flow

```text
User Action
      │
      ▼
Job Dispatched
      │
      ▼
Queue
      │
      ▼
Queue Worker
      │
      ▼
Horizon Monitors Jobs
      │
      ▼
Supervisor Keeps Workers Running
```

---

# 📦 Useful Laravel Commands

### Start Queue Worker

```bash
php artisan queue:work
```

### Start Scheduler

```bash
php artisan schedule:work
```

### Install Horizon

```bash
composer require laravel/horizon
```

### Install Horizon Assets

```bash
php artisan horizon:install
```

### Start Horizon

```bash
php artisan horizon
```

### Restart Queue Workers

```bash
php artisan queue:restart
```

### View Failed Jobs

```bash
php artisan queue:failed
```

### Retry Failed Jobs

```bash
php artisan queue:retry all
```

### Clear Failed Jobs

```bash
php artisan queue:flush
```

### Clear Application Cache

```bash
php artisan optimize:clear
```

### Start Development Server

```bash
php artisan serve
```

---

# ⚖️ Comparison

| Feature | Purpose | Best Used For |
|----------|----------|---------------|
| Queue | Process jobs asynchronously | Emails, Notifications |
| Queue with Cron | Schedule recurring jobs | Reports, Reminders |
| Horizon | Monitor Redis queues | Queue Dashboard |
| Supervisor | Keep queue workers running | Production Servers |

---

# 🚀 Real-World Example

### E-Commerce Application

Background Tasks

- Order Confirmation Email
- Invoice Generation
- Inventory Update
- SMS Notification
- Daily Sales Report

Application Flow

```text
User Places Order
        │
        ▼
Job Added to Queue
        │
        ▼
Queue Worker Processes Job
        │
        ▼
Horizon Monitors Progress
        │
        ▼
Supervisor Keeps Worker Running
```

---

# 📌 Features

- Queue Processing
- Queue with Cron Jobs
- Laravel Horizon Integration
- Queue Monitoring Dashboard
- Supervisor Configuration
- Automatic Worker Restart
- Production Queue Management
- Failed Job Monitoring
- Real-Time Queue Statistics

---

## 📷 Screenshots

Add screenshots of the following:

- Horizon Dashboard
- Queue Worker
- Supervisor Status
- Scheduled Job Execution
- Queue Processing

---

## 📄 License

This project is open-source and available under the **MIT License**.
