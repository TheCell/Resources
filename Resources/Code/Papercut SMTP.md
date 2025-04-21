---
tags:
  - software
  - utility
created: 21.04.2025
up: "[[Code]]"
---

[https://github.com/ChangemakerStudios/Papercut-SMTP](https://github.com/ChangemakerStudios/Papercut-SMTP)

![[PapercutLogo.png]]

_The Simple Desktop Email Helper_

## The problem

If you ever send emails from an application or website during development, you're familiar with the fear of an email being released into the wild. Are you positive none of the 'test' emails are addressed to colleagues or worse, customers? Of course, you can set up and maintain a test email server for development -- but that's a chore. Plus, the delay when waiting to view new test emails can radically slow your development cycle.

![[papercut-choice.png]]

## Papercut SMTP to the rescue!

Papercut SMTP is a 2-in-1 quick email viewer AND built-in SMTP server (designed to receive messages only). Papercut SMTP doesn't enforce any restrictions on how you prepare your email, but it allows you to view the whole email-chilada: body, HTML, headers, and attachment right down to the naughty raw encoded bits. Papercut can be configured to run on startup and sit quietly (minimized in the tray) only providing a notification when a new message has arrived.

## Requirements

Papercut SMTP UI Requires the "WebView2" Microsoft shared system component to be installed on your system. If you have any problems getting it running go to this site: [WebView2 Download](https://developer.microsoft.com/en-us/microsoft-edge/webview2) and install it.

## Features

#### Instant Feedback When New Email Arrives
![[PapercutV7-Notification-1.png]]

#### Rich and Detailed View of Received Email
![[PapercutV7-Main-1.png]]

#### View and Download the Mime Sections of your Email
![[68747470733a2f2f6368616e67656d616b657273747564696f732e75732f636f6e74656e742f696d616765732f323032302f30372f50617065726375742d4d696d652e706e67.png]]

#### Raw View
![[68747470733a2f2f6368616e67656d616b657273747564696f732e75732f636f6e74656e742f696d616765732f323032302f30372f50617065726375742d5261772e706e67.png]]

#### Logging View
![[68747470733a2f2f6368616e67656d616b657273747564696f732e75732f636f6e74656e742f696d616765732f323032302f30372f50617065726375742d4c6f672e706e67.png]]

## (Optional) Download Papercut SMTP Service

Papercut SMTP has an optional HTTP server to receive emails even when the client is not running. It can be run in an almost portable way by downloading [Papercut.Smtp.Service.*.zip](https://github.com/ChangemakerStudios/Papercut/releases), unzipping, and [following the service installation instructions](https://github.com/ChangemakerStudios/Papercut/tree/develop/src/Papercut.Service).

### Host in Docker

Optionally you can run Papercut SMTP Service in docker: [Papercut SMTP Service in Docker](https://hub.docker.com/r/changemakerstudiosus/papercut-smtp)

#### Pull Image:

```
> docker pull changemakerstudiosus/papercut-smtp:latest
```

#### Run Papercut STMP Server Locally in Docker (HTTP Port :8080 and STMP port 25)

```
docker run -d -p 8080:80 -p 25:25 changemakerstudiosus/papercut-smtp:latest
```


The Papercut-SMTP Server Site will be accessible at [http://localhost:8080](http://localhost:8080).

## License

Papercut SMTP is Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).