# Fatima-Ennasseri-Marwa-El-Kamil-TP3---Traitement-d-un-signal-ECG
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

#### Pour supprimer les bruits à très basse fréquence dues aux mouvements du corps,on utilisera un filtre idéal passe-haut. 


1-Pour ce faire, calculer tout d’abord la TFD du signal ECG:
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

spectre_x=fft(ecg);

plot(fshift,fftshift(abs(spectre_x)));


![3](https://user-images.githubusercontent.com/120643516/211198007-0ba0aad7-a855-47c6-b2cc-de794b87c2c2.png)


2- régler les fréquences inférieures à 0.5Hz à zéro, puis effectuer une TFDI pour restituer le signal filtré.
pass_haut_ideal = ones(size(ecg));

fc = 0.5; 

indexe_fc = ceil((fc*N)/fe); %% ceil(A) arrondit les éléments de A aux entiers les plus proches supérieurs ou égaux à A. Pour le complexe A, les parties imaginaire et réelle sont arrondies indépendamment.

pass_haut_ideal(1:indexe_fc)=0;

pass_haut_ideal(N-indexe_fc+1:N)=0;

f=(0:N-1)*(fe/N);

spectre_ecg_filtree = pass_haut_ideal .* spectre_ecg;

tmp_ecg_filre = ifft(spectre_ecg_filtree,'symmetric');

plot(t,tmp_ecg_filre);

![3-2](https://user-images.githubusercontent.com/120643516/211198720-f2b0e143-f9f8-416b-92de-d5db2219cce4.png)

## Suppression des interférences des lignes électriques 50Hz

### ECG est contaminé par un bruit du secteur 50 Hz qui doit être supprimé.
Les filtres Notch sont des filtres de blocage de bande. Ils sont utilisés pour supprimer les faisceaux laser puissants en spectroscopie laser Raman et dans d'autres applications. Ils peuvent également être utilisés pour protéger la vue ou servir de filtres amovibles pour les caméras.

Ce filtre permet de supprimer la composante secteur EDF tout en préservant le contenu fréquentiel des signaux de mesures (ECG, SPO2, etc..).

#### On applique  un filtre Notch idéal pour supprimer cette composante. Les filtres Notch sont utilisés pour rejeter une seule fréquence d'une bande de fréquence donnée.

## script :

% Elimination bruit 50hz

pass_notch_ideal = ones(size(ecg));%%  returns an array of 1s that is the same size as ecg.

fc = 50; 

indexe_fc = ceil((fc*N)/fe)+1;

pass_notch_ideal(indexe_fc)=0;

pass_notch_ideal(N-indexe_fc+1)=0;

spectre_ecg2_filtree = pass_notch_ideal .* fft(tmp_ecg_filre);

tmp_ecg2_filre = ifft(spectre_ecg2_filtree,'symmetric');

subplot(311)

plot(t,ecg);

subplot(312)
plot(t,tmp_ecg2_filre);

subplot(313)
plot(t,ecg-tmp_ecg2_filre);


![5-6](https://user-images.githubusercontent.com/120643516/211199635-ff5662e0-64e4-42b2-a5c4-3751dce02651.png)

## Amélioration du rapport signal sur bruit :

Le signal ECG est également atteint par des parasites en provenance de l’activité musculaire extracardiaque du patient. La quantité de bruit est proportionnelle à la

largeur de bande du signal ECG. Une bande passante élevée donnera plus de bruit dans les signaux, et limiter la bande passante peut enlever des détails importants du 

signal. 

Après avoir appliqué les 3 filtres on'a reussi a troouvé le signal suivant :

![Capture d’écran 2023-01-27 124500](https://user-images.githubusercontent.com/120643516/215080150-56c1af14-f098-4632-9cc3-0db08f7f6fa7.png)

on utilisons le spectre suivant : 







![elimini](https://user-images.githubusercontent.com/120643516/215080258-6ea7d8b3-5025-455e-b6bf-3c2b32f28af6.png)

explication de la fonction ceil :""B = ceil(A) rounds the elements of A to the nearest integers greater than or equal to A. For complex A, the imaginary and real parts are rounded independently""

## Identification de la fréquence cardiaque avec la fonction d’autocorrélation

La fréquence cardiaque peut être identifiée à partir de la fonction d'autocorrélation du signal ECG. à tau =0, en cherchant le premier maximum local après le maximum global  de cette fonction. 

on va Ecrire un programme permettant de calculer l’autocorrélation du signal ECG, puis de chercher cette fréquence cardiaque de façon automatique. Utiliser ce programme sur le signal traité ecg3 ou ecg2 et sur le signal ECG non traité

 ![Capture d’écran 2023-01-27 124843](https://user-images.githubusercontent.com/120643516/215081007-a010374a-fd36-4cea-80d0-f43835d876fb.png)
 
 Grâce à  la commande xcorr on a pu detecter une correclation dans les maximum a tau égale à 2;
 



 ![auto](https://user-images.githubusercontent.com/120643516/215081656-d4ba0c5c-9b22-4859-a12b-3038ac9a91f9.png)










