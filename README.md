# notepad
from tkinter import *
from tkinter.messagebox import  showinfo
from tkinter.filedialog import askopenfilename,asksaveasfilename
import os
def newfile():
    global  file
    root.title("Untitled-notepad")
    file=None
    Textarea.delete(1.0,END)
def openfile():
    global file
    file=askopenfilename(defaultextension=".txt",filetypes=[("All files","*.*"),("text document","*.txt")])
    if file=="":
        file=None
    else:
        root.title(os.path.basename(file) + "-notedpad")
        Textarea.delete(1.0,END)
        f=open(file,"r")
        Textarea.insert(1.0,f.read())
        f.close()
def savefile():
    global  file
    if file==None:
        file=asksaveasfilename(initialfile='untitled.txt',defaultextension=".txt",filetypes=[("All files","*.*"),("text document","*.txt")])
        if file=="":
            file=None
        else:
            #save file as new file
            f=open(file,"w")
            f.write(Textarea.get(1.0,END))
            f.close()
            root.title(os.path.basename(file)+"-notepad")
            print("file saved")
def quitapp():
    root.destroy()
def cut():
    Textarea.event_generate(("<<Cut>>"))

def copy():
    Textarea.event_generate(("<<Copy>>"))
def paste():
    Textarea.event_generate(("<<Paste>>"))

def about():
    showinfo("notepad","notepad by code with anjali")
if __name__ == '__main__':
    root=Tk()
    root.title("notepad")
    #root.wm_iconbitmap("1.ico")
    root.geometry("644x577")

    #Add textArea
    Textarea=Text(root,font="lucida 13")
    file=None
    Textarea.pack(fill=BOTH,expand=True)

    #lets create a menubar
    Menubar=Menu(root)
    FileMenu=Menu(Menubar,tearoff=0)

    #to open new file
    FileMenu.add_command(label="new",command=newfile)

    #to open already exiting file
    FileMenu.add_command(label="open",command=openfile)

    #save the current file
    FileMenu.add_command(label="save",command=savefile)
    FileMenu.add_separator()
    FileMenu.add_command(label="exit",command=quitapp)
    Menubar.add_cascade(label="File",menu=FileMenu)

    #file Menu ends
   #edit Menu start
    EditMenu=Menu(Menubar,tearoff=0)
    EditMenu.add_command(label="cut",command=cut)
    EditMenu.add_command(label="copy", command=copy)
    EditMenu.add_command(label="paste", command=paste)

    Menubar.add_cascade(label="Edit",menu=EditMenu)

    #edit menu ends
    #help menu start
    HelpMenu=Menu(Menubar,tearoff=0)
    HelpMenu.add_command(label="about notepad",command=about)
    Menubar.add_cascade(label="Help",menu=HelpMenu)
   #help menu ends
  #to give a feature of cut

    root.config(menu=Menubar)

    ##adding scroll bar
    scroll=Scrollbar(Textarea)
    scroll.pack(side=RIGHT,fill=Y)
    scroll.config(command=Textarea.yview)
    Textarea.config(yscrollcommand=scroll.set)
    root.mainloop()
