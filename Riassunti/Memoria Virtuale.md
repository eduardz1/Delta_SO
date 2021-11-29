// work in progress //

# Memoria Virtuale
Per realizzare la memoria virtuale sono necessari:
1. Algoritmo di Sostituzione delle Pagine 
1. Algoritmo di allocazione dei frame     

## **Memoria RAM**

- Allocazione con **caricamento totale**
    - VINCOLO sullo spazio occupabile dai processi
    - VINCOLO sul programmatore
    - Il grado di MULTIPROGRAMMAZIONE cresce
    - La Memoria necessaria potrebbe essere inferiroe a quella Caricata
- Allocazione mediante **memoria virtuale**
    - PAGINAZIONE

### PAGINA INVALIDA
2 Definizioni:
- Pagina non caricata in RAM
- Pagina alla quale si effettua accesso in maniera illecita ( *es. OS che assegna ad un processo spazio extra per frammentazione e si effetua la lettura su queste pagine per frammentazione* )

### COPIATURA SU SCRITTURA
In caso di fork il processo figlio clona la tabella delle pagine del padre, P ed F condividono lo stack fino a quando uno dei due non lo modifica. Quando questo avviene si copia lo stack modificato in un frame libero
- Aumento l'efficienza di gestione della RAM

### POOL OF FREE FRAMES
In caso di PAGE FAULT la pagina da caricare viene scritta in un frame libero mentre la pagine vittima diventa un frame libero ( *che può essere sovrascritto* )

Se il processo ha bisogno della pagina vittima questa è già presente in RAM e basta linkarla 
- Evito la copiatura da disco

## **Memoria Secondaria** - *area di swap*
- Vista come estensione della RAM accedibile solo in caso di PAGE FAULT
- Contiene dati ( *stack* ), non codice ( *file system* )



# Algoritmi di Sostituzione delle Pagine
Quando la richiesta di una pagina porta a page fault [^1]:
1. Verifico la validità della pagina
2. Identifico un frame libero
3. Se lo trovo
   1. Avvio caricamento pagine ( *processo in waiting* )
   2. Aggiorno la tabella delle pagine del processo
   3. Processo ready
4. Altrimenti
   1. Trovo una pagina vittima
   2. Effettuo la sostituzione
      1. Salvo la pagina vittima su disco [^2]
      2. Copio la pagina che ha causato page fault nello stesso frame
   
Gestione del page fault:
1. Gestione dell'interruzione $\simeq 10^-6$ secondi
2. Lettura della pagina mancante $\simeq 10^-3$ secondi
3. Riavvio del processo $\simeq 10^-6$ secondi

## FIFO - *First In First Out*


[^1]: *durante la fase di fetch o execute, operazione costosa*
[^2]: *solo se ha associato un dirty bit con valore 1, che sta ad indicare che la copia in RAM è diversa da quella su disco*