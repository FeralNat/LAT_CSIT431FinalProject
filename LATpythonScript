import tkinter as tk
from tkinter import IntVar
import subprocess
from fabric import Connection, Config
TitleFont=('Times',18,'bold','underline')
HeadingFont=('Times',14,'bold')

configpass=str("")
def addIp():
  """fn to add ip address to the ip list. shares ssh public key """
  newip = str(en_ip.get())
  ipstr=str(en_usr.get()) + "@" + newip
  ippass=str(en_passwd.get())
  global configpass 
  configpass = ippass
  newipno=newip.replace('.','')
  if newipno.isnumeric(): 
    command = "ssh-copy-id "+ipstr
    """config=Config(overrides={'sudo':{'password': configpass}})"""
    subprocess.call(command, shell=True)
    lbox_iplist.insert("end",ipstr)
    en_ip.delete(0,"end")
    en_usr.delete(0,"end")
    en_passwd.delete(0,"end")
  else:
    print("Please enter a numeric value")

def clearSel():
  """fn to clear ip list"""
  sel = lbox_iplist.curselection()
  if not sel:
    print("No Ip selected to delete")
  else:
    lbox_iplist.delete(sel)

def showUsers():
  global configpass
  lbox_ipusers.select_clear
  command='getent passwd {1000..2000}'
  sel = lbox_iplist.get(lbox_iplist.curselection())
  config=Config(overrides={'sudo':{'password': configpass}})
  with Connection(sel, config=config) as c:
    outstring = c.run(command, warn=True).stdout
    outlist = []
    outlist = outstring.split("\n")
    for line in outlist:
      lbox_ipusers.insert("end", line.split(":")[0])
    """  
    c_stdout = c.stdout.split("\n")
    for line in c_stdout:
      lbox_ipusers.insert("end", line)"""

def addUser():
  global configpass
  newUser= str(en_newusr.get())
  command = 'useradd ' + newUser
  sel = lbox_iplist.get(lbox_iplist.curselection())
  config=Config(overrides={'sudo':{'password': configpass}})
  with Connection(sel, config=config) as c:
    c.sudo(command)

def deleteUser():
  global configpass
  usr2del = lbox_ipusers.get(lbox_ipusers.curselection())
  command = 'userdel -r ' + usr2del
  sel = lbox_iplist.get(lbox_iplist.curselection())
  config=Config(overrides={'sudo':{'password': configpass}})
  c = Connection(sel, config=config)
  c.sudo(command)

def show_pass():
  if(cb_v1.get()==1):
    en_passwd.config(show='')
  else:
    en_passwd.config(show='*')

window = tk.Tk()
window.title("LAT")
window.resizable(width=False, height= False)

window.rowconfigure(7,minsize=10,weight=1)
window.columnconfigure(5,minsize=10,weight=1)

frm_entry = tk.Frame(master=window)
lb_title = tk.Label(text="Linux Admin Tool", font=TitleFont)
lb_ip = tk.Label(text="Enter IP:")
en_ip = tk.Entry()
lb_usr = tk.Label(text="Enter Username:")
en_usr = tk.Entry()
lb_passwd = tk.Label(text="Enter Password:")
en_passwd = tk.Entry(show='*')
cb_v1=IntVar(value =0)
cb_showpass = tk.Checkbutton(text='Show Password',variable=cb_v1, onvalue=1, offvalue=0, command=show_pass)
btn_addip = tk.Button(text="Add IP", command=addIp)
lb_ipbox = tk.Label(text="IP List:", font=HeadingFont)
lbox_iplist = tk.Listbox(window,height=10)
btn_clear = tk.Button(text="Clear Selected", command=clearSel)
btn_connect = tk.Button(text="Selected IP Users", command= showUsers)
lb_newusr = tk.Label(text="Enter new Username: \n for selected IP")
en_newusr = tk.Entry()
btn_addnu = tk.Button(text="Add User", command= addUser)
btn_delusr = tk.Button(text="Delete User", command= deleteUser)
lbox_ipusers = tk.Listbox(window,height=10)
btn_chgpass = tk.Button(text="Change Password")
btn_lockunlockusr = tk.Button(text="Lock/Unlock User")
btn_usrpermission = tk.Button(text="Give User Sudo")
lb_usrlist = tk.Label(text="User List:", font=HeadingFont)

lb_title.grid(row=0,columnspan=2,sticky="ew",padx=5,pady=5)
lb_ip.grid(row=1,column=0,sticky="ew",padx=5,pady=5)
en_ip.grid(row=1,column=1,sticky="ew",padx=5,pady=5)
lb_usr.grid(row=2,column=0,sticky="ew",padx=5,pady=5)
en_usr.grid(row=2,column=1,sticky="ew",padx=5,pady=5)
lb_passwd.grid(row=3,column=0,sticky="ew",padx=5,pady=5)
en_passwd.grid(row=3,column=1,sticky="ew",padx=5,pady=5)
cb_showpass.grid(row=3,column=2,sticky="ew",padx=5,pady=5)
btn_addip.grid(row=4,column=1,sticky="ew",padx=5,pady=5)
lb_ipbox.grid(row=4,column=0,sticky="ew",padx=5,pady=5)
lbox_iplist.grid(row=5,columnspan=2,sticky="nsew",padx=5,pady=5)
btn_clear.grid(row=6,column=0,sticky="ew",padx=5,pady=5)
btn_connect.grid(row=6,column=1,sticky="ew",padx=5,pady=5)
lb_newusr.grid(row=2,column=3,sticky="ew",padx=5,pady=5)
en_newusr.grid(row=3,column=3,sticky="ew",padx=5,pady=5)
btn_addnu.grid(row=4,column=3,sticky="ew",padx=5,pady=5)
btn_delusr.grid(row=5,column=3,sticky="sew",padx=5,pady=5)
lbox_ipusers.grid(row=5,column=2,sticky="nsew",padx=5,pady=5)
lb_usrlist.grid(row=4,column=2,sticky="ew",padx=5,pady=5)
btn_chgpass.grid(row=5,column=3,sticky="new",padx=5,pady=5)
btn_lockunlockusr.grid(row=5,column=3,sticky="ew",padx=5,pady=5)
btn_usrpermission.grid(row=6,column=3,sticky="ew",padx=5,pady=5)

window.mainloop()
