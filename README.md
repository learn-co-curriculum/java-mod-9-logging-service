# Logging Service

## Learning Goals

- Create a logging service in Angular.

## Introduction

We know that we can log information to the console using the simple
`console.log()` built-in functionality of JavaScript. But what if I wanted each
log entry to tell me what time it was created, so I can go back and check the
timing of different statements coming out, for example.

## Create a Logging Service

Let's create a logging service that all components in our
application can use, therefore not only allowing us to have custom logging
functionality, but also making sure that functionality will be consistent across
our entire application.

Let's start by creating a `logging.service.ts` class:

```typescript
export class LoggingService {
  log(logMessage: string) {
    console.log(new Date().toLocaleString + "::" + logMessage);
  }
}
```

By convention, the class is stored in a file with the following name
`<service-name>.service.ts`

Now that the class has been created, it can be used like any other class in any
of the components we want to use it from. Let's start with using it in our "send
message" component to log the message to be sent when the user clicks on the
"send" button:

```typescript
import { Component, OnInit } from "@angular/core";
import { LoggingService } from "src/app/logging.service"; // import the service like any other class

@Component({
  selector: "app-send-message-component",
  templateUrl: "./send-message-component.component.html",
  styleUrls: ["./send-message-component.component.css"],
})
export class SendMessageComponentComponent implements OnInit {
  messageString: string;

  loggingSvce = new LoggingService(); // create a new instance of the service

  constructor() {}

  ngOnInit(): void {}

  // use the instance of the logging service in our event handler
  onSendMessage() {
    this.loggingSvce.log("Send following message: ");
    this.loggingSvce.log(this.messageString);
  }
}
```

We can link our `onSendMessage()` function to our button in our view with the
`click()` instruction we used before:

```html
<div class="container">
  <div class="row">
    <div class="col-10 p-3">
      <input
        type="input"
        class="form-control"
        placeholder="Type a message"
        [(ngModel)]="messageString"
      />
    </div>
    <div class="col-2 p-3">
      <div class="float-end">
        <button class="btn btn-primary" (click)="onSendMessage()">Send</button>
        <!-- Added (click) event here -->
      </div>
    </div>
  </div>
  <div class="row">
    <div class="col-3 p-3">Message Preview</div>
    <div class="col-9 p-3">
      <div class="col-10 p-3 border rounded-5">
        <span>{{messageString}}</span>
      </div>
    </div>
  </div>
</div>
```

Now your application will put out a message like the following every time you
hit the "Send" button :

```plaintext
8/13/2022, 4:47:22 PM::Send following message:
8/13/2022, 4:47:22 PM::Hello from my web application
```

The above will work, but it's not how Angular would like us to use services. The
preferred way to use services is through Dependency Injection, which we will
cover next.
