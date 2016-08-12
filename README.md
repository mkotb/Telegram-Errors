# Telegram-Errors
Documentation of all errors sent from the Telegram Bot API

## Formatting errors

If you tell the API to parse your text as markdown or html, it may throw errors if the text is not formatted correctly.

In case of markdown, it will send an error with the following message: "Can't parse message text: Can't find end of the entity starting at byte offset x." In case of HTML formatting, it will send an error with the following message: "Can't parse message text: Can't found end tag corresponding to start tag at byte offset x."

This issue may be caused by a simple typo or error, or a case where user input isn't escaped.

Escaping your text for Telegram can be a little confusing due to the lack of documentation out there:

### Escaping Markdown

After a little bit of trial and error, I found out by chance that there is an extremely easy way to escape your text, use square brackets! Merely wrapping your text in square brackets will escape the text, you will have to escape `]` though, which you can do by replacing it by `]][`. Although a backslash would've been more simple and easy to guess, it's a fairly simple solution.

### Escaping HTML

Assuming your text is in unicode, you can escape the HTML as if it were XML:

```
& becomes &amp;
< becomes &lt;
> becomes &gt;
```

## Markup errors

This section outlines errors sent by the API when parsing markup such as inline keyboards, reply keyboards, etc.

### InlineKeyboardButton

As sending an empty inline keyboard is completely valid, let's outline the errors that can occur with buttons:

#### text

If this is the only field sent, Telegram will return "Can't parse inline keyboard button: Text buttons are unallowed in the inline keyboard"

Note that Telegram clients *will* cut off your text if it's too long, there is no known maximum that the API will not accept.

#### url

Telegram will respond with "BUTTON\_URL\_INVALID" if the url is invalid or contains spaces.

#### callback_data

If the data is more than 64 bytes, Telegram will respond with "BUTTON\_DATA\_INVALID"

## getMe

Used at init., it will throw a 302 and an odd HTML response if an invalid key is provided:

```html
<html>
  <head><title>302 Found</title></head>
  <body bgcolor="white">
    <center><h1>302 Found</h1></center>
    <hr><center>nginx/1.10.0</center>
  </body>
</html>
```

## sendMessage

This method can throw a [formatting error](#formatting-errors), or a set of errors based on input:

### text

If not provided, Telegram will return "Message text is empty."

If misformatted, Telegram will throw a [formatting error](#formatting-errors)

### chat_id

If not provided, Telegram will return "chat_id is empty"

If the bot doesn't have access to the chat, or it doesn't exist, Telegram will return "chat not found"

### reply_to_message_id

If the message id is invalid, Telegram will return "message not found"

### reply_markup

Refer to [Markup errors](#markup-errors)
