# Lesson 1. Simple echo-bot

## Introduction

Hello, reader! Accumulated quite a lot of different issues and questions that, in fact, became the reason of creation of this "course".
I must say: the bot, which we will create is only a prototype, the purpose of all these posts to tell about the basics of bot writing, to show how to write a simple bot for their needs.
In this book, as programming language I will use Python 3, but this does not mean that fans of PHP, Ruby, etc. are "out of flight"; all the basic principles are the same. I'm not going to stop on the description of the language, anyone can read the documentation for Python [here](https://docs.python.org/3/).

> Important!
Over time, it became clear that the material from the first version of this tutorial is very outdated, and causes more questions than answers. So from now in this book we will use handlers. If you don't know what is it, read [this](https://ya.ru).

## Prepare to launch

The interaction of bots with people based on the HTTP requests. From the first days of the Bot API launch, I use the library [pyTelegramBotAPI](https://github.com/eternnoir/pyTelegramBotAPI), which takes care of all the nuances of sending and receiving requests, allowing to concentrate directly on the logic. Installing the library is simple:

```bash
$ sudo pip install pytelegrambotapi
```

In order to install the Python Package Manager `pip`, refer to the documentation for your operating system. If You have several Python interpreters installed on your machine, it is better to replace `pip` with `pip3` to install the correct version. Here and below means the use of Ubuntu. To check that everything was installed successfully, run the following command:

```bash
$ python3
```

When the input window will show up (`>>>`), enter `import telebot` and press Enter. If nothing happens, it means the library is installed correctly. How it looks can be seen in picture 1.

![telebot](https://groosha.gitbooks.io/telegram-bot-lessons/content/l1_1.jpg "Telebot installed successfully")

Now you can leave from Python console by pressing Ctrl+Z or Ctrl+D, or just write `exit()`.

## Writing simple echo-bot

Enough words, let's get to it. As a practice the first lesson let's write a bot, that will repeat the sent text message. Create a directory and inside it create 2 files: `bot.py` and `config.py`. I recommend you to make different constants and settings in the file `config.py` in order not to overload others. In file `config.py` type:

```python
# -*- coding: utf-8 -*-
#This token is invalid, can not even try :)
token = '110831855:AAE_GbIeVAUwk11O12vq4UeMnl20iADUtM'
```

token - is what is returned to You [@BotFather](https://telegram.me/botfather) when creating your bot.

Now open `bot.py` and create our bot instance:

```python
# -*- coding: utf-8 -*-
import config
import telebot

bot = telebot.TeleBot(config.token)
```

Now we have to teach the bot to respond to messages. Let's write a handler that will respond to all text messages. (remember: to understand the handlers, you should read [this additional material](https://ya.ru)).

```python
@bot.message_handler(content_types=["text"])
def repeat_all_messages(message): # You can name your function whatever you want
    bot.send_message(message.chat.id, message.text)
```

Now start the loop of obtaining new records from Telegram servers:

```python
if __name__ == '__main__':
    bot.polling(none_stop=True)
```

Function `polling` runs the so-called [Long Polling](http://www.pubnub.com/blog/http-long-polling/), and parameter `none_stop=True` tells, that bot will need to try not to stop working if yourecieve any errors. In this case, of course, you need to watch for the bot because Telegram servers periodically stops responding or doing it with a big delay causing errors 5xx.

Now our `bot.py` looks like this:

```python
# -*- coding: utf-8 -*-
import config
import telebot

bot = telebot.TeleBot(config.token)

@bot.message_handler(content_types=["text"])
def repeat_all_messages(message): # You can name your function whatever you want
    bot.send_message(message.chat.id, message.text)

if __name__ == '__main__':
    bot.polling(none_stop=True)
```

Well done! Just run your bot by typing:

```bash
$ python3 bot.py
```

Now we can make sure that everything is working and the bot is really repeating our message.

![Bot echoes text](https://groosha.gitbooks.io/telegram-bot-lessons/content/l1_2.jpg "Bot repeats everything")

The first lesson is over, now.



