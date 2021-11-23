# -*- coding: utf-8 -*-
"""
Created on Tue Apr 13 17:42:51 2021

@author: quent
"""

import numpy as np
import matplotlib.pyplot as plt
import random as rnd
import pandas as pd

data = []
preTest = []
finalTest = []

with open("data.csv","r") as file:
    for i in file:
        data.append(i.split(","))
#rnd.shuffle(data)

with open("preTest.csv","r") as file1:
    for i in file1:
        preTest.append(i.split(","))
#rnd.shuffle(preTest)

with open("finalTest.csv","r") as file2:
    for i in file2:
        finalTest.append(i.split(","))
#rnd.shuffle(finalTest)




def separe(liste, pourcentageTest = 0.7):
    try:
        pourcentageTest>=0 and pourcentageTest<=1
    except :
        raise ValueError("Pourcentage de test doit être compris entre 0 et 1")
    #rnd.shuffle(liste)
    nbrTest = round(len(liste)*pourcentageTest)
    test = liste[0:nbrTest]
    verif = liste[nbrTest+1:len(liste)]
    return test,verif
aaa = separe(data,0.01)
#print(aaa[0])
"""
Prend une liste et un pourcentage(individus alloués au test) en paramètre et retourne un tuple 
dont le premier elem est l'échnatillon de test et le deuxième l'échantillon de vérification
"""



def affiche(data):
    compteur=0
    for i in data:
        compteur+=1
        print("||{}| {:.3f} | {:.3f} | {:.3f} | {:.3f} | {:.3f} | {:.3f} | {} ".format(compteur,float(i[0]),float(i[1]),float(i[2]),float(i[3]),float(i[4]),float(i[5]),i[6]))
    #affiche(aaa[0])
"""
Prend une liste de liste de 6 elem et l'affiche proprement avec 3 c.s
"""

def distance(p1, p2):
    return ((float(p1[0])-float(p2[0]))**2+(float(p1[1])-float(p2[1]))**2+(float(p1[2])-float(p2[2]))**2+(float(p1[3])-float(p2[3]))**2+(float(p1[4])-float(p2[4]))**2+(float(p1[5])-float(p2[5]))**2)**0.5
p1 = data[0]
p2 = data[1]
test = [p1,p2]
#affiche(test)
#print(distance(p1,p2))
"""
Prend 2 points en entrée et retourne la distance euclidienne
"""

def distanceTout(p1,test):
    data2 = []
    for i in test:
        data2.append([distance(p1,i),i[6]])
        data2.sort(key = lambda x: x[0] )
    return data2
bbb = distanceTout(p1,aaa[0])
#print(bbb)
"""
Prend un point et les données de test en paramètre et retourne une liste de liste
triée selon la distance contenant la distance entre p1 et chaque pts de test 
ainsi que la classe des points de test
!!! Le premier element de data2 est la classe de p1
"""

def maxClasse(data3,k):
    data2 = data3[1:]
    test = [0,0,0,0,0]
    compteur = 0
    for i in range(k):
        if (data2[i][1]=="classA\n"):
            test[0]+=1
        elif (data2[i][1]=="classB\n"):
            test[1]+=1
        elif (data2[i][1]=="classC\n"):
            test[2]+=1
        elif (data2[i][1]=="classD\n"):
            test[3]+=1
        elif (data2[i][1]=="classE\n"):
            test[4]+=1
        compteur+=1
    classIndex = test.index(max(test))
    classe = ""
    if (classIndex == 0):
        classe = "classA"
    elif (classIndex == 1):
        classe = "classB"
    elif (classIndex == 2):
        classe = "classC"
    elif (classIndex == 3):
        classe = "classD"
    elif (classIndex == 4):
        classe = "classE"
    #print(test)
    #print(max(test))
    #print(test.index(max(test)))
    #print(classe)
    return classe
ccc = maxClasse(bbb,2)
#print(ccc)
"""
Prend en entrée la liste des distance/classes associées à 1 point ainsi qu'un 
paramètre k correspondant au nombre de voisins que l'on prend en compte
Cela retourne la classe du point dont la liste découle
"""

