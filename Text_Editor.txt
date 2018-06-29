from tkinter import *
from tkinter import filedialog,scrolledtext,END,messagebox,simpledialog
import os

#--------------------------------------WINDOW-CREATION-----------------------------------------------------------------
root = Tk()
root.title("Silk v1.0 Text Editor")
textArea = scrolledtext.ScrolledText(root,width=100,height=10)
textArea.pack()

#----------------------------------------FUNCTIONS----------------------------------------------------------------------
def new_():
    if len(textArea.get('1.0',END+'-1c')) > 0:
        if messagebox.askyesno("Closing file","Do you want to save?"):
            save_()
    else:
        textArea.delete('1.0',END)

def open_():
    file = filedialog.askopenfile(initialdir="/", title='Select a text file',filetypes=[("Text file","*.txt"),("All files","*.*")])
    textArea.delete('1.0',END)
    root.title(os.path.basename(file.name) + " - Silk v1.0 Text Editor")
    if file != None:
        content = file.read()
        textArea.insert('1.0',content)
        file.close()
        
def save_():
    file = filedialog.asksaveasfile(mode="w",defaultextension=".txt",filetypes=[("HTML file","*.html"),("Text file","*.txt"),("All files","*.*")])
    if file != None: 
        saved = textArea.get('1.0',END+'-1c')
        file.write(saved)
        file.close()
       
def about_():
    lbl = messagebox.showinfo("About","Text created on Python by Saurav Verma. All Rights Reserved. ")            
    lbl.pack()
    
def exit_():
    if messagebox.askyesno("Quit","Are you sure to exit?"):
        root.destroy()
        
def find_():
    findString = simpledialog.askstring("Find","Enter Text")
    textData = textArea.get('1.0',END+'-1c')
    occurs = textData.upper().count(findString.upper())
    if textData.upper().count(findString.upper()):
        label1 = messagebox.showinfo("Result",findString + " has " +str(occurs) +" occurences")
    else:
        label1 = messagebox.showinfo("Result","No match found")        
        
def undo_():
    textArea.edit_undo()

def redo_():
    textArea.edit_redo()
    
def FontHelvetica():
   global text
   textArea.config(font="Helvetica")

def FontCourier():
   global text
   textArea.config(font="Courier")
   
def FontTimes():
   global text
   textArea.config(font="Times")   

def FontArial():
   global text
   textArea.config(font="Arial")
           
def FontChiller():
   global text
   textArea.config(font="Chiller") 
    
#--------------------------------MENU-OPTIONS---------------------------------------------------------------
menu = Menu(root)
root.config(menu=menu)
fileMenu = Menu(menu)
menu.add_cascade(label="File",menu=fileMenu)
fileMenu.add_command(label="New",command=new_)
fileMenu.add_command(label="Open",command=open_)
fileMenu.add_command(label="Save",command=save_)
fileMenu.add_command(label="Print")
fileMenu.add_separator()
fileMenu.add_command(label="Exit",command=exit_)
#--------------------------------EDIT-MENU-------------------------------------------------------------------
editMenu = Menu(menu)
menu.add_cascade(label="Edit",menu=editMenu)
editMenu.add_command(label = "Find Text",command=find_)
editMenu.add_command(label = "Undo",command=undo_)
editMenu.add_command(label = "Redo",command=redo_)
#-------------------------------FONTS-MENU-------------------------------------------------------------------
fontMenu = Menu(menu) 
menu.add_cascade(label="Fonts",menu=fontMenu)
helvetica=IntVar() 
courier=IntVar()
fontMenu.add_checkbutton(label="Courier", variable=courier, command=FontCourier)
fontMenu.add_checkbutton(label="Helvetica", variable=helvetica, command=FontHelvetica)    
fontMenu.add_checkbutton(label="Times New Roman", variable=helvetica, command=FontTimes)  
fontMenu.add_checkbutton(label="Arial", variable=helvetica, command=FontArial)  
fontMenu.add_checkbutton(label="Chiller", variable=helvetica, command=FontChiller)  
#------------------------------HELP-MENU---------------------------------------------------------------------
helpMenu = Menu(menu)
menu.add_cascade(label="Help",menu=helpMenu)
helpMenu.add_command(label = "About",command=about_)
#---------------------------CLOSE-THE-PROGRAM----------------------------------------------------------------
root.mainloop()
