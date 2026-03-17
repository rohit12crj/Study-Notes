✅ what is .ebextension file used for & how will u use use it to send custom cloudwatch logs ?
- We use .ebextensions to configure the CloudWatch agent by defining additional log file paths and log groups, enabling automatic log streaming from EC2 instances to CloudWatch.

---

✅ middleware / reverse proxy used in beanstalk & explain execution flow path

Internet -> ALB → Nginx → App

| Platform      | Default |
| ------------- | ------- |
| Node.js       | Nginx   |
| Python        | Nginx   |
| Java (Tomcat) | Apache  |
| PHP           | Apache  |

