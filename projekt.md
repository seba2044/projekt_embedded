% Sterownik domowy
% Sebastian Brzezinka, Maciej Kolendowicz
% 30 Maja, 2015

Wstęp
=====

Przeznaczenie
-------------

Sterownik przeznaczony jest do zdalnego i scentralizowanego zarządzania
urządzeniami elektrycznymi poprzez włączanie ich i wyłączanie. Sterownik
zaprojektowany jest z myślą o zarządzaniu światłem i prostymi urządzeniami
RTV/AGD. Drugorzędną funkcją sterownika jest odczyt danych z czujników do
niego podłączonych (temperatura, wilgotność, ciśnienie lub inne).

Ogólny opis użytkowania
=======================

Sterownik obsługiwany jest za pomocą dotykowego wyświetlacza, który pełni rolę
wejścia/wyjśca. Na ekranie znajdują się cztery przyciski odpowiadające
kolejnym portom, zegar oraz przycisk umożliwiający parowanie urządzenia z
telefonem. Informacje o aktywności danego portu przekazywane są poprzez zmianę
koloru przyciku.

- przycisk "OUT 1" odpowiada za przejście do panelu konfiguracyjnego portu
"OUT 1"
- przycisk "OUT 2" odpowiada za przejście do panelu konfiguracyjnego portu
"OUT 2"
- przycisk "IN 1" odpowiada za przejście do panelu konfiguracyjnego portu
"IN 1"
- przycisk "IN 1" odpowiada za przejście do panelu konfiguracyjnego portu
"IN 2"

Model statycznych aspektów sterownika
=====================================

Parametry
---------

- obsługa do 2 urządzeń za pośrednictwem portów OUT,
- obsługa do 2 czujników za pośrednictwem portów IN,
- prezentacja wyników pobranych z portów IN,
- prezentacja aktualnie włączonych urządzeń,
- ekran LCD służący do prezentacji wyników,
- zarządzanie sterownikiem przy użyciu telefonu z systemem Android;

Przypadki użycia
----------------

![diagram przypadków użycia](uml/UseCaseDiagram2.png)

System został zaprojektowany do użytkowania przez jedną osobę jednocześnie,
jednak podczas działania systemu nie musi być to ta sama osoba. Poniżej
pokazany jest diagram przypadków użycia sterownika.

Na diagramie przedstawiono _LICZBĘ_ przypadków użycia:

- wybierz akcję z panelu --
:   Ten przypadek użycia wywołuje się gdy użytkownik dostaje się do panelu
    administracyjnego któregoś z urządzeń poprzez naciśnięcie odpowiadającego
    mu przycisku na wyświetlaczu. Pozwala na (generalizuje) przypadki "wybierz
    zmianę stanu" oraz "ustaw zegar".

- wybierz zmianę stanu --
:   Ten przypadek pokrywa rozkaz użytkownika do sterownika o zmianę
    aktualnego stanu wybranego urządzenia wyjściowego. Jednocześnie użytkownik
    może wywołać przypadek "ustaw zegar" opisany poniżej.

- ustaw zegar --
:   Pozwala użytkownikowi na ustawienie czasu do automatycznej zmiany stanu
    urządzenia wyjściowego. Może zostać wywołany samodzielnie jako szczególna
    akcja z panelu administracyjnego lub rozszerzać przypadek "wybierz zmianę
    stanu".

- odczytaj dane z czujników --
:   Przypadek reprezentujący możliwość odczytania z wyświetlacza informacji
    dostarczanych przez podłączone do sterownika czujniki.

- odczytaj zegar dla urządzenia --
:   Przypadek reprezentujący możliwość odczytania ustawionego w przypadku
    "ustaw zegar" czasu do kolejnej zmiany stanu urządzenia.

- zmień stan urządzenia --
:   Ten przypadek reprezentuje faktyczną zmianę stanu urządzenia wyjściowego,
    wywoływaną przez użytkownika

- automatycznie zmień stan urządzenia --
:   Ten przypadek reprezentuje faktyczną zmianę stanu urządzenia wyjściowego
    _automatycznie_, zgodnie z ustawionym przez użytkownika zegarem.


