---
title: exclaimer-mail-disclaimer
description: 
published: true
date: 2023-12-31T14:38:03.279Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:38:00.085Z
---

# Exclaimer Mail Disclaimer

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-signatur-wird-bei-mo-1">Signatur wird bei Mobilen Geräten nicht an der richtigen stelle angezeigt</span>

Disclaimer is placed at the bottom of the message when using Entourage\\iPhone\\Gmail

SYMPTOMS

The disclaimer is placed at the bottom of the message when replying to or forwarding a message when using Entourage, an iPhone or Gmail.

  
CAUSE

Entourage, the iPhone and Gmail use a reply separator that is set on the date, time and the sender of the original message like the example below: On 22/02/08 11:00, "Joshua Clarke" wrote:

This reply separator is not static and will always change; therefore Mail Utilities can not recognise the reply separator. The reply separator is used by Mail Utilities to determine where the disclaimer should be placed.

  
RESOLUTION

Entourage and Gmail

This issue can be resolved by adding a reply separator that Mail Utilities recognises, in the signature for replies and forwards section of Entourage or the signature section in Gmail. We recommend using the following reply separator:

-Original Message-

You can also add the Entourage forwarded message reply separator into Mail Utilities. 1.Open the Exclaimer Mail Utilities control panel 2.Click on Set up from the menu on the left 3.Click on the Advanced button in the bottom right of the Set up page 4.Click on the Identifiers tab 5.In the Reply Separators section add the following reply separator: -Forwarded Message

Please note that there must be a semi colon and space before the new entry like so:

-Original Message -; -Forwarded Message

iPhone

The iPhone does not have a separate signature section for new mails, replies or forwards but just one generic signature section. Therefore we recommend adding a line of ten underscores as the signature for all messages. This will ensure the signature\\disclaimer will be applied in the correct location as the line of ten underscores is a recognised reply separator to Mail Utilities. This does not require amendment in the reply separator in Mail Utilities, only on the iPhone.

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.exclaimer.de/support/kb/68/disclaimer-is-placed-at-the-bottom-of-the-message-when-using-entourage-iphone-gmail" rel="nofollow">http://www.exclaimer.de/support/kb/68/disclaimer-is-placed-at-the-bottom-of-the-message-when-using-entourage-iphone-gmail</a>
```