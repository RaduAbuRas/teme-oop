comp1=[]
list2 = [[]for y in range(100)]
#matrice tridimensionala in care memorez valorile rezultate din operatiile din input
#programul functioneaza cu valori pana la 100 dar evident se pot marii
matValori = [[[0 for x in range(100)]for y in range(100)]for z in range(100)]

'''
folosesc urmatoarele 4 variabile pentru a verifica daca operatia este grup , ele sunt updatate cand functiile ce
verifica asociativitatea , comutativitatea , elementul neutru si elementele simetrice sunt chemate(initial sunt 0)
'''
EsteAsociativa = 0
EsteComutativa = 0
ExistaElementNeutru = 0
ExistaElementeSimetrice = 0
MarimeMultime = 0
Multime = []
Multime2 = []

def citire():
    #cateva variabile folosite pentru stocarea multimii de elemente
    global MultimeInitiala , Multime2 , Multime
    global MarimeMultime
    MarimeMultime = 0
    MarimeMultime = input("Sa se dea marimea multimi :")
    MarimeMultime = int(MarimeMultime)

    #variabila in care se vor retine elementele multimii pe care se opereaza
    MultimeInitiala = input("Sa se dea multimea de numere A sub forma  a , b , c , d ...  :")

    '''
    o a doua multime pe care o folosesc la afisare deoarece pe prima o modific ca sa imi fie mai usor
    la prelucrarea elementelor
    '''

    Multime2 = MultimeInitiala

    '''
    folosesc split pentru a cauta intre virgule cifrele si le adaug in Multime pentru a le folosii in functiile
    de mai jos
    '''
    MultimeInitiala = MultimeInitiala.replace(" ","")
    MultimeInitiala = MultimeInitiala.strip()

    for s in MultimeInitiala.split(","):
        if s.isdigit():
            Multime.append(s)

    '''
    -se citesc operatiile care definesc functia apoi agaug in comp1 cifrele ce se gasesc in list folosesc elementele
     asa cum sunt pe post de  x , y , z cand completez matricea tridimensionala
    -de asemenea la fel ca mai sus folosesc o a doua lista pentru afisarea operatiilor asa cum sunt date
    '''

    #la definirea unei operatii trebuie introduse marimea multii la patrat operatii (fiecare element cu fiecare element)
    print('Sa se defineasca operatia folosind elementele din multimea data astfel "x op y = z" :')
    for i in range(1 , MarimeMultime * MarimeMultime + 1):
        list = input()
        list2[i] = list

        #din lista extrag doar numerele , pot aparea erori de scriere precum 0 opp 0 = 0 fara probleme

        for s in list.split():
            if s.isdigit() :
                comp1.append(s)

        #notez cele trei numere cu x , y , z
        x = int(comp1[0])
        y = int(comp1[1])
        z = int(comp1[2])

        #le adaug la matricea de valori
        matValori[x][y][0] = z
        matValori[x][y][0] = int(matValori[x][y][0])
        del comp1[:]



def Comutativitate():
    global EsteComutativa
    EsteComutativa = 0
    #verific daca s-a introdus o multime sau nu
    if MarimeMultime == 0:
        print("Nu s-a introdus o multime de numere si o operatie binara , selectati optiunea 1 mai intai\n")
    else:
        #parcurg matricea de valori in lungime si latime
        ok = 1
        for i in range(0 , MarimeMultime - 1):
            for j in range(0 , MarimeMultime - 1):
                #folosesc valorile din matricea de valori accesand elementele din multimea data (matValori de Multime[i] si Multime[j])
                if matValori[  int(Multime[i])  ][   int(Multime[j])  ][  0   ]  != matValori[   int(Multime[j])   ][   int(Multime[i])  ][  0   ] :
                    ok = 0

        if ok == 1:
            EsteComutativa = 1
            print("Functia este comutativa\n")
        else :
            print("Functia nu este comutativa\n")


def Asociativitate():
    global EsteAsociativa
    if MarimeMultime == 0:
        print("Nu s-a introdus o multime de numere si o operatie binara , selectati optiunea 1 mai intai\n")
    else:
        #la fel ca la comutativitate parcurg matricea si verific daca relatia clasica de comutativitate este adevarata
        ok = 1
        for i in range(0 , MarimeMultime - 1):
            for j in range(0 , MarimeMultime - 1):
                for k in range(0 , MarimeMultime - 1):
                    if matValori[   matValori[  int(Multime[i])   ][  int(Multime[j])   ][  0   ]][ int(Multime[k])   ][  0   ]  !=  matValori[   int(Multime[i])   ][  matValori[  int(Multime[j])   ][  int(Multime[k])   ][  0   ]][ 0   ]  :
                        ok = 0

        if ok == 1:
            EsteAsociativa = 1
            print("Functia este asociativa\n")
        else :
            print("Functia nu este asociativa\n")


def ElementNeutru():
    global ExistaElementNeutru
    ExistaElementNeutru = 0
    global Element
    Element = 0
    if MarimeMultime == '0':
        print("Nu s-a introdus o multime de numere si o operatie binara , selectati optiunea 1 mai intai\n")
    else:
        #caut in matricea de valori daca exista un element astfel incat op(a,e)=op(e,a)=a
        for i in range(0 , MarimeMultime - 1):
            ok = 1
            for j in range(0 , MarimeMultime - 1):
                if matValori[   int(Multime[j])   ][  int(Multime[i])   ][  0   ] != int(Multime[j]):
                    ok = 0
            if ok == 1:
                ExistaElementNeutru = 1
                #daca este gasit il salvez pentru afisare
                Element = int(Multime[i])
                break

        if ok == 1:
            print("Elementul neutru exista si este :",Element,"\n")
        else:
            print("Elementul neutru nu exista\n")


def ElementeSimetrice():
    global ExistaElementeSimetrice
    ExistaElementeSimetrice = 0
    if MarimeMultime == 0:
        print("Nu s-a introdus o multime de numere si o operatie binara , selectati optiunea 1 mai intai\n")
        print()
    else:
        ok = 1
        for i in range(0 , MarimeMultime - 1):
            ok2 = 0
            for j in range(0 , MarimeMultime - 1):
                #ma folosesc de relatia op(a,a')=op(a',a)=e ca sa verific daca exista elemente simetrice sau nu
                if matValori[   int(Multime[i])   ][   int(Multime[j])  ][  0   ] == matValori[   int(Multime[j])   ][   int(Multime[i])  ][  0   ] == Element :
                    ok2 = 1
        if ok2 == 0:
            ok = 0

        if ok == 1:
            ExistaElementeSimetrice = 1
            print("Fiecare element are un simetric\n")
        else:
            print("Fiecare element nu are un simetric\n")

def Grup():
    if MarimeMultime == 0:
        print("Nu s-a introdus o multime de numere si o operatie binara , selectati optiunea 1 mai intai\n")
        print()
    else:
        '''
        Pentru verificarea conditiei de grup sau grup comutativ ma folosesc de cele 4 variabile pe care le-am declarat
        la inceput . Observati ca la inceput ele sunt initializate cu 0 si se schimba doar la apelarea functiei deci
        chiar daca o operatie este asociativa , daca nu a fost apelata functia de asociativitate cand o sa verificati
        daca este grup o sa rezulte ca nu
        '''
        if EsteAsociativa == 1:
            ElementNeutru()
            if ExistaElementNeutru == 1:
                ElementeSimetrice()
                if ExistaElementeSimetrice == 1:
                    if EsteComutativa == 1:
                        print("Operatia binara este un grup comutativ")
                    else:
                        print("Operatia binara este un grup")
                else:
                    print("Operatia binara nu este un grup deoarece nu are elemente simetrice sau nu s-a verificat aceasta proprietate")
            else:
                print("Operatia binara nu este un grup deoarece nu are element neutru sau nu s-a verificat aceasta proprietate")
        else:
            print("Operatia binara nu este un grup deoarece nu este asociativa sau nu s-a verificat aceasta proprietate")

        print()

#functia folosita la optiunea 1 , ce afiseaza multimea si operatiile
def Afisare():
    if MarimeMultime == '0':
        print("Nu s-a introdus o multime de numere si o operatie binara\n")
    else:
        print("Multimea de elemente este:")
        print("{",Multime2,"}")
        print()
        print("Operatiile ce definesc operatia binara sunt:")
        for i in range(1 ,MarimeMultime ** 2 + 1 ):
            print(list2[i])
    print()


def MENU():
    print("MENIU:")
    print("Optiunea 1 : Citeste o multime de numere si o operatie binara.")
    print("Optiunea 2 : Se afiseaza multimea data si operatiile ce definesc operatia binara")
    print("Optiunea 3 : Verifica daca operatia este comutativa ")
    print("Optiunea 4 : Verifica daca operatia este asociativa ")
    print("Optiunea 5 : Verifica daca operatia este grup / grup comutativ ")
    print("Optiunea 6 : Iesire din program")
    type = input("Alegeti optiunea : ")
    if type == '1':
        print()
        citire()
        MENU()
    elif type == '2':
        print()
        Afisare()
        MENU()
    elif type == '3':
        print()
        Comutativitate()
        MENU()
    elif type == '4':
        print()
        Asociativitate()
        MENU()
    elif type == '5':
        print()
        Grup()
        MENU()
    elif type == '6':
        print()
        print("Programul se va inchide.")
    else:
        print()
        print("Optiunea aleasa este invalida , introduceti alta.")
        MENU()


MENU()