Użytkownik używa systemu wybierając konkretne akcje z paneli urządzeń
(aktywnie, trzy pierwsze przypadki użycia) lub odczytując dane pojawiające się
na głównym ekranie (pasywnie, kolejne dwa przypadki użycia). Ostatni przypadek
jest wywoływany wewnętrznie poprzez sterownik na podstawie wprowadzonych przez
użytkownika rozkazów.

Uproszczony widok konstrukcji obiektu
-------------------------------------

![diagram klas konstrukcji obiektu](uml/ClassDiagram2.png)

- główny kontroler --
:   Obiekt zajmujący się kontrolą wszystkich urządzeń oraz czujników, jak
    również obsługą zdarzeń interfejsu (rozkazy użytkownika i wyświetlanie
    informacji na ekranie)
    
- zegary urządzeń --
:   Jednostki odpowiedzialne za automatyczną zmianę stanu urządzeń
    wejściowych. 
    
- wyświetlacz i dotyk wyświetlacza --
:   Stanowią całość interfejsu użytkownika. Wyświetlacz dotykowy (pomimo
    bycia postrzeganym jako jedno fizyczne urządzenie) zdecydowano się opisać
    jako dwa różne obiekty. Pozwala to na ewentualną modyfikację sterownika i
    zastąpienie wyświetlacza dotykowego wyświetlaczem oddzielonym od klawiatury.
    
- czujnik --
:   Reprezentuje wejście interfejsu czujników dołączonych do sterownika.

- włącznik urządzenia --
:   Reprezenteuje fizyczne układy podłączone do urządzeń wyjściowych. Ich
    zadaniem jest ujednolicenie zadań włączania i wyłączania urządzeń.


Z powyżej opisanych przupadków użycia oraz wymagań systemu jasno wynika
pokazany poniżej diagram klas. Należy zauważyć że nie jest to diagram klas w
ścisłym rozumieniu UML -- służy jedynie do opisania budowy sterownika w
kontekście wykonywanych funkcji.

Powyższy diagram klas wynika bezpośrednio z opisanych wcześniej przypadków
użycia. Klasy na nim opisane pokrywają wszystkie przypadki użycia. Przypadki
"wybierz akcję z panelu" wiążą obiekty "kontroler" oraz obiekty wyświetlacza.
Odpowiednie uszczególnienia tych przypadków wywołują przypadki "ustaw zegar" i
"zmień stan urządzenia", które wykonują się pomiędzy odpowiednio "zegarami
urządzenia" i "kontrolerem" oraz "włącznikami urządzenia" i "kontrolerem".
Przypadek "automatycznie zmień stan urządzenia" wiąże klasy "zegar urządzenia"
i "włącznik urządzenia" dla każdego z urządzeń wejściowych.

Ze względu na bezpieczeństwo systemu zdecydowano o fizycznym rozdzieleniu
głównego kontrolera i zegarów urządzeń. Pozwala to na niezawodne działanie
automatycznego wyłączania urządzeń w przypadku awarii innych obiektów systemu.
Wprowadza to również komplikację w postaci podwójnego wejścia włącznika
urządzenia, co może prowadzić do wyścigu o zasoby (przypadek jednoczesnej
zmiany stanu urządzenia automatycznie, na wniosek zegara i na żądanie
użytkownika poprzez kontroler).


Panele konfiguracyjne
=====================

OUT
---

Po podłączeniu urządzenia do portu OUT nr staje się aktywny przycisk "OUT nr",
co pozwala na przejście do panelu administracyjnego. Panel administracyjny
pozwala na włączenie/wyłączenie zasilania urządzenia podpiętego do danego
portu. Panel pozwala również na ustalenie przedziału czasowego w którym
urządzenie jest włączone. Dodatkową opcją panelu jest możliwość sprawdzenia
poboru energii urządzenia podpiętego do portu.

IN
--

Po naciśnięciu przycisku "IN nr" wyświetlone zostają informacje o sensorze
oraz dane przekazane do urządzenia przez sensor. W panelu tym znajduje
się przycisk umożliwiający odłączenie zasilania od czujnika.


