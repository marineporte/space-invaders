# space-invaders

ce projet à pour objectif de creer le jeu space invaders

les commandes qui vont etre utiliser sont:
-pour tirer: la barre d'espace
-diriger le canon: les fleches
-faire pause: lettre p

les imports qui vont nous etre utiles sont les suivants:
from random import *


la premiere fonction que je vais coder est celle ui va servir à afficher l'écran de présentation du jeu, avec un 
def EcranDePresentation():
pour cela j'ai utiliser 'global DebutJeu' et j'ai defini la variable 'DebutJeu = 0'

j'ai egalement commencé à créé un bouton 'commencer' et un bouton 'quitter'



exemple de programme space invaders
from __future__ import division
from tkinter import *
from random import randrange
from time import sleep
 
pos_souris=[0,0]
haut=500;large=600
ca='dark green'
cb='gray'

#on cree l'ecran de presentation

def EcranDePresentation():
    global DebutJeu
    if DebutJeu!=1:
        AffichageScore.configure(text="",font=('Fixedsys',16))
        AffichageVie.configure(text="",font=('Fixedsys',16))
        can.delete(ALL)
        fen.after(1500,Titre)

 
 
#On crée une variable de centre autour de laquelle on créera les ennemis.
 
def option():
    global Tv
    u=Tv.get()
    print ('essai=',u)
 
 
    MODES = [
        ("Monochrome", "1"),
        ("Grayscale", "L"),
        ("True color", "RGB"),
        ("Color separation", "CMYK"),
    ]
    Tv = StringVar(racine,value="1")
        #On peut changer le '1' pour tenir compte du cntexte
    for t, mode in MODES:
     b = Radiobutton(racine, text=t,variable=Tv,
            value=mode,indicatoron=1,command=change)
     b.pack(anchor=W)
 
#Coordonnées du centre de tout les monstres.
f=40
g=15
 
fen = Tk()
fond=Canvas(fen, width=large, height=haut, background='darkgray')
fond.pack()
 
bouton = Button(fen, text='Options', command=option)
bouton.pack()
 
 
 
 
#Pour rajouter une ligne il faut incrémenter y de 20
 
def createrectangle(u,v,a,b,c1,c2,num):
    fond.create_rectangle(f-5,g+5,f+5,g-5,fill=c1,outline=c2,tags='k'+str(num))
    #                     l1   h2    l2    h1
    fond.create_rectangle(f+5,g+8,f+8,g-2,fill=c1,outline=c2,tags='k'+str(num))
    fond.create_rectangle(f-8,g+8,f-5,g-2,fill=c1,outline=c2,tags='k'+str(num))
 
 
#Ennemis
 
 
n = 27
for k in range (n):
#Ici je crée une liste P qui va regrouper toutes les coordonnées du centre des ennemis
    P=[]
    P, fond
    createrectangle(f+320,g+5,f+330,g-5,ca,cb,k)
    f=f+20
    P.append(k)
 
c=0 #Largeur 2
d=0 #Largeur 1
e=0 #Largeur sommet polygone
 
#VAISSEAU DU HEROS
 
def cree_vaisseau():
    global pos_souris, T, fond
    T=[]
#                         l1  h2  l2  h1
    T.append(fond.create_rectangle(d+185,280,c+190,260,fill='gray',outline='black',tags='v'))#Tireur gauche
    T.append(fond.create_rectangle(d+210,280,c+215,260,fill='gray',outline='black',tags='v'))#Tireur droite
#                       xbas ybas xsommet ysommet xbasdroite y basdroite
    T.append(fond.create_polygon(d+185,280, e+200, 250, c+215, 280, fill="black", outline="gray",tags='v'))
#                    l1  h2  l2  h1
    T.append(fond.create_oval(d+185,286,c+193,278,fill='yellow', outline="red",tags='v'))#Propulseur gauche
    T.append(fond.create_oval(d+207,286,c+215,278,fill='yellow', outline="red",tags='v'))#Propulseur droit
    T.append(fond.create_oval(d+192,292,c+208,278,fill='yellow', outline="red",tags='v'))#Effet vitesse
    pos_souris=[200,270]
 
def tir():
    global fond,pos_souris
    u=pos_souris[0]
    v=pos_souris[1]
#               coord bas du tir/ coord haut du tir
#               u = gauche/droite
#J'ai un peu changé les coordonneés pour qu'elles soit bien sur les canons.
    fond.create_line(u+13,v+3,u+13,v-10,fill='red',width=2,tags='d')#Tir gauche
    fond.create_line(u-13,v+3,u-13,v-10, fill='red',width=2,tags='d')#Tir droite
    fen.update()
    while v>0: #la condition s'arrêtent quand les tirs sortent de la fenetre
#les coordonnées du haut et bas des tirs sont réduite de 10 PIXELS
#en réduisant la somme on peut modifier la vitesse des tirs
        v=v-10
        fond.move('d',0,-10)
        fen.update()#fonction qui permet de rafraichir la fenetre
        sleep(0.03)#donne le temps de rafraichissement de la fenêtre
        #en la réduisant on augmente la fluidité du programme et vice/versa
    fond.delete('d')
 
def bouge(event):
    global fond, pos_souris
    #assigne les coordonnées à la position de la souris
    u=event.x-pos_souris[0]
    v=event.y-pos_souris[1]
    fond.move('v',u,v)
    pos_souris=[event.x,event.y]
 
def clic(event):
    global fond, pos_souris
    bouge(event)
    tir()
    test()
 
def test():
    for k in range (n) :
        abs((u+13)-P[k])<3
        fond.delete('k'+str(k))
        P[k]=-100
 
 
cree_vaisseau()
#fonction qui associe le clic gauche à l'évenement clic
fond.bind('<ButtonPress-1>',clic)
#fonction qui associe le mouvement à l'évenement mouve
fond.bind('<Motion>',bouge)
fen.mainloop()



