from tkinter import *
import os
from tkinter import messagebox
from random import *
import pandas as pd


interface_de_connexion = Tk()
interface_de_connexion.geometry("452x230")
interface_de_connexion.title(string = "GesMag")
interface_de_connexion.configure(bg = '#FFFFFF')


label_connectez_vous = Label(interface_de_connexion, text = "Login", font = ("Arial", 15, 'bold'), fg = 'white', bg = '#375D81', width = 10, height = 1, bd = 3, relief = 'groove')
label_connectez_vous.grid(row = 0, columnspan = 2, pady = 10)


identifiant = StringVar()
label_pour_indentifiant = Label(interface_de_connexion, text = "Identifiant :", font = ("Arial", 12, 'bold'), bg = 'white')
label_pour_indentifiant.grid(row = 1, column = 0, sticky = W, padx = 35, pady = 10)
zone_de_saisi_pour_identifiant = Entry(interface_de_connexion, textvariable = identifiant, font = ("Arial", 12), bg = '#e3e3e3',width = 20, bd = 3)
zone_de_saisi_pour_identifiant.grid(row = 1, column = 1, sticky = E, padx = 35, pady = 10)
zone_de_saisi_pour_identifiant.focus()


mot_de_passe = StringVar()
label_pour_le_mot_de_passe = Label(interface_de_connexion, text = "Mot de passe :", font = ("Arial", 12, 'bold'), bg = 'white')
label_pour_le_mot_de_passe.grid(row = 2, column = 0, sticky = W, padx = 35, pady = 10)
zone_de_saisi_pour_mot_de_passe = Entry(interface_de_connexion, textvariable = mot_de_passe, font = ("Arial", 12), bg = '#e3e3e3', width = 20, bd = 3, show = '*')
zone_de_saisi_pour_mot_de_passe.grid(row = 2, column = 1, sticky = E, padx = 35, pady = 10)


df = pd.read_excel('base_de_données_caissiers_et_managers.xlsx')


def verification_de_connexion():
    mask = df["Identifiant"].values
    if zone_de_saisi_pour_identifiant.get() in mask:
        if zone_de_saisi_pour_identifiant.get().startswith('C'):
            mask2 = df.loc[df["Identifiant"] == zone_de_saisi_pour_identifiant.get(), "Mot de passe"].values[0]
            if zone_de_saisi_pour_mot_de_passe.get() == mask2:
                messagebox.showinfo(message = "Vous êtes connecté en tant que caissier")
                zone_de_saisi_pour_identifiant.delete(0, END)
                zone_de_saisi_pour_mot_de_passe.delete(0, END)
                zone_de_saisi_pour_identifiant.focus()
            else:
                messagebox.showinfo(message = "Mot de passe incorrecte")
                zone_de_saisi_pour_mot_de_passe.delete(0, END)
                zone_de_saisi_pour_mot_de_passe.focus()
        elif identifiant.get().startswith('M'):
            mask2 = df.loc[df["Identifiant"] == zone_de_saisi_pour_identifiant.get(), "Mot de passe"].values[0]
            if zone_de_saisi_pour_mot_de_passe.get() == mask2:
                messagebox.showinfo(message = "Manager")
                zone_de_saisi_pour_identifiant.delete(0, END)
                zone_de_saisi_pour_mot_de_passe.delete(0, END)
                zone_de_saisi_pour_identifiant.focus()
            else: 
                messagebox.showerror(message = "Mot de passe incorrecte")
                zone_de_saisi_pour_mot_de_passe.delete(0, END)
                zone_de_saisi_pour_mot_de_passe.focus()
    else:
        messagebox.showerror(message = "L'identifiant n'est pas bon!")
        zone_de_saisi_pour_identifiant.delete(0, END)
        zone_de_saisi_pour_mot_de_passe.delete(0, END)
        zone_de_saisi_pour_identifiant.focus()
       

bouton_pour_connexion = Button(interface_de_connexion, text = "Connexion", font = ('Arial', 12, 'bold'), width = 10, bd = 3, bg = 'white', fg = '#6FC771', activebackground = '#6FC771', activeforeground = 'white', cursor = 'hand2', command = verification_de_connexion)
bouton_pour_connexion.grid(row = 3, column = 1, sticky = E, padx = 35, pady = 30)


bouton_pour_quitter = Button(interface_de_connexion, text = "Quitter", font = ('Arial', 12, 'bold'), width = 10, bd = 3, bg = 'white', fg = '#F32665', activebackground = '#F32665', activeforeground = 'white', cursor = 'hand2', command = quit)
bouton_pour_quitter.grid(row = 3, column = 0, sticky = W, padx = 35, pady = 30)


interface_de_connexion.mainloop()
