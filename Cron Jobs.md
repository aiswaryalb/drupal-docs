# ⏳ Understanding Cron Jobs in Drupal

In Drupal, **cron** runs only when the website is visited by a user. To ensure that cron executes regularly, we need to add cron jobs to our server's crontab.

## 📅 Adding Cron Jobs

To initiate cron in Drupal, follow these steps:

1. **Find the Cron URL**: Go to **/admin/config/system/cron** to find the cron link you need to use.

2. **Get a Cron Expression**: Use an online cron calculator to determine the appropriate cron expression.

3. **Edit the Crontab**: Open your crontab configuration:
   ```bash
   crontab -e

4. **Add the Cron Job**: Insert a line similar to the following, adjusting the URL to match your chron URL:
  ```bash
  * * * * * curl -s https://d10.ddev.site/cron/pP6k_5ctXLRp9drcGXTYNwo7Cb6sUokmPaOheVrFBl6eUmrbqC3WjOgGUv-vVNP4k-pUXQFNLA > /dev/null 2>&1
  ```

# 📖 Understanding the Cron Command
  
  The command * * * * * is a cron expression used to define when the job should run. Here’s what each asterisk represents:

  | Field         | Value        | Description                             |
  |---------------|--------------|-----------------------------------------|
  | Minute        | 0-59        | At every minute                        |
  | Hour          | 0-23        | At every hour                          |
  | Day of Month  | 1-31        | At every day of the month              |
  | Month         | 1-12        | At every month                        |
  | Day of Week   | 0-6 (Sun-Sat)| On every day of the week              |

  In this case, * * * * * indicates that the cron job should run every minute.

# 📝 Conclusion
By setting up cron jobs effectively, you ensure that background tasks in Drupal execute automatically, improving the performance and functionality of your website.
