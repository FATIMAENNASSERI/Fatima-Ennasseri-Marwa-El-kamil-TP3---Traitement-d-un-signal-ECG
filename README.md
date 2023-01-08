# Fatima-Ennasseri-Marwa-El-kamil-TP3---Traitement-d-un-signal-ECG
## Introduction 
Un électrocardiogramme (ECG) est une représentation graphique de l’activation électrique du cœur à l’aide d’un électrocardiographe. Cette activité est recueillie sur un patient allongé, au repos, par des électrodes posées à la surface de la peau. 
L’analyse du signal ECG est utile dans le but de diagnostiquer des anomalies 
cardiaques telles qu’une arythmie, un risque d’infarctus, de maladie cardiaque 
cardiovasculaire ou encore extracardiaque. 
Ci-dessous, voici un schéma représentant une représentation classique d’une courbe 
d’un ECG. Ce schéma se nomme un « Complexe QRS » mettant en évidence le bon 
fonctionnement d’un cycle cardiaque.

## Suppression du bruit provoqué par les mouvements du corps :

On Sauvegarder le signal ECG sur la répertoire de travail, puis on le charge dans 
Matlab à l’aide la commande load.

 Ce signal a été échantillonné avec une fréquence de 50Hz. 
 
## script :

  clear all
  
  close all

  clc

  load 'ecg.mat';

  fe=500;

  te=1/fe;

  N=length(ecg);

  t=0:te:(N-1)*te;

  fshift= (-N/2:N/2-1)*(fe/N); 

  plot(t,ecg)

![1](https://user-images.githubusercontent.com/120643516/210903526-d0f43674-09e7-4e39-8361-83b64625e6aa.png)

#### Pour supprimer les bruits à très basse fréquence dues aux mouvements du corps,on utilisera un filtre idéal passe-haut. Pour ce faire, calculer tout d’abord la TFD du signal ECG, régler les fréquences inférieures à 0.5Hz à zéro, puis effectuer une TFDI pour restituer le signal filtré.




























