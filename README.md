from tkinter import *

from tkinter import Tk

from tkinter import scrolledtext

from tkinter import ttk,messagebox

from tkinter import filedialog

import textblob

#pip install googletrans

from PIL import Image,ImageTk

import googletrans

from googletrans import Translator,LANGUAGES

from playsound import playsound

from gtts import gTTS

import speech_recognition as sr

import os

#from langdetect import detect

#from google_trans_new import google_translator  

#title

win=Tk()

win.title("Language Translator")

win.geometry("450x350") #window size

win.minsize(450,350) 

  

'''#for arrow

arrow_button=ImageTk.PhotoImage(file="/home/student/Documents/python_project/translate.png")

convert=Button(win, image=arrow_button,cursor="hand2",borderwidth=0)

convert.place(x=600,y=600)'''

#for background image

bg=ImageTk.PhotoImage(file=r"C:\\Users\\hp\\Downloads\\New folder\\bg.png") 

canvas=Canvas(win,width=50,height=0)

canvas.pack(fill="both",expand=True)

canvas.create_image(0,0, image=bg, anchor="nw")

#functions for button

#for translate button

def translation():

    text_=text1.get(1.0,END)

    t1=Translator()

    trans_text=t1.translate(text_,src=combo1.get(),dest=combo2.get())

    trans_text=trans_text.text

    text2.delete(1.0,END)

    text2.insert(END,trans_text)

def translate():

    global language

    try:

        txt=text1.get(1.0,END)

        c1=combo1.get()

        c2=combo2.get()

        if(txt):

            words=textblob.TextBlob(txt)

            lan=words.detect_language()

            for i,j in language.items():

                if(j==c2):

                    lan_=i

            words=words.translate(from_lang=lan,to=str())

            text2.delete(1.0,END)

            text2.insert(END,words) 

    except Exception as e:

        messagebox.showerror("try again")

def clear():

    text1.delete(1.0,END)

    text2.delete(1.0,END)

    combo1.current(0)

    combo2.current(0)

    word_count_label.config(text=f"Word count: 0")

def show():

    v1=combo1.get()

    v2=combo2.get()

    win.after(1000,show) 

    

def exit():

    win.destroy()

    

  

def Text_to_speech():

    Message= text2.get(1.0,END)

    speech=gTTS(text=Message)

    speech.save("speaker.mp3")

    playsound("speaker.mp3")

    os.remove("speaker.mp3")

r= sr.Recognizer() 

def Speech_to_text():

    print("hello")

    r = sr.Recognizer()

    language = LANGUAGES

    with sr.Microphone() as source:

        

        pyaudio = r.listen(source)

    try:

        print("YOU said"+r.recognize_google(pyaudio))

        text = r.recognize_google(pyaudio)

        text1.insert( END,text)

    except sr.UnknownValueError:

        messagebox.showerror(ValueError, "Sorry could not recognize your speech.")

    except sr.RequestError as e:

        messagebox.showerror(

            END, "Could not request results from google speech recognition service;{0}".format(e)) 

options=[LANGUAGES] 

                     

languages=googletrans.LANGUAGES

languagesV=list(languages.values())

languagesV.insert(0, "SELECT LANGUAGE")

lang1=list(languages.keys())

lang1.insert(0, "")

#language selector

combo1=ttk.Combobox(win,values=languagesV, font="Roboto 14",state="NORMAL")

combo1.place(x=180,y=100)

combo1.set("AUTO-DETECT")

'''label_img1=ImageTk(file="WhatsApp Image 2023-04-24 at 12.20.02.jpeg") 

label1=Label(win,text="TEXT TO TRANSLATE",image= label_img1 )#font="Aerial 12 bold",fg="white",

             #bg="black",width=30,height=3,bd=3,relief=GROOVE)

label1.place(x=155,y=30)'''

label1 = Label(win, text="LANGUAGE TO CONVERT FROM", font="Aerial", fg="white",

               bg="black", width=30, height=3, bd=3, relief=GROOVE)

label1.place(x=165, y=30)

#language selector

combo2=ttk.Combobox(win,values=languagesV,font="Roboto 14",state="NORMAL")

combo2.place(x=900,y=100)

combo2.set("Select Language")

#combo2.current(0)

