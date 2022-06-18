+++
author = "Chaoming Li"
title = "Python Voice Assistant Source Code"
description = "My first Python project is to create a voice assistant on Raspberry Pi. It’s a fun little project that combines both software and hardware."
date = "2018-01-15"
categories = [
    "Software Development"
]
tags = [
    "python",
    "voice recognition",
    "raspberry pi"
]
image = "pytho-voice-assistant.png"
+++

Back in mid-2015, I created my first and only Python script so far. The script was a voice assistant. I thought I lost the source code, but it’s been in my Dropbox all the time and was named with a weird project name so I didn’t realise that’s it. I just didn’t try to look for it hard enough.

Here are a couple of videos I took back in 2015 when it was built. The script was running on a Raspberry Pi 3 with a USB sound card connecting to a microphone and a speaker.

{{< youtube id="mphr9-64irA" >}}

{{< youtube id="4IHVUHfimCw" >}}

You will notice that it was pretty slow in response. That’s probably because I couldn’t figure out how to lower the sound input sample rate. The rate was 44.1KHz. I think 8KHz is good enough and it can cut down the data size significantly.

How the script works is fairly simple. Get the voice data from the USB sound card, and send it to Google Speech Recognition API if the sound goes above a threshold. Get the text back from Google API and decide what to do. It can only handle two kinds of commands. If you say “(.*)(tell|say)(.*) about (.*)”, it will call Wikipedia for the last “(.*)” which is supposed to be a name or a thing. Otherwise, it will call WolframAlpha to try to get an answer to your question. Once it gets the text for the response, it responds through the speaker.

The most challenging part of this project was not coding. The original plan was to use a Bluetooth speaker with microphone and I spent a long time trying to make the speaker work with Raspberry Pi but failed.

Now, here is the source code. I can’t guarantee it still works since it’s over 2 years old. It might not even be the working version on my Raspberry Pi. But I hope this is helpful for anyone who is interesting in building a voice assistant.

```python
import speech_recognition as sr
import time
import pyttsx
import threading
import wikipedia
import re
import wolframalpha

def listen():

    def say(text):
        engine = pyttsx.init()
        engine.setProperty("rate", 150)
        engine.setProperty("volume", 0.3)
        engine.say(text)
        engine.runAndWait()

    def wiki(text):
        summary = wikipedia.summary(text, sentences=3)
        summary = re.sub('\([^\)]*\)', '', summary)
        summary = re.sub('\/[^\/]*\/', '', summary)
        return summary

    def wolfra(question):
        result = ""
        client = wolframalpha.Client("Your WolframAlpha API Key")
        res = client.query(speechtext)
        if(len(res.pods) > 0):
            results = list(res.results)
            result = results[0].text
        return result

    r = sr.Recognizer()
    m = sr.Microphone()
    m.RATE = 44100
    m.CHUNK = 512

    say("How can I help you?")
    print("A moment of silence, please...")
    with m as source:
        r.adjust_for_ambient_noise(source)
        if(r.energy_threshold < 2000):
            r.energy_threshold = 2000
        print("Set minimum energy threshold to {}".format(r.energy_threshold))
        print("Say something!")
        audio = r.listen(source)
        print("Got it! Now to recognize it...")
        say("one moment, let me think")
        try:
            speechtext = r.recognize(audio)
            print("You said: " + speechtext)
            pattern = re.compile("(.*)(tell|say)(.*) about (.*)")
            result = pattern.match(speechtext)
            if(result != None):
                answer = wiki(result.group(4))
                print(answer)
                say(answer)
            else:
                answer = wolfra(speechtext)
                if not answer:
                    print("sorry, I don't know")
                    say("sorry, I don't know")
                else:
                    print(answer)
                    say(answer)
        except LookupError:
            print("sorry, I didn't catch that")
            say("sorry, I didn't catch that")
while True:
    listen()
```

If you want to build a voice assistant, I suggest trying DialogFlow as the response engine. It can make responses more natural, and it can have a conversation to gather all the required information for a task (e.g. book a flight ticket).