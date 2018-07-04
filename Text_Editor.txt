from tkinter import *
from tkinter import filedialog,scrolledtext,END,messagebox,simpledialog
import os

#--------------------------------------WINDOW-CREATION-----------------------------------------------------------------
root = Tk()
root.title("Silk v1.0 Text Editor")

root.iconbitmap('C:/Users/Intel/Desktop/TE images/silk.ico')
root.geometry('800x500')                

#----------------------------------------FUNCTIONS----------------------------------------------------------------------
def new_():
    if len(content_text.get('1.0',END+'-1c')) > 0:
        if messagebox.askyesno("Closing file","Do you want to save?"):
            save_()
    else:
        content_text.delete('1.0',END)

def open_():
    file = filedialog.askopenfile(initialdir="/", title='Select a text file',filetypes=[("Text file","*.txt"),("All files","*.*")])
    content_text.delete('1.0',END)
    root.title(os.path.basename(file.name) + " - Silk v1.0 Text Editor")
    if file != None:
        content = file.read()
        content_text.insert('1.0',content)
        file.close()
        
def save_():
    file = filedialog.asksaveasfile(mode="w",defaultextension=".txt",filetypes=[("HTML file","*.html"),("Text file","*.txt"),("All files","*.*")])
    if file != None: 
        saved = content_text.get('1.0',END+'-1c')
        file.write(saved)
        file.close()
       
def about_():
    lbl = messagebox.showinfo("About","Silk Text Editor by Saurav Verma. All Rights Reserved. ")            
    lbl.pack()
    
def exit_():
    if messagebox.askyesno("Quit","Are you sure to exit?"):
        root.destroy()
        
def find_():
    findString = simpledialog.askstring("Find","Enter Text")
    textData = content_text.get('1.0',END+'-1c')
    occurs = textData.upper().count(findString.upper())
    if textData.upper().count(findString.upper()):
        label1 = messagebox.showinfo("Result",findString + " has " +str(occurs) +" occurences")
    else:
        label1 = messagebox.showinfo("Result","No match found")        
        
def undo_():    
    content_text.edit_undo()

def redo_():
    content_text.edit_redo()
    
def FontHelvetica():
   global text
   content_text.config(font="Helvetica")

def FontCourier():
   global text
   content_text.config(font="Courier")
   
def FontTimes():
   global text
   content_text.config(font="Times")   

def FontArial():
   global text
   content_text.config(font="Arial")
           
def FontChiller():
   global text
   content_text.config(font="Chiller") 

def change_theme(event=None):
    selected_theme = theme_choice.get()
    fg_bg_colors = color_schemes.get(selected_theme)
    foreground_color, background_color = fg_bg_colors.split('.')
    content_text.config(background=background_color, fg=foreground_color)

def update_line_numbers(event=None):
    line_numbers = get_line_numbers()
    line_number_bar.config(state='normal')
    line_number_bar.delete('1.0', 'end')
    line_number_bar.insert('1.0', line_numbers)
    line_number_bar.config(state='disabled')

def get_line_numbers():
    output = ''
    if show_line_number.get():
        row, col = content_text.index("end").split('.')
        for i in range(1, int(row)):
            output += str(i) + '\n'
    return output

def on_content_changed(event=None):
    update_line_numbers()
    update_cursor()

def show_cursor():
    show_cursor_info_checked = show_cursor_info.get()
    if show_cursor_info_checked:
        cursor_info_bar.pack(expand='no', fill=None, side='right', anchor='se')
    else:
        cursor_info_bar.pack_forget()

def update_cursor(event=None):
    row, col = content_text.index(INSERT).split('.')
    line_num, col_num = str(int(row)), str(int(col) + 1)  # col starts at 0
    infotext = "Line: {0} | Column: {1}".format(line_num, col_num)
    cursor_info_bar.config(text=infotext)

#--------------------------------ICON'S-LIST---------------------------------------------------------------
new_file_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/add.png')
open_file_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/open.png')
save_file_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/save.png')
cut_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/cut.png')
copy_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/copy.png')
undo_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/undo.png')
redo_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/redo.png')
find_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/find.png')
about_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/about.png')
exit_icon = PhotoImage(file='C:/Users/Intel/Desktop/TE images/Icons/exit.png')    

#--------------------------------MENU-OPTIONS---------------------------------------------------------------
menu = Menu(root)
root.config(menu=menu)
fileMenu = Menu(menu)
menu.add_cascade(label="File",menu=fileMenu)
fileMenu.add_command(label="New",accelerator='New',command=new_,image=new_file_icon)
fileMenu.add_command(label="Open",accelerator='Open',command=open_,image=open_file_icon)
fileMenu.add_command(label="Save",accelerator='Save',command=save_,image=save_file_icon)
fileMenu.add_separator()
fileMenu.add_command(label="Exit",accelerator='Exit',command=exit_,image=exit_icon)

#--------------------------------EDIT-MENU-------------------------------------------------------------------
editMenu = Menu(menu)
menu.add_cascade(label="Edit",menu=editMenu)
editMenu.add_command(label = "Find Text",accelerator='Find Text',underline=0,command=find_,image=find_icon)
editMenu.add_command(label = "Undo",accelerator='Undo',underline=0,command=undo_,image=undo_icon)
editMenu.add_command(label = "Redo",accelerator='Redo',underline=0,command=redo_,image=redo_icon)

show_line_number=IntVar()
show_line_number.set(1)
show_cursor_info=IntVar()
show_cursor_info.set(1)

#-------------------------------FONTS-MENU-------------------------------------------------------------------
fontMenu = Menu(menu) 
menu.add_cascade(label="Fonts",menu=fontMenu)
helvetica=IntVar() 
courier=IntVar()
fontMenu.add_checkbutton(label="Courier",command=FontCourier)
fontMenu.add_checkbutton(label="Helvetica",command=FontHelvetica)    
fontMenu.add_checkbutton(label="Times New Roman", command=FontTimes)  
fontMenu.add_checkbutton(label="Arial", command=FontArial)  
fontMenu.add_checkbutton(label="Chiller", command=FontChiller)  

#------------------------------THEME-MENU---------------------------------------------------------------------
themeMenu = Menu(menu)
menu.add_cascade(label="Themes",menu=themeMenu)

#-------------------------------HELP-MENU---------------------------------------------------------------------
helpMenu = Menu(menu)
menu.add_cascade(label="Help",menu=helpMenu)
helpMenu.add_command(label = "About",accelerator='About',underline=0,command=about_,image=about_icon)

#------------------------------LINES & THEMES-----------------------------------------------------------------
line_number_bar = Text(root, width=4, padx=3, takefocus=0, fg='white', border=0, background='#282828', state='disabled',  wrap='none')
line_number_bar.pack(side='left', fill='y')


''' THEMES OPTIONS'''

color_schemes = {
    'Default': '#000000.#FFFFFF',
    'Greygarious': '#83406A.#D1D4D1',
    'Aquamarine': '#5B8340.#D1E7E0',
    'Bold Beige': '#4B4620.#FFF0E1',
    'Cobalt Blue': '#ffffBB.#3333aa',
    'Olive Green': '#D1E7E0.#5B8340',
    'Night Mode': '#FFFFFF.#000000',
}
    
theme_choice=StringVar()
theme_choice.set('Default')
for k in sorted(color_schemes):
    themeMenu.add_radiobutton(label=k, variable=theme_choice, command=change_theme)

content_text = Text(root, wrap='word')
content_text.pack(expand='yes', fill='both')

scroll_bar = Scrollbar(content_text)
content_text.configure(yscrollcommand=scroll_bar.set)
scroll_bar.config(command=content_text.yview)
scroll_bar.pack(side='right', fill='y')

cursor_info_bar = Label(content_text, text='Line: 1 | Column: 1')
cursor_info_bar.pack(expand='no', fill=None, side='right', anchor='se')


content_text.bind('<Control-Y>',redo_)
content_text.bind('<Control-y>',redo_)
content_text.bind('<Control-F>',find_)
content_text.bind('<Control-f>',find_)

content_text.bind('<Any-KeyPress>', on_content_changed)
content_text.tag_configure('active_line', background='ivory2')

#---------------------------CLOSE-THE-PROGRAM----------------------------------------------------------------
root.mainloop()
