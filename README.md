# Hugo-the-Virtual-Assistant

import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import os
from tkinter import*
from tkinter.filedialog import*
import tkinter.font as font
from threading import *
import time
from playsound import playsound
import random

engine=pyttsx3.init('sapi5')
voice=engine.getProperty('voices')
#print(voice)
engine.setProperty('voice',voice[0].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()
def wishme():
    hour=int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("good morning")
    elif hour >=12 and hour < 18:
        speak("good noon")
    else:
        speak("good night")
def take():
    r=sr.Recognizer()
    with sr.Microphone() as source:
        text.delete(0, END)
        listen = "listening..."
        text.insert(0, listen)
        print("lisining....")
        #r.pause_threshold = 5.5
        r.energy_threshold = 1000
        audio=r.listen(source)
        print("hi")
    try:
        text.delete(0, END)
        text.insert(0, "recognizing...")
        print("recognizing...")
        query=r.recognize_google(audio,language='en-in')
        print(query)
       #speak(query)
    except Exception as e:
        #print(e)
        text.delete(0, END)
        text.insert(0, "try again...")
        print("try again...")
        return "None"
    return query
def l_main():

    try:
        if 1:

            query =take().lower()
            if 'wikipedia' in query:
                text.insert(0, "searching in wikipedia...")
                text.delete(0, END)
                speak("searching in wikipedia...")
                query = query.replace("wikipedia", "")
                result = wikipedia.summary(query, sentences=2)
                print("according to wikipedia...")
                print(result)
                speak(result)

            elif 'open google' in query:
                text.insert(0, "opening Google...")
                text.delete(0, END)
                webbrowser.open("google.com")
            elif 'open' in query:
                cou = 0
                st = 0
                te = query
                l = list(te)
                for i in te:
                    if i == 'o':
                        if l[cou + 1] == 'p':
                            st = cou + 5
                            break
                    cou = cou + 1
                val = list()
                val = l[st:]
                org = ''.join(map(str, val))
                com=".com"
                real=org+com
                webbrowser.open(real)

            elif 'play music' in query:
                text.insert(0, "playing music...")
                text.delete(0, END)
                musicp = askdirectory()
                songs = os.listdir(musicp)
                i=random.randint(0,(len(songs)-1))
                print(len(songs))
                os.startfile(os.path.join(musicp, songs[i]))

            elif 'time' in query:
                timel = datetime.datetime.now().strftime('%H:%M:%S')
                temp=f"Time Is-{timel}"
                text.insert(0,temp)
                text.delete(0, END)
                speak(f"time is {timel}")

    except Exception as e:
        print(e)
        print("try again...")



def thread_main():
    thread=Thread(target=l_main)
    thread.start()


if name == 'main':

    main = Tk()

    main.title("HUGO")

    main.geometry("710x444+550+0")

    #background image
    img=PhotoImage(file="sar.png")
    img_l=Label(main,image=img)
    img_l.pack(side=TOP)

   #frame  for start button
    frame=Frame(main,bg="#000000")
    frame.place(x=270,y=330,height=50,width=150)

    #button   start
    myFont = font.Font(size=30)
    bt=Button(frame,text="START", bg='#000000', fg='#53afe0',command=thread_main)
    bt['font'] = myFont
    bt.pack()

    #For text
    frame_t = Frame(main, bg="#000000")
    frame_t.place(x=90, y=60, height=24, width=500)
