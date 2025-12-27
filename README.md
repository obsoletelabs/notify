
How to use email sender:
thats the headers

```py
smtp_enabled = environ.get("NOTIFIER_SMTP_ENABLED", "false").lower() == "true" # default to disabled
smtp_username = environ.get("NOTIFIER_SMTP_USERNAME", "") # apparently I shouldnt fill these with my defaults bc security?
smtp_password = environ.get("NOTIFIER_SMTP_PASSWORD", "")
smtp_server = environ.get("NOTIFIER_SMTP_SERVER", "mail.obsoletelabs.net") # default to obsoletelabs mail server because why not

smtp_security = environ.get("NOTIFIER_SMTP_SECURITY", "starttls").lower()  # starttls, tls, none
smtp_port = int( environ.get("NOTIFIER_SMTP_PORT", "0") ) # 0 means auto-detect, I cant convert "None" to int
# Determine default port based on security setting
if smtp_port == 0:
    if smtp_security == "tls":
        smtp_port = 465
    elif smtp_security == "starttls":
        smtp_port = 587
    else:
        smtp_port = 25


email_from = environ.get("NOTIFIER_EMAIL_FROM_ADDRESS", smtp_username)
reply_to = environ.get("NOTIFIER_SMTP_REPLYTO_ADDRESS", email_from)
email_to = environ.get("NOTIFIER_EMAIL_TO_ADDRESSES", "")
unsubscribe_header = environ.get("NOTIFIER_SMTP_UNSUBSCRIBE_HEADER", f"<mailto:{email_from}>")
smtp_precedence = environ.get("NOTIFIER_SMTP_PRECEDENCE", "bulk")

# Retry configuration
smtp_retries = int(environ.get("NOTIFIER_SMTP_RETRIES", "3"))
smtp_retry_delay = float(environ.get("NOTIFIER_SMTP_RETRY_DELAY", "1.5"))
```

and you call
```send_email_notification(email_context, sendtoaddresses) where the sendto is optional
email context is a dictionary
```py
subject = context.get("Subject", "No Subject")
greeting = context.get("Greeting", "Hello,")
body = context.get("Body", "No Content")
conclusion = context.get("Conclusion", "Regards,<br>Obsoletelabs Notification System")
footer = context.get("Footer", "This is an automated message sent by the Obsoletelabs Notification System.<br>We do not control who receives these emails, please contact the sender if you have any questions.")
```
