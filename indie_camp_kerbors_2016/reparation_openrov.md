---
title: Réparation d'un drône sous-marin (OpenROV)
description: Documentation pour réparer un OpenROV, issue du site de François (hack-your-brain.com) en explications et images.
image_url: https://github.com/multibao/contributions/blob/master/media/ROV-complet-face-meta.jpg?raw=true
---

![](https://framapic.org/0otFGnFrkbfa/IPdydbDnXxIX)

# Réparation d'un OpenROV

Article originellement publié sur www.hack-your-brain.com, je le republie ici pour partager l'expérience aux autres participants du SummerCamp et toute autre personne qui pourrait être intéressée, Fanrçois Dupayrat.

_There is a summary in English at the end in case you just want to repair your ROV and be on your way._

Face avant du ROV
![](https://framapic.org/0otFGnFrkbfa/IPdydbDnXxIX)

Actuellement au SummerCamp en Bretagne, Manu a ramené un [OpenROV][2] qui ne marchait plus, à réparer.
C’était pas gagné, des Makers de Pontivy c’étaient déjà penché dessus pendant 2 jours sans arriver à trouver la cause de la panne. ( [Historique des Tests decet Open ROV](https://explore.hackpad.com/OceanisOpen-Tests-et-amlioration-de-lOpenROV-Explore-p4Y4dhu0uVt) )

L’OpenROV c’est quoi ? C’est un robot d’exploration sous-marin open-source à bas coût.

## Les symptômes de la panne 

Les LED avant marchent, il y a des LEDs qui clignotent sur les cartes, donc l'alimentation marche, mais impossible d'avoir un lien avec le ROV. 

Premier enjeu de taille : je n'y connais absolument rien à comment est fabriqué le ROV, et étant donné qu'il va sous l'eau, il est étanche, et on est un peu réticent à l'idée de l'ouvrir et casser l'étanchéité. 

## Suspect n°1 : les piles 

Batteries du moteur
![](https://framapic.org/Az5BkhvgmjLY/ve46gDYl6jv6)

En demandant, elles étaient chargées, et de toute façon ça ne sert à priori qu'à alimenter les moteurs, pas les cartes (dont l'alimentation passe par le cable relié au PC), donc ça ne peut pas être la cause de la panne. 

## Ouverture du ROV 

ROV ouvert
![](https://framapic.org/vZmQwkaEha7t/C79D0ndLQaSp)

Donc on a ouvert le corps de l'OpenROV. Là, je vois une BeagleBone, qui a l'air d'être alimentée par les PINs, et est reliée à un Ethernet)

BeagleBone
![](https://framapic.org/Dfwd5bXLDmxn/4WNcFCrmmcKQ)

Ni une ni deux, on enlève la BeagleBone, on la branche direct en Ethernet (au lieu de passer par l'Ethernet du ROV), avec une alimentation USB. Et là, première bonne surprise : l'interface charge, ce qui montre que le "cerveau" du ROV est fonctionnel". 

Ça limite donc la liste des suspects à 2 choses : 
* les câbles qui font transiter les données 
* le matériel qui fait transiter les données 

En inspectant plus avant, les données passent par le câble d'alimentation entre le ROV et le boitier sur lequel on branche le PC : il s'agit de courant porteur, qui est fait par 2 petits modules verts identiques (un dans le ROV, un dans le boitier), les Tenda Homeplug. 

Là petit problème : sans oscilloscope, on ne peut pas vraiment observer le courant porteur. 

## Le désespoir 

topside interface board
![](https://framapic.org/WakVjXMszaYr/kFXAzPvxMri0)

A ce moment là, petit désespoir mêlé de joie : on est sûr à 95% que ça viens de ces 2 modules Tenda (l'Ethernet n'a pas l'air abimé, et le reste des câbles marchent vu que l'alimentation passe). Ca veut dire que c'est facile à réparer, mais sans les pièces sous les mains, impossible de le remettre en marche aujourd'hui. 

Les LEDs du boitier confirment nos doutes : la LED Power est allumée quand on branche l'USB, la LED Ethernet Connect est allumée quand on branche l'Ethernet, mais la LED HomePlug Connect ne s'allume jamais. 

## Le regain d'espoir 

Tenda HomePLug
![](https://framapic.org/bjZR0Omq45gQ/oyn60H41YwRI)
bouton tenda ROV
![](https://framapic.org/fDdmyaUyZbj5/mMFHgQudxfdK)

Après quelques recherches, il se trouve que les 2 modules Tenda doivent être appairés. Autant tenter le coup : on appuie pendant une seconde sur le bouton des 2 modules à la fois, avec le BeagleBone débranché. Et là, la joie : la LED HomePlug Connect s'allume. 

On rebranche le BeagleBone, la caméra qu'on avait oublié au passage, et tout remarche correctement ! 

Summary: the problem comes from the 2 Tenda Homeplug adapters (small green cards which have an Ethernet plug), which encounter a pairing problem. You can check you have the same problem if, on the OpenROV Topside Interface Board (where you plug your USB), both Power and Ethernet Connect LEDs are lit, but the HomePlug Connect LED is not. 

To solve the problem, you need to disassemble the OpenRov tube to reach the other Tenda adapter. Remove the BeagleBone (the black card), press the button on both Tenda adapter 1 sec at the same time. Now check your OpenROV Topside Interface Board : the HomePlug Connect LED should be lit. It it isn't, retry pairing. 

When it's done, put the BeagleBone back on, test that everything works correctly, then re-assemble your OpenROV.


* http://www.hack-your-brain.com/reparation-dun-openrov/

## Video de contexte de travail

<iframe width="560" height="315" src="https://www.youtube.com/embed/V_wXdLakOAg" frameborder="0" allowfullscreen></iframe>