'''label_img=ImageTk(file="WhatsApp Image 2023-04-24 at 12.19.55.jpeg")

label2=Label(win,text="TRANSLATION",image=label_img)

label2.place(x=890,y=30)'''

label2 = Label(win, text="LANGUAGE TO CONVERT IN", font="Aerial", fg="white",

               bg="black", width=30, height=3, bd=3, relief=GROOVE)

label2.place(x=880, y=30)

def update_word_count(event):

    words = text1.get("1.0", "end-1c").split()

    count = len(words)

    word_count_label.config(text=f"Word count: {count}")

word_count_label =Label(win, text="Word count: 0")

word_count_label.place(x=90, y=450)

 

#for box 1

frame1=Frame(win,bg="white",bd=0)

frame1.place(x=90,y=150,width=440,height=300)

#input text

text1=scrolledtext.ScrolledText(frame1,font="Roboto 20",bg="#99ffff",relief=GROOVE,wrap=WORD)

text1.place(x=0,y=0,width=500,height=300)

text1.pack(fill=BOTH, expand=True)

scrollbar1=Scrollbar(frame1, orient='horizontal')

scrollbar1.pack(side="bottom",fill='x')

scrollbar1.configure(command=text1.xview)

text1.configure(xscrollcommand=scrollbar1.set)

scrollbar2=Scrollbar(frame1, orient='vertical')

scrollbar2.pack(side="right",fill='y')

scrollbar2.configure(command=text1.yview)

text1.configure(yscrollcommand=scrollbar2.set)

#for box 2

frame2=Frame(win,bg="white",bd=0)

frame2.place(x=800,y=150,width=440,height=300)

#output text

text2=scrolledtext.ScrolledText(frame2,font="Roboto 20",bg="#ffcccc",relief=GROOVE,wrap=WORD)

text2.place(x=0,y=0,width=500,height=300)

text2.pack(fill=BOTH, expand=True)

#for bottons 

trans=Button(win, command=translation, text="Translate", font=("Roboto", 15), relief=RAISED,

               activebackground="purple", cursor="hand2", width=10, height=1, bg="black", fg="white")

trans.place(x=400,y=550)

clearbtn=Button(win, command=clear, text="Clear", font=("Roboto", 15), relief=RAISED,

                  activebackground="lightblue", cursor="hand2", width=10, height=1, bg="black", fg="white")

clearbtn.place(x=650,y=550)

exitbtn=Button(win,command=exit,text="Exit",font=("Roboto", 15), relief=RAISED,

                  activebackground="lightblue", cursor="hand2", width=10, height=1, bg="black", fg="white")

exitbtn.place(x=900,y=550)

'''arr=Button(win,command=exit,text="Exit", cursor="hand2")

arr.place(x=560,y=250)

mic_button=ImageTk.PhotoImage(file="C:\\Users\\hp\\Downloads\\New folder\\mic.png")

mic=Button(text="microphone",)

mic.place(x=500,y=460)

spk_button=ImageTk.PhotoImage(file="C:\\Users\\hp\\Downloads\\New folder\\speaker.png")

spk=Button(text="speaker",image=spk_button,command=Text_to_speech)

spk.place(x=1200,y=460)'''

mic_button = Button(win, text="mic", command=Speech_to_text, width=6)

mic_button.place(x=450, y=460)

spk_button = Button(win, text="Speaker", font="aerial 10 bold",

                    command=Text_to_speech, width=6)

spk_button.place(x=1200, y=460)

def browse_file():

    # Open a file dialog and return the file path

    file_path = filedialog.askopenfilename()

    return file_path

def open_file_in_textbox():

    # Get the file path from the browse_file function

    file_path = browse_file()

    # Open the file and read its contents

    with open(file_path, 'r') as file:

        file_contents = file.read()

    # Set the contents of the text box to the file contents

    text1.insert(END, file_contents)

# Create a button with the "Browse" label

browse_button = Button(win, text="Browse", command=open_file_in_textbox,font=("Roboto", 15), relief=RAISED,

                  activebackground="lightblue", cursor="hand2", width=10, height=1, bg="black", fg="white")

browse_button.place(x=190, y=550)

# Bind the <KeyRelease> event to the search function

text1.bind("<KeyRelease>", update_word_count)

show() 

 

win.mainloop()