def normalise(data):
    finalData = []
    var1 = []
    var2 = []
    var3 = []
    var4 = []
    var5 = []
    var6 = []
    new1 = []
    new2 = []
    new3 = []
    new4 = []
    new5 = []
    new6 = []
    for i in data:
        var1.append(float(i[0]))
        var2.append(float(i[1]))
        var3.append(float(i[2]))
        var4.append(float(i[3]))
        var5.append(float(i[4]))
        var6.append(float(i[5]))
    maxVar1 = float(max(var1))
    minVar1 = float(min(var1))
    maxVar2 = float(max(var2))
    minVar2 = float(min(var2))
    maxVar3 = float(max(var3))
    minVar3 = float(min(var3))
    maxVar4 = float(max(var4))
    minVar4 = float(min(var4))
    maxVar5 = float(max(var5))
    minVar5 = float(min(var5))
    maxVar6 = float(max(var6))
    minVar6 = float(min(var6))
    for i in var1:
        new1.append((i-minVar1)/(maxVar1-minVar1))
    for i in var2:
        new2.append((i-minVar2)/(maxVar2-minVar2))
    for i in var3:
        new3.append((i-minVar3)/(maxVar3-minVar3))
    for i in var4:
        new4.append((i-minVar4)/(maxVar4-minVar4))
    for i in var5:
        new5.append((i-minVar5)/(maxVar5-minVar5))
    for i in var6:
        new6.append((i-minVar6)/(maxVar6-minVar6))
    for i in range(len(var1)):
        finalData.append([new1[i],new2[i],new3[i],new4[i],new5[i],new6[i],data[i][6]])
    return finalData


def normalise2(data):
    finalData = []
    var1 = []
    var2 = []
    var3 = []
    var4 = []
    var5 = []
    var6 = []
    new1 = []
    new2 = []
    new3 = []
    new4 = []
    new5 = []
    new6 = []
    for i in data:
        var1.append(float(i[0]))
        var2.append(float(i[1]))
        var3.append(float(i[2]))
        var4.append(float(i[3]))
        var5.append(float(i[4]))
        var6.append(float(i[5]))
    maxVar1 = float(max(var1))
    minVar1 = float(min(var1))
    maxVar2 = float(max(var2))
    minVar2 = float(min(var2))
    maxVar3 = float(max(var3))
    minVar3 = float(min(var3))
    maxVar4 = float(max(var4))
    minVar4 = float(min(var4))
    maxVar5 = float(max(var5))
    minVar5 = float(min(var5))
    maxVar6 = float(max(var6))
    minVar6 = float(min(var6))
    for i in var1:
        new1.append((i-minVar1)/(maxVar1-minVar1))
    for i in var2:
        new2.append((i-minVar2)/(maxVar2-minVar2))
    for i in var3:
        new3.append((i-minVar3)/(maxVar3-minVar3))
    for i in var4:
        new4.append((i-minVar4)/(maxVar4-minVar4))
    for i in var5:
        new5.append((i-minVar5)/(maxVar5-minVar5))
    for i in var6:
        new6.append((i-minVar6)/(maxVar6-minVar6))
    for i in range(len(var1)):
        finalData.append([new1[i],new2[i],new3[i],new4[i],new5[i],new6[i]])
    return finalData


def testTable(data, pourcentageTest,k):
    tupple = separe(data,pourcentageTest)
    memory = tupple[0]
    verif = tupple[1]
    testTable = []
    for i in verif:
        testTable.append([maxClasse(distanceTout(i,memory),k)+"\n",i[6]])
        #print(i)
    return testTable
#ddd = testTable(data,0.8,8)
#print(ddd)
"""
Prend en entrée un dataset un pourcentage et un entier k
split le dataset en 2 et fait un test du knn sur un des 2 set crées
retourne un liste de liste avec la classe estimée et la classe réelle
"""


def testTable2(memory, verif,k):
    testTable2 = []
    for i in verif:
        testTable2.append([maxClasse(distanceTout(i,memory),k)+"\n",i[6]])
        #print(i)
    return testTable2
#ddd2 = testTable(data,0.8,8)
#print(ddd)
"""
Prend en entrée un dataset de mémoire et un dataset de validation et un entier k
fait un test du knn sur le dataset de validation avec celui de mémoire
retourne un liste de liste avec la classe estimée et la classe réelle
"""


def finalTable(memory, verif,k):
    finalTable = []
    for i in verif:
        finalTable.append(maxClasse(distanceTout(i,memory),k))
        #print(i)
    return finalTable
#ddd2 = testTable(data,0.8,8)
#print(ddd)
"""
Permet de retourner la table de classe finale
"""


def accuracy(classTable):
    TotalClass = len(classTable)
    goodGuess = 0
    for i in classTable:
        if(i[0] == i[1]):
            goodGuess += 1
    return goodGuess/TotalClass
#eee = accuracy(ddd)
#print(eee)
"""
Prend en entrée une table classeEstimée/classeRéelle et retourne
la précision
"""


def multiTest(data, pourcentageTest,kDebut,kFin):
    with open("resultsData.txt","w") as destination:
        for i in range(kDebut,kFin):
            print("Pour |k = {}| précision de |{:.2f}%| Pourcentage de |{}|".format(i+1,accuracy(testTable(data,pourcentageTest, i))*100,pourcentageTest))
            #destination.write("Pour |k = {}| précision de |{:.2f}%| Pourcentage de |{}|".format(i+1,accuracy(testTable(data,pourcentageTest, i))*100,pourcentageTest)+"\n")        
#multiTest(data,0.8,0,30)

"""
prend en param un dataset un pourcentage de données à garder en test et un intervalle
Affiche la précision de l'estimation pour chaque i appartenant à l'intervalle
"""



def multiPreTest(memory,verif, kDebut,kFin):
    with open("results.txt","w") as destination:
        for i in range(kDebut,kFin):
            print("Pour |k = {}| précision de |{:.2f}%|".format(i+1,accuracy(testTable2(memory,verif,i))*100))
            #destination.write("Pour |k = {}| précision de |{:.2f}%|".format(i+1,accuracy(testTable2(memory,verif,i))*100)+"\n")
#multiPreTest(data,preTest,0,30)
#multiPreTest(normalise(data),normalise(preTest),0,30)

"""
Fait comme ci-dessus mais avec le dataset pretest.csv en tant que validateur
On se sert de testTable2
"""


#print("Pour k = {} on a une précision de {:.2f} %".format(4,100*accuracy(testTable2(data,preTest,8))))
#print("Pour k = {} on a une précision de {:.2f} %".format(28,100*accuracy(testTable2(data,preTest,28))))
#print("Pour k = {} on a une précision de {:.2f} %".format(4,100*accuracy(testTable2(normalise(data),normalise(preTest),8))))


#print(accuracy(testTable(data,0.8,8)))
#tttt = normalise(data)
#affiche(data[:5])
#affiche(tttt[:5])
#print(accuracy(testTable(tttt,0.8,8)))



superDataSet = data + preTest
megaDataSet = normalise(superDataSet)
theFinalTest = normalise2(finalTest)

#finalTest
finalTab = finalTable(megaDataSet, theFinalTest, 13)
print(finalTab)

with open("FINAL.txt","w") as destFinale:
    for i in finalTab:
        destFinale.write(i+'\n')


def proportion(data):
    propTab = [0,0,0,0,0]
    for i in data:
        if(i == "classA"):
            propTab[0]+=1
        elif(i == "classB"):
            propTab[1]+=1
        elif(i == "classC"):
            propTab[2]+=1
        elif(i == "classD"):
            propTab[3]+=1
        elif(i == "classE"):
            propTab[4]+=1
    print("|{:.2f} % de la |classA |{}".format(propTab[0]/len(data)*100,propTab[0]))
    print("|{:.2f} % de la |classB |{}".format(propTab[1]/len(data)*100,propTab[1]))
    print("|{:.2f} % de la |classC |{}".format(propTab[2]/len(data)*100,propTab[2]))
    print("|{:.2f} % de la |classD |{}".format(propTab[3]/len(data)*100,propTab[3]))
    print("|{:.2f} % de la |classE |{}".format(propTab[4]/len(data)*100,propTab[4]))
    return propTab

proportion(finalTab)
















