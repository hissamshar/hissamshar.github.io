---
title: "Creating a WhatsApp AI Assistant Using n8n: A Step-by-Step Guide"
description: ""
summary: ""
categories: ["AI", "Automation"]
tags: ["n8n", "API"]
#externalUrl: ""
date: 2024-08-09
draft: false
---

Build your own AI-powered WhatsApp chatbot using **n8n**, **WhatsApp Business Cloud API**, and **OpenAI**. This guide walks you through every step—from setup to testing—with real-world error handling and solutions.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Prerequisites](#prerequisites)  
3. [Registering Your WhatsApp Business App](#registering-your-whatsapp-business-app)  
4. [Configuring Your Webhook in n8n](#configuring-your-webhook-in-n8n)  
5. [Processing Incoming Messages](#processing-incoming-messages)  
6. [Integrating OpenAI for Responses](#integrating-openai-for-responses)  
7. [Sending Replies via WhatsApp](#sending-replies-via-whatsapp)  
8. [Common Errors & Fixes](#common-errors--fixes)  
9. [Going Live](#going-live)  
10. [Conclusion & Next Steps](#conclusion--next-steps)  

---

## 1. Introduction

Want to chat with an AI on WhatsApp? In this tutorial, you'll learn how to build a **WhatsApp AI Assistant** using:

- **n8n** (automation tool)  
- **WhatsApp Business Cloud API**  
- **OpenAI** (for generating intelligent replies)

By the end, you'll have a working chatbot and gain hands-on experience with APIs, webhooks, and automation.

---

## 2. Prerequisites

- n8n account (Cloud or self-hosted)  
- Meta Developer account with WhatsApp Business Cloud API access  
- OpenAI API key  
- Basic understanding of APIs and webhook workflows

---

## 3. Registering Your WhatsApp Business App

### A. Create WhatsApp App in Meta Developer

1. Visit [Meta for Developers](https://developers.facebook.com/apps/)  
2. Create a new app: choose **Business** → **WhatsApp**  
3. Link or create a WhatsApp Business Account

### B. Obtain Testing Credentials

Your app dashboard will show:
- **Test phone number**  
- **Phone Number ID**  
- **Temporary access token** (valid for only 24 hours)

> Tip: For long-term use, generate a 60-day system-user token later.

### C. Add Recipients to Test List

By default, only approved numbers can receive messages:

1. Navigate to **WhatsApp → API Setup**  
2. Add numbers in E.164 format (e.g., `+923001234567`)  
3. Users must accept the invite via WhatsApp to become valid recipients

---

## 4. Configuring Your Webhook in n8n

### A. Create a Webhook Node

- Method: **GET** (for initial verification)  
- Endpoint example:  
  ```
  https://yourname.app.n8n.cloud/webhook/your-unique-id/webhook
  ```

### B. Verify the Webhook with Meta

In your app’s **Webhook** section:

- **Callback URL**: your n8n webhook URL  
- **Verify Token**: any secret string you choose (e.g., `mySecret2025`)

### C. Echo Back Meta’s Challenge

Configure n8n's Webhook node response:

| Field         | Value                                           |
|---------------|-------------------------------------------------|
| Response Mode | On Received                                     |
| Response Body | `{{$json["query"]["hub.challenge"]}}`           |

This ensures Meta can verify your endpoint successfully.

---

## 5. Processing Incoming Messages

WhatsApp sends JSON data with structure like:

```json
{
  "entry": [
    {
      "changes": [
        {
          "value": {
            "messages": [
              {
                "from": "923001234567",
                "text": { "body": "Hello bot!" }
              }
            ],
            "metadata": {
              "phone_number_id": "698352170035199"
            }
          }
        }
      ]
    }
  ]
}
```

Extract:
- `from`: user’s number  
- `text.body`: user’s text  
- `metadata.phone_number_id`: correct sender ID

---

## 6. Integrating OpenAI for Responses

Use an **HTTP Request** node to call OpenAI:

```http
POST https://api.openai.com/v1/chat/completions
Authorization: Bearer YOUR_OPENAI_API_KEY
Content-Type: application/json

{
  "model": "gpt-4o-mini",
  "messages": [
    { "role": "system", "content": "You are a helpful WhatsApp AI assistant." },
    { "role": "user", "content": "{{ $json[...] }}" }
  ]
}
```

Replace `{{ $json[...] }}` with the actual path to the user's message text from the Webhook node.

---

## 7. Sending Replies via WhatsApp

Use another **HTTP Request** node to respond:

```http
POST https://graph.facebook.com/v21.0/{{ $json[...] }}/messages
Authorization: Bearer YOUR_LONG_LIVED_TOKEN
Content-Type: application/json

{
  "messaging_product": "whatsapp",
  "to": "{{ $json[...] }}",
  "text": {
    "body": "{{ $node['OpenAI Response'].json.choices[0].message.content }}"
  }
}
```

Use the metadata’s `phone_number_id` for the endpoint and `from` for the recipient. This avoids hardcoding and ensures proper routing.

---

## 8. Common Errors & Fixes

- **Recipient phone number not in allowed list**  
  → Add as test number or switch to Live mode

- **401 – Session expired**  
  → Refresh token via Graph API:
  ```http
  GET https://graph.facebook.com/v21.0/oauth/access_token
  ?grant_type=fb_exchange_token
  &client_id=YOUR_APP_ID
  &client_secret=YOUR_APP_SECRET
  &fb_exchange_token=YOUR_CURRENT_TOKEN
  ```

- **Webhook verification failed**  
  → Ensure verify token matches between Meta and n8n and echo `hub.challenge`

- **No execution data available**  
  → Trigger workflow via actual WhatsApp message, not manual run

---

## 9. Going Live

1. Add a **Privacy Policy URL** in Meta App → Settings → Basic (required for live access)  
2. Switch app to **Live** mode once all compliance items are met  
3. Remove restricted recipient list  
4. Use WhatsApp message templates for messages sent after 24 hours of user interaction

---

## 10. Conclusion & Next Steps

Congrats! You now have a WhatsApp AI Assistant built with n8n and OpenAI.

### Where to go from here:
- Wire up custom knowledge (PDFs, documents)  
- Implement memory for conversation context  
- Launch multilingual support  
- Export n8n workflow as JSON for reuse

---

**Need help?** Join the [n8n Community Forum](https://community.n8n.io/) or OpenAI Discord to connect with fellow builders.

Happy automating!
