---
title: Cron
category: CLI
tags:
updated:
---

### Format

```
Min  Hour Day  Mon  Weekday
```

```
*    *    *    *    *  command to be executed
```

```
┬    ┬    ┬    ┬    ┬
│    │    │    │    └─  Weekday  (0=Sun .. 6=Sat)
│    │    │    └──────  Month    (1..12)
│    │    └───────────  Day      (1..31)
│    └────────────────  Hour     (0..23)
└─────────────────────  Minute   (0..59)
```

### Operators

| Operator | Description                |
| ---      | ---                        |
| `*`      | all values                 |
| `,`      | separate individual values |
| `-`      | a range of values          |
| `/`      | divide a value into steps  |

### Examples

| Example        | Description                 |
| ---            | ---                         |
| `0 * * * *`    | every hour                  |
| `*/15 * * * *` | every 15 mins               |
| `0 */2 * * *`  | every 2 hours               |
| `0 18 * * 0-6` | every week Mon-Sat at 6pm   |
| `10 2 * * 6,7` | every Sat and Sun on 2:10am |
| `0 0 * * 0`    | every Sunday midnight       |
| ---            | ---                         |
| `@reboot`      | every reboot                |

### Crontab

```bash
# Adding tasks easily
echo "@reboot echo hi" | crontab
```

```bash
# Open in editor
crontab -e
```

```bash
# List tasks
crontab -l [-u user]
```

### Here are some self-explanatory crontab syntax examples:

```bash
30 4 echo "It is now 4:30 am."
0 22 echo "It is now 10 pm."
30 15 25 12 echo "It is 3:30pm on Christmas Day."
30 3 * * * echo "Remind me that it's 3:30am every day."
0 * * * * echo "It is the start of a new hour."
0 6 1,15 * * echo "At 6am on the 1st and 15th of every month."
0 6 * * 2,3,5 echo "At 6am on Tuesday, Wednesday and Thursdays."
59 23 * * 1-5 echo "Just before midnight on weekdays."
0 */2 * * * echo "Every two hours."
0 20 * * 4 echo "8pm on a Thursday."
0 20 * * Thu echo "8pm on a Thursday."
*/15 9-17 * * 2-5 echo "Every 15 minutes from 9am-5pm on weekdays."
@yearly echo "Happy New Year!"
```
---

THE END
