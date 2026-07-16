
Lightning Message Service (LMS) enables communication between components that are not directly related.

LMS can communicate between:

- LWC ↔ LWC
- Aura ↔ Aura
- Aura ↔ LWC
- Visualforce ↔ LWC

---

# Why Use LMS?

Without LMS:

```text
Parent
  ↓
Child
```

Communication is limited.

With LMS:

```text
Component A
      ↓
Message Channel
      ↓
Component B
```

Components do not need a parent-child relationship.

---

# LMS Architecture

```text
Publisher
     ↓
 publish()
     ↓
Message Channel
     ↓
 subscribe()
     ↓
Subscriber
```

---

# Folder Structure

```text
force-app
└── main
    └── default
        ├── lwc
        │   ├── accountPublisher
        │   │   ├── accountPublisher.html
        │   │   ├── accountPublisher.js
        │   │   └── accountPublisher.js-meta.xml
        │   │
        │   └── accountSubscriber
        │       ├── accountSubscriber.html
        │       ├── accountSubscriber.js
        │       └── accountSubscriber.js-meta.xml
        │
        └── messageChannels
            └── AccountChannel.messageChannel-meta.xml
```

---

# Message Channel

Message Channels act as the communication bridge.

Location:

```text
force-app/main/default/messageChannels
```

---

## AccountChannel.messageChannel-meta.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<LightningMessageChannel
    xmlns="http://soap.sforce.com/2006/04/metadata">

    <masterLabel>
        Account Channel
    </masterLabel>

    <isExposed>true</isExposed>

</LightningMessageChannel>
```

---

# Publisher Component

Publishes data to the channel.

---

## accountPublisher.html

```html
<template>

    <lightning-button
        label="Send Account"
        onclick={publishMessage}>
    </lightning-button>

</template>
```

---

## accountPublisher.js

```javascript
import { LightningElement, wire }
from 'lwc';

import {
    publish,
    MessageContext
}
from 'lightning/messageService';

import ACCOUNT_CHANNEL
from '@salesforce/messageChannel/AccountChannel__c';

export default class AccountPublisher
extends LightningElement {

    @wire(MessageContext)
    messageContext;

    publishMessage() {

        const payload = {
            accountId: '001XXXXXXXXXXXX',
            accountName: 'Salesforce'
        };

        publish(
            this.messageContext,
            ACCOUNT_CHANNEL,
            payload
        );
    }
}
```

---

# Subscriber Component

Receives messages from the channel.

---

## accountSubscriber.html

```html
<template>
    <lightning-card
        title="Subscriber">
        <div class="slds-p-around_medium">
            <p>
                Account Id:
                {accountId}
            </p>
            <p>
                Account Name:
                {accountName}
            </p>
        </div>
    </lightning-card>
</template>
```

---

## accountSubscriber.js

```javascript
import {
    LightningElement,
    wire
}
from 'lwc';

import {
    subscribe,
    unsubscribe,
    MessageContext
}
from 'lightning/messageService';

import ACCOUNT_CHANNEL
from '@salesforce/messageChannel/AccountChannel__c';

export default class AccountSubscriber
extends LightningElement {

    subscription = null;

    accountId;
    accountName;

    @wire(MessageContext)
    messageContext;

    connectedCallback() {

        this.subscribeToChannel();
    }

    subscribeToChannel() {

        if(this.subscription) {
            return;
        }

        this.subscription =
            subscribe(
                this.messageContext,
                ACCOUNT_CHANNEL,
                (message) => {
                    this.accountId =
                        message.accountId;
                    this.accountName =
                        message.accountName;
                }
            );
    }

    disconnectedCallback() {

        unsubscribe(
            this.subscription
        );

        this.subscription =
            null;
    }
}
```

---

# LMS Methods

## Publish

```javascript
publish(
    messageContext,
    ACCOUNT_CHANNEL,
    payload
);
```

Sends a message to all subscribers.

---

## Subscribe

```javascript
subscribe(
    messageContext,
    ACCOUNT_CHANNEL,
    callback
);
```

Receives messages from a channel.

---

## Unsubscribe

```javascript
unsubscribe(
    subscription
);
```

Stops listening to messages.

---

# MessageContext

Required for LMS operations.

```javascript
@wire(MessageContext)
messageContext;
```

Used by:

- publish()
- subscribe()

---

# Payload Example

```javascript
const payload = {

    accountId:
        '001XXXXXXXXXXXX',

    accountName:
        'Salesforce'
};
```

---

# Subscription Lifecycle Flow

```text
connectedCallback()
        ↓
subscribe()
        ↓
Message Received
        ↓
Update UI
        ↓
disconnectedCallback()
        ↓
unsubscribe()
```

---

# LMS vs Custom Events

| Custom Events | LMS |
|--------------|-----|
| Parent ↔ Child | Unrelated Components |
| DOM Hierarchy Required | No Hierarchy Required |
| Limited Scope | Application Scope |
| Component Communication | Cross Technology Communication |

---

# LMS Use Cases

## Communication Between Unrelated Components

```text
Search Component
       ↓
Message Channel
       ↓
Result Component
```

---

## Utility Bar ↔ Record Page

```text
Utility Component
       ↓
Message Channel
       ↓
Record Component
```

---

## Aura ↔ LWC Communication

```text
Aura Component
       ↓
Message Channel
       ↓
LWC Component
```

---

## Visualforce ↔ LWC Communication

```text
Visualforce
      ↓
Message Channel
      ↓
LWC
```

---
# Best Practices

- Always unsubscribe in `disconnectedCallback()`
- Use LMS only for unrelated components
- Keep payloads small
- Use meaningful channel names
- Avoid creating unnecessary channels

---

# Summary

## LMS Components

```text
Message Channel
Publisher
Subscriber
MessageContext
```

---

## LMS Methods

```javascript
publish()
subscribe()
unsubscribe()
```

---

## File Location

```text
force-app/main/default/messageChannels
```

---

## Message Channel File

```text
AccountChannel.messageChannel-meta.xml
```

---

## Import Syntax

```javascript
import ACCOUNT_CHANNEL
from '@salesforce/messageChannel/AccountChannel__c';
```

Lightning Message Service is the preferred Salesforce solution for communication between unrelated components and is one of the most frequently asked LWC interview topics.