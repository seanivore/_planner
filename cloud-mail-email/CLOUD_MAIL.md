# Cloud-Mail by Cloudflare 

**Created**: 2026-05-07
**Status**: Researching 

--- 

## Overview 

The product is very being buzzed about a lot online right now. I had in notes, grabbed from Twitter. Then this week I came across a list of AI tools that included Agent Inboxes using this Cloudflare Service. I recognized the "workers" and other terms from the setup for Resent in my `~/Development/everlastings-website` project. 

+ Investigate 'cloud-mail' 
  - Uses Cloudflare Workers
  - Read about it being an option for free send/receive setup

+ Agentic Inbox
  - A self-hosted email client with an AI agent 
  - Runs entirely on Cloudflare Workers
  - [GitHub: Agentic Inbox](https://github.com/cloudflare/agentic-inbox)
  - [Cloudflare Blog: Email for Agents](https://blog.cloudflare.com/email-for-agents/)

### Objective 

I want to learn more, be aware of opportunities for myself and for development and business marketing, etc. purposes. 

  1. What is Cloud-Mail? 
  2. What use cases might it have for me personally? 
  3. Use-cases for me in business, my own or clients? 

---

## My Domain, Email & Cloudflare Setup 

  - My custom domain `august.style` 
    - Owned by SquareSpace 
    - DNS fully managed by Cloudflare
    - Has Cloudflare Nameservers set up 
   
  - R2 CDN 
    - I use for my portfolio project`~/Development/360-design`
    - We implemented it into my client project `~/Development/everlastings-website`
  
  - We did some kind of magic in Cloudflare for my email receipt 
    - Gmail inbox settings doesn't allow you to pull non-Google Workspace emails into your public email inbox 
    - Cloudflare and and agent set it up so it works perfectly for me 

  - iCloud Custom Domain Email Host manages `august.style` email aliases 
    - Which I'd love to explore leaving 
    - It is nice that it costs $2.99 that Apple forces you to pay for iCloud 200GB anyway 
    - I've moved to Google Workspace and back a few times because of cost 

### Jump-Start Links

- [Easily creating and routing email addresses with Cloudflare Email Routing](https://blog.cloudflare.com/introducing-email-routing/)
- [Migrating to Cloudflare Email Routing](https://blog.cloudflare.com/migrating-to-cloudflare-email-routing/)
- [Email Routing is now in open beta, available to everyone](https://blog.cloudflare.com/email-routing-open-beta/)

### Email Routing FAQs

- **What is Cloudflare Email Routing?**
  Cloudflare Email Routing is a free and private service that allows Cloudflare users to create custom email addresses for their domains and have the messages forwarded to any inbox, without exposing their primary email addresses.
- **How does Cloudflare Email Routing work?** 
  Cloudflare Email Routing modifies the SMTP envelope to deliver the message to a chosen destination inbox while keeping the original email headers and body intact.
- **Is Email Routing free to use?**
  Creating custom addresses and forwarding messages to your inbox is free.
- **Does Cloudflare store the emails that are routed?**
  Cloudflare Email Routing is private. Cloudflare does not store or access the content of your emails.
- **How does the service handle spam?**
  Cloudflare uses phishing detection to help prevent spam from being forwarded to destination mailboxes.
- **Can I use Email Routing for my business?**
  Businesses can use Cloudflare Email Routing to route emails for different types of inquiries, like sales or support, and easily manage who receives these messages.
- **Can I see analytics for my routed emails?**
  Cloudflare Email Routing provides insights into a number of metrics, including how many emails were sent, whether they were successfully forwarded or dropped, and the delivery success rate at the destination mailbox.

--- 

**Primary Question**: Can I leave iCloud Custom Domains and still have my email go through my personal gmail inbox? 

---

Please continue researching the above. This might be helpful: `https://developers.cloudflare.com/`, as well as `cloud-mail-email/EMAIL_ROUTING_DOCS.md`. 

---