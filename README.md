# Bibliotheca
- A package created for <a href="https://github.com/larrikin-coder/Pandora-Ai.git">Pandora</a> utilizing this package for more minimal and polyglot's oriented interface.
- New UI/UX specifically harnessing the power of the package for linguistic purpose.
- It also have capabilities for offline storage of the searched semantics and meanings.
- We can also translate words from one language to another for time being using only few Languages.
```py
def convert():
    def cmd(event):
        global lang
        lang = n.get()
    def find():
        try:
            if lang == "Languages":
                pyautogui.alert("No language selected","Attention!")
            if lang == "English":
                query = e1.get().strip()#english word
                dictionary = PyDictionary()
                translate = dictionary.translate(query,'en')
                title = "{} word in English".format(query)
                pyautogui.alert(translate,title)
            else:
                d1 = {'Spanish':'es','Hindi':'hi'}
                dictionary = PyDictionary()
                language = d1.get(lang)#language to be converted in
                print(language)
                word = e1.get().strip()#word
                print(word)
                trans_wrd = dictionary.translate(word,language)
                print(trans_wrd)
                title = "{} word in {}".format(word,lang)
                print(title)
                pyautogui.alert(trans_wrd,title)
        except Exception as e:
            print(e)
```

# Installation
- Installation Wizard also created which helps to create database in mysql
```py
def install():
    def createbrain():
        mycon = con.connect(host="localhost",user="root",passwd=z3)
        if mycon.is_connected():
            print("BODY connected to BRAIN")
        cur = mycon.cursor()
        query = "create database if not exists bibliotheca"
        cur.execute(query)

    def credentials():
        global z3
        x = e1.get()#name
        y = e2.get()#email id
        z = e3.get()#password
        z1 = ex.get()#gender
        z2 = e4.get()#confirm password
        z3 = e5.get()#mysql password
        print(x)
        print(y)
        print(z1)
        print(z2)
        print(z3)
        createbrain()
        mycon = con.connect(host="localhost",user="root",passwd=z3,database="bibliotheca")
        if mycon.is_connected():
            pass
        cur = mycon.cursor()
        query = "create table if not exists credentials(user_name varchar(30) not null,emailid varchar(40) not null,gender char(1),passwd varchar(30) not null)"
        cur.execute(query)
        query2 = "insert into credentials values('{}','{}','{}','{}')".format(x,y,z1,z)
        print(query2)
        cur.execute(query2)
        mycon.commit()
        root.destroy()

    root = tk.Tk()
    root.title("installation")
    l1 = tk.Label(text = "Bibliotheca installation wizard")
    l2 = tk.Label(text = "user name:")
    l3 = tk.Label(text = "email id:")
    lx = tk.Label(text = "gender(M/F)")
    l4 = tk.Label(text = "password:")
    l5 = tk.Label(text = "confirm password:")
    l6 = tk.Label(text = "mysql password:")
    l1.grid(column = 1,row = 0)
    l2.grid(column = 0,row = 1)
    l3.grid(column = 0,row = 2)
    lx.grid(column = 0,row = 3)
    l4.grid(column = 0,row = 4)
    l5.grid(column = 0,row = 5)
    l6.grid(column = 0,row = 6)
    e1 = tk.Entry(root,width=30) #user name
    e2 = tk.Entry(root,width=30) #email id
    ex = tk.Entry(root,width=10) #gender
    e3 = tk.Entry(root,show="*",width=30) #password
    e4 = tk.Entry(root,show="*",width=30) #confirm password
    e5 = tk.Entry(root,show="*",width=30) #mysql password
    e1.grid(column = 1,row = 1)
    e2.grid(column = 1,row = 2)
    ex.grid(column = 1,row = 3)
    e3.grid(column = 1,row = 4)
    e4.grid(column = 1,row = 5)
    e5.grid(column = 1,row = 6)
    photo = PhotoImage(file = r"C:\Users\SHAURYA\Desktop\school project 2.0\resources\installer.png")
    photo2 = PhotoImage(file = r"C:\Users\SHAURYA\Desktop\school project 2.0\resources\ok.png")
    root.iconphoto(False,photo)
    ttk.Button(text = "okay",image = photo2,command = credentials).grid(column = 2,row =7)
    root.geometry()
    root.mainloop()
```
# OCR capabilities
- Utilized tesseract-OCR to extract words from images and then find the semantics and meaning of the recognized words.
```py
def upload_photo():
        pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract'
        window.filename=filedialog.askopenfilename(initialdir="/", title="Select A File", filetypes=(("png files", "*.png"),("jpeg files","*.jpg"),("all files","*")))
        lbl = tk.Label(window,text=window.filename)
        directory = lbl.cget("text")
        x = (pytesseract.image_to_string(directory))
        if lang == "English":
            dictionary = PyDictionary()
            translate = dictionary.translate(x,'en')
            title = "{} word in {}".format(x,lang)
            pyautogui.alert(translate,title)
        else:
            d1 = {'Spanish':'es','Hindi':'hi'}
            language = d1.get(lang)
            dictionary = PyDictionary()
            translate = dictionary.translate(x,language)
            title = "{} word in {}".format(x,lang)
            pyautogui.alert(translate,title)

    root.destroy()
    window = tk.Tk()
    window.title("Convert English Words to preferred Languages")
    photo = PhotoImage(file = r"C:\Users\SHAURYA\Desktop\school project 2.0\resources\dictionary.png")
    window.iconphoto(False,photo)
    ttk.Label(window,text = "Convert English Words to preferred Languages").grid(column = 1,row = 0)
    ttk.Label(window,text = "English Word-").grid(column = 0,row = 4,pady = 10)
    e1 = ttk.Entry(window)
    e1.grid(column = 1,row = 4,pady = 10)
    n = tk.StringVar()
    lang_combo = ttk.Combobox(window,width = 20,textvariable = n)
    lang_combo.bind('<<ComboboxSelected>>',cmd)
    lang_combo['values'] = ('Languages','English','Spanish','Hindi')
    lang_combo.grid(column = 2,row = 4,pady = 10)
    lang_combo.current(0)# default value of language
    ttk.Button(window,text="FIND",command=find).grid(column = 0,row = 7,pady = 10)#pady
    ttk.Button(window,text="UPLOAD PHOTO",command=upload_photo).grid(column = 2,row = 7,pady = 10,sticky = 'e')#pady
    window.geometry("600x150")
    window.mainloop()
```
