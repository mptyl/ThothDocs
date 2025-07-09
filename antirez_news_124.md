# Scrivere software di sistema: commenti al codice

**Autore:** antirez  
**Data:** 2466 giorni fa  
**Visualizzazioni:** 382077  

Da un po' di tempo volevo registrare un nuovo video per parlare dei commenti al codice per la mia serie "scrivere software di sistema" su YouTube. Tuttavia, dopo averci pensato, ho realizzato che l'argomento era più adatto a un post sul blog, quindi eccoci qui. In questo post analizzo i commenti in Redis, cercando di categorizzarli. Lungo il percorso, cerco di mostrare perché, a mio avviso, scrivere commenti è di fondamentale importanza per produrre codice di qualità, che sia manutenibile nel lungo periodo e comprensibile agli altri e agli autori durante le attività di modifica e debug.

Non tutti la pensano allo stesso modo. Molti credono che i commenti siano inutili se il codice è abbastanza solido. L'idea è che quando tutto è ben progettato, il codice stesso documenta ciò che fa, rendendo i commenti superflui. Non sono d'accordo con questa visione per due motivi principali:

1. Molti commenti non spiegano cosa fa il codice. Spiegano ciò che non si può capire solo da ciò che il codice fa. Spesso queste informazioni mancanti riguardano il *perché* il codice sta eseguendo una certa azione, o perché sta facendo qualcosa di ovvio invece di qualcos'altro che sembrerebbe più naturale.

2. Anche se non è generalmente utile documentare, riga per riga, cosa fa il codice, perché è comprensibile semplicemente leggendolo, un obiettivo chiave nella scrittura di codice leggibile è ridurre la quantità di sforzo e il numero di dettagli che il lettore deve tenere a mente mentre legge del codice. Quindi, per me, i commenti possono essere uno strumento per ridurre il carico cognitivo del lettore.

Il seguente frammento di codice è un buon esempio del secondo punto sopra. Nota che tutti gli snippet di codice in questo post sul blog sono ottenuti dal codice sorgente di Redis. Ogni snippet di codice è presentato con il prefisso del nome del file da cui è stato estratto. Il branch utilizzato è l'attuale "unstable" con hash 32e0d237.

**scripting.c:**
```c
/* Initial Stack: array */
lua_getglobal(lua,"table");
lua_pushstring(lua,"sort");
lua_gettable(lua,-2);       /* Stack: array, table, table.sort */
lua_pushvalue(lua,-3);      /* Stack: array, table, table.sort, array */
if (lua_pcall(lua,1,0,0)) {
    /* Stack: array, table, error */

    /* We are not interested in the error, we assume that the problem is
     * that there are 'false' elements inside the array, so we try
     * again with a slower function but able to handle this case, that
     * is: table.sort(table, __redis__compare_helper) */
    lua_pop(lua,1);             /* Stack: array, table */
    lua_pushstring(lua,"sort"); /* Stack: array, table, sort */
    lua_gettable(lua,-2);       /* Stack: array, table, table.sort */
    lua_pushvalue(lua,-3);      /* Stack: array, table, table.sort, array */
    lua_getglobal(lua,"__redis__compare_helper");
    /* Stack: array, table, table.sort, array, __redis__compare_helper */
    lua_call(lua,2,0);
}
```

Lua utilizza un'API basata su stack. Un lettore che segue ogni chiamata nella funzione sopra, avendo anche un riferimento all'API di Lua a portata di mano, sarà in grado di ricostruire mentalmente il layout dello stack in ogni momento dato. Ma perché costringere il lettore a fare uno sforzo del genere? Mentre scriveva il codice, l'autore originale ha dovuto fare comunque quello sforzo mentale. Quello che ho fatto lì è stato semplicemente annotare ogni riga con il layout dello stack corrente dopo ogni chiamata. Leggere questo codice è ora banale, indipendentemente dal fatto che l'API di Lua sia altrimenti non banale da seguire.

Il mio obiettivo qui non è solo offrire il mio punto di vista sull'utilità dei commenti come strumento per fornire un contesto che non è chiaramente disponibile leggendo una sezione locale del codice sorgente. Ma anche fornire alcune prove sull'utilità del tipo di commenti che storicamente sono considerati inutili o addirittura pericolosi, ovvero i commenti che affermano *cosa* fa il codice, e non il perché.

## Classificazione dei commenti

Il modo in cui ho iniziato questo lavoro è stato leggere parti casuali del codice sorgente di Redis, per verificare se e perché i commenti fossero utili in contesti diversi. Ciò che è emerso rapidamente è che i commenti sono utili per ragioni molto diverse, poiché tendono a essere molto diversi in funzione, stile di scrittura, lunghezza e frequenza di aggiornamento. Alla fine ho trasformato il lavoro in un compito di classificazione.

Durante la mia ricerca ho identificato nove tipi di commenti:

- Commenti di funzione
- Commenti di design
- Commenti sul perché
- Commenti da insegnante
- Commenti di checklist
- Commenti guida
- Commenti banali
- Commenti di debito
- Commenti di backup

I primi sei sono, a mio parere, forme di commento per lo più molto positive, mentre gli ultimi tre sono alquanto discutibili. Nelle sezioni successive ogni tipo sarà analizzato con esempi dal codice sorgente di Redis.

### COMMENTI DI FUNZIONE

L'obiettivo di un commento di funzione è impedire al lettore di leggere il codice in primo luogo. Invece, dopo aver letto il commento, dovrebbe essere possibile considerare del codice come una scatola nera che deve obbedire a determinate regole. Normalmente i commenti di funzione si trovano all'inizio delle definizioni delle funzioni, ma possono essere in altri luoghi, documentando classi, macro o altri blocchi di codice funzionalmente isolati che definiscono qualche interfaccia.

**rax.c:**
```c
/* Seek the grestest key in the subtree at the current node. Return 0 on
 * out of memory, otherwise 1. This is an helper function for different
 * iteration functions below. */
int raxSeekGreatest(raxIterator *it) {
...
```

I commenti di funzione sono in realtà una forma di documentazione API in linea. Se il commento della funzione è scritto abbastanza bene, l'utente dovrebbe essere in grado, nella maggior parte dei casi, di tornare a ciò che stava leggendo (leggendo il codice che chiama tale API) senza dover leggere l'implementazione di una funzione, una classe, una macro o altro.

Tra tutti i tipi di commenti, questi sono quelli più ampiamente accettati dalla comunità di programmazione in generale come necessari. L'unico punto da analizzare è se sia una buona idea collocare commenti che sono in gran parte documentazione di riferimento API all'interno del codice stesso. Per me la risposta è semplice: voglio che la documentazione API corrisponda esattamente al codice. Man mano che il codice viene modificato, la documentazione dovrebbe essere modificata. Per questo motivo, utilizzando i commenti di funzione come prologo di funzioni o altri elementi, rendiamo la documentazione API vicina al codice, ottenendo tre risultati:

- Man mano che il codice viene modificato, la documentazione può essere facilmente modificata allo stesso tempo, senza il rischio di rendere obsoleto il riferimento API.

- Questo approccio massimizza la probabilità che l'autore della modifica, che dovrebbe essere quello che meglio comprende la modifica, sarà anche l'autore della modifica alla documentazione API.

- Leggere il codice è utile per trovare la documentazione di funzioni o metodi direttamente dove sono definiti, in modo che il lettore del codice possa concentrarsi esclusivamente sul codice, invece di passare continuamente tra codice e documentazione.

### COMMENTI DI DESIGN

Mentre un "commento di funzione" è solitamente situato all'inizio di una funzione, un commento di design si trova più spesso all'inizio di un file. Il commento di design afferma fondamentalmente come e perché un dato pezzo di codice utilizza determinati algoritmi, tecniche, trucchi e implementazioni. È una panoramica di livello superiore di ciò che vedrai implementato nel codice. Con tale contesto, leggere il codice sarà più semplice. Inoltre, tendo a fidarmi di più del codice dove posso trovare note di design. Almeno so che una qualche fase di design esplicita è avvenuta, a un certo punto, durante il processo di sviluppo.

Nella mia esperienza, i commenti di design sono anche molto utili per affermare, nel caso in cui la soluzione proposta dall'implementazione sembri un po' troppo banale, quali erano le soluzioni concorrenti e perché una soluzione molto semplice è stata considerata sufficiente per il caso in questione. Se il design è corretto, il lettore si convincerà che la soluzione è appropriata e che tale semplicità deriva da un processo, non dall'essere pigri o dal sapere solo come codificare cose basilari.

**bio.c:**
```c
 * DESIGN
 * ------
 *
 * The design is trivial, we have a structure representing a job to perform
 * and a different thread and job queue for every job type.
 * Every thread waits for new jobs in its queue, and process every job
 * sequentially.
 ...
```

### COMMENTI SUL PERCHÉ

I commenti sul perché spiegano la ragione per cui il codice sta facendo qualcosa, anche se ciò che il codice sta facendo è cristallino. Vedi il seguente esempio dal codice di replica di Redis.

**replication.c:**
```c
if (idle > server.repl_backlog_time_limit) {
    /* When we free the backlog, we always use a new
     * replication ID and clear the ID2. This is needed
     * because when there is no backlog, the master_repl_offset
     * is not updated, but we would still retain our replication
     * ID, leading to the following problem:
     *
     * 1. We are a master instance.
     * 2. Our replica is promoted to master. It's repl-id-2 will
     *    be the same as our repl-id.
     * 3. We, yet as master, receive some updates, that will not
     *    increment the master_repl_offset.
     * 4. Later we are turned into a replica, connect to the new
     *    master that will accept our PSYNC request by second
     *    replication ID, but there will be data inconsistency
     *    because we received writes. */
    changeReplicationId();
    clearReplicationId2();
    freeReplicationBacklog();
    serverLog(LL_NOTICE,
        "Replication backlog freed after %d seconds "
        "without connected replicas.",
        (int) server.repl_backlog_time_limit);
}
```

Se controllo solo le chiamate di funzione, c'è poco da chiedersi: se viene raggiunto un timeout, cambia l'ID di replica principale, cancella l'ID secondario e infine libera il backlog di replica. Tuttavia, non è esattamente chiaro perché dobbiamo cambiare gli ID di replica quando liberiamo il backlog.

Ora, questo è il tipo di cosa che accade continuamente nel software una volta raggiunto un certo livello di complessità. Indipendentemente dal codice coinvolto, il protocollo di replica ha un certo livello di complessità intrinseca, quindi dobbiamo fare determinate cose per assicurarci che altre cose negative non possano accadere. Probabilmente questi tipi di commenti sono, in qualche modo, opportunità per ragionare sul sistema e verificare se dovrebbe essere migliorato, in modo che tale complessità non sia più necessaria, quindi anche il commento può essere rimosso. Tuttavia, spesso rendere qualcosa più semplice può rendere qualcos'altro più difficile o semplicemente non è fattibile, o richiede lavoro futuro che rompe la compatibilità all'indietro.

Ecco un altro esempio.

**replication.c:**
```c
/* SYNC can't be issued when the server has pending data to send to
 * the client about already issued commands. We need a fresh reply
 * buffer registering the differences between the BGSAVE and the current
 * dataset, so that we can copy to other replicas if needed. */
if (clientHasPendingReplies(c)) {
    addReplyError(c,"SYNC and PSYNC are invalid with pending output");
    return;
}
```

Se esegui SYNC mentre ci sono ancora output pendenti (da un comando precedente) da inviare al client, il comando dovrebbe fallire perché durante l'handshake di replica il buffer di output del client viene utilizzato per accumulare modifiche, e potrebbe essere successivamente duplicato per servire altre repliche che si connettono mentre stiamo già creando il file RDB per la sincronizzazione completa con la prima replica. Questo è il perché facciamo così. Quello che facciamo è banale. Risposte pendenti? Emetti un errore. Il perché è piuttosto oscuro senza il commento.

Si potrebbe pensare che tali commenti siano necessari solo quando si descrivono protocolli e interazioni complessi, come nel caso della replica. È così? Cambiamo completamente file e obiettivi, e vediamo comunque tali commenti ovunque.

**expire.c:**
```c
for (j = 0; j < dbs_per_call && timelimit_exit == 0; j++) {
    int expired;
    redisDb *db = server.db+(current_db % server.dbnum);

    /* Increment the DB now so we are sure if we run out of time
     * in the current DB we'll restart from the next. This allows to
     * distribute the time evenly across DBs. */
    current_db++;
    ...
```

Questo è interessante. Vogliamo far scadere le chiavi da diversi DB, fintanto che abbiamo tempo. Tuttavia, invece di incrementare l'ID del database da processare successivamente alla fine del ciclo che elabora il database corrente, lo facciamo diversamente: selezioniamo il DB corrente nella variabile `db`, ma poi incrementiamo immediatamente l'ID del database successivo da processare (alla prossima chiamata di questa funzione). In questo modo, se la funzione termina perché è stato speso troppo sforzo in una singola chiamata, non abbiamo il problema di ricominciare sempre dallo stesso database, lasciando che le chiavi logicamente scadute si accumulino negli altri database poiché siamo troppo concentrati a processare ripetutamente lo stesso database.

Con tale commento spieghiamo sia perché incrementiamo in quella fase, sia che la prossima persona che andrà a modificare il codice dovrebbe preservare tale qualità. Nota che senza il commento il codice sembra completamente innocuo. Seleziona, incrementa, vai a fare del lavoro. Non c'è una ragione evidente per non spostare l'incremento alla fine del ciclo dove potrebbe sembrare più naturale.

Trivia: l'incremento del ciclo era infatti alla fine nel codice originale. È stato spostato lì durante una correzione: allo stesso tempo è stato aggiunto il commento. Quindi diciamo che questo è una sorta di "commento di regressione".

### COMMENTI DA INSEGNANTE

I commenti da insegnante non cercano di spiegare il codice stesso o certi effetti collaterali di cui dovremmo essere consapevoli. Insegnano invece il *dominio* (ad esempio matematica, grafica computerizzata, networking, statistica, strutture dati complesse) in cui opera il codice, che potrebbe essere al di fuori delle competenze del lettore, o è semplicemente troppo pieno di dettagli per ricordarli tutti a memoria.

Il comando LOLWUT nella versione 5 deve mostrare quadrati ruotati sullo schermo (<a rel="nofollow" href="http://antirez.com/news/123">http://antirez.com/news/123</a>). Per farlo utilizza un po' di trigonometria di base: nonostante il fatto che la matematica utilizzata sia semplice, molti programmatori che leggono il codice sorgente di Redis potrebbero non avere un background matematico, quindi il commento all'inizio della funzione spiega cosa accadrà all'interno della funzione stessa.

**lolwut5.c:**
```c
/* Draw a square centered at the specified x,y coordinates, with the specified
 * rotation angle and size. In order to write a rotated square, we use the
 * trivial fact that the parametric equation:
 *
 *  x = sin(k)
 *  y = cos(k)
 *
 * Describes a circle for values going from 0 to 2*PI. So basically if we start
 * at 45 degrees, that is k = PI/4, with the first point, and then we find
 * the other three points incrementing K by PI/2 (90 degrees), we'll have the
 * points of the square. In order to rotate the square, we just start with
 * k = PI/4 + rotation_angle, and we are done.
 *
 * Of course the vanilla equations above will describe the square inside a
 * circle of radius 1, so in order to draw larger squares we'll have to
 * multiply the obtained coordinates, and then translate them. However this
 * is much simpler than implementing the abstract concept of 2D shape and then
 * performing the rotation/translation transformation, so for LOLWUT it's
 * a good approach. */
```

Il commento non contiene nulla che sia correlato al codice della funzione stessa, o ai suoi effetti collaterali, o ai dettagli tecnici relativi alla funzione. La descrizione è limitata solo al concetto matematico che viene utilizzato all'interno della funzione per raggiungere un dato obiettivo.

Penso che i commenti da insegnante siano di enorme valore. Insegnano qualcosa nel caso in cui il lettore non sia a conoscenza di tali concetti, o almeno forniscono un punto di partenza per ulteriori indagini. Ma questo a sua volta significa che un commento da insegnante aumenta la quantità di programmatori che possono leggere un certo percorso di codice: scrivere codice che può essere letto da molti programmatori è un obiettivo principale per me. Ci sono sviluppatori che potrebbero non avere competenze matematiche ma sono programmatori molto solidi che possono contribuire con qualche meravigliosa correzione o ottimizzazione. E in generale il codice dovrebbe essere letto oltre che eseguito, poiché è scritto da esseri umani per altri esseri umani.

Ci sono casi in cui i commenti da insegnante sono quasi impossibili da evitare per scrivere codice decente. Un buon esempio è l'implementazione dell'albero radix in Redis. Gli alberi radix sono strutture dati articolate. L'implementazione di Redis ristabilisce l'intera teoria della struttura dati mentre la implementa, mostrando i diversi casi e cosa fa l'algoritmo per unire o dividere i nodi e così via. Immediatamente dopo ogni sezione di commento, abbiamo il codice che implementa ciò che è stato scritto prima. Dopo mesi di non toccare il file che implementa l'albero radix, sono stato in grado di aprirlo, correggere un bug in pochi minuti e continuare a fare qualcos'altro. Non c'è bisogno di studiare di nuovo come funziona un albero radix, poiché la spiegazione è la stessa cosa del codice stesso, tutto mescolato insieme.

I commenti sono troppo lunghi, quindi mostrerò solo alcuni frammenti.

**rax.c:**
```c
/* If the node we stopped at is a compressed node, we need to
 * split it before to continue.
 *
 * Splitting a compressed node have a few possible cases.
 * Imagine that the node 'h' we are currently at is a compressed
 * node contaning the string "ANNIBALE" (it means that it represents
 * nodes A -> N -> N -> I -> B -> A -> L -> E with the only child
 * pointer of this node pointing at the 'E' node, because remember that
 * we have characters at the edges of the graph, not inside the nodes
 * themselves.
 *
 * In order to show a real case imagine our node to also point to
 * another compressed node, that finally points at the node without
 * children, representing 'O':
 *
 *     "ANNIBALE" -> "SCO" -> []
 ... snip ...
 * 3a. IF $SPLITPOS == 0:
 *     Replace the old node with the split node, by copying the auxiliary
 *     data if any. Fix parent's reference. Free old node eventually
 *     (we still need its data for the next steps of the algorithm).
 *
 * 3b. IF $SPLITPOS != 0:
 *     Trim the compressed node (reallocating it as well) in order to
 *     contain $splitpos characters. Change chilid pointer in order to link
 *     to the split node. If new compressed node len is just 1, set
 *     iscompr to 0 (layout is the same). Fix parent's reference.
 ... snip ...
    if (j == 0) {
        /* 3a: Replace the old node with the split node. */
        if (h->iskey) {
            void *ndata = raxGetData(h);
            raxSetData(splitnode,ndata);
        }
        memcpy(parentlink,&splitnode,sizeof(splitnode));
    } else {
        /* 3b: Trim the compressed node. */
        trimmed->size = j;
        memcpy(trimmed->data,h->data,j);
        trimmed->iscompr = j > 1 ? 1 : 0;
        trimmed->iskey = h->iskey;
        trimmed->isnull = h->isnull;
        if (h->iskey && !h->isnull) {
            void *ndata = raxGetData(h);
            raxSetData(trimmed,ndata);
        }
        raxNode **cp = raxNodeLastChildPtr(trimmed);
    ...
```

Come puoi vedere, la descrizione nel commento è poi abbinata con le stesse etichette nel codice. È difficile mostrarlo tutto in questa forma, quindi se vuoi avere un'idea completa, controlla il file completo su:

    <a rel="nofollow" href="https://github.com/antirez/redis/blob/unstable/src/rax.c">https://github.com/antirez/redis/blob/unstable/src/rax.c</a>

Questo livello di commento non è necessario per tutto, ma cose come gli alberi radix sono davvero pieni di piccoli dettagli e casi limite. Sono difficili da ricordare, e certi dettagli sono *specifici* di una data implementazione. Fare questo per una lista concatenata non ha molto senso, ovviamente. È una questione di sensibilità personale capire quando ne vale la pena o no.

### COMMENTI DI CHECKLIST

Questo è molto comune e strano: a volte a causa di limitazioni del linguaggio, problemi di design, o semplicemente a causa della complessità naturale che emerge nei sistemi, non è possibile centralizzare un dato concetto o interfaccia in un unico pezzo, quindi ci sono posti nel codice che ti dicono di ricordarti di fare cose in qualche altro posto del codice. Il concetto generale è:

    /* Attenzione: se aggiungi un ID di tipo qui, assicurati di modificare anche
     * la funzione getTypeNameByID(). */

In un mondo perfetto questo non dovrebbe mai essere necessario, ma in pratica a volte non ci sono vie di fuga da ciò. Ad esempio, i tipi di Redis potrebbero essere rappresentati usando una struttura di "tipo di oggetto", e ogni oggetto potrebbe collegarsi al tipo a cui appartiene, quindi potresti fare:

    printf("Il tipo è %s\n", myobject->type->name);

Ma indovina un po'? È troppo costoso per noi, perché un oggetto Redis è rappresentato così:

**redisObject:**
```c
typedef struct redisObject {
    unsigned type:4;
    unsigned encoding:4;
    unsigned lru:LRU_BITS; /* LRU time (relative to global lru_clock) or
                            * LFU data (least significant 8 bits frequency
                            * and most significant 16 bits access time). */
    int refcount;
    void *ptr;
} robj;
```

Usiamo 4 bit invece di 64 per rappresentare il tipo. Questo è solo per mostrare perché a volte le cose non sono così centralizzate e naturali come dovrebbero essere. Quando la situazione è così, a volte ciò che aiuta è usare commenti difensivi per assicurarsi che se una certa sezione di codice viene toccata, ti ricordi di modificare anche altre parti del codice. Specificamente, un commento di checklist fa una o entrambe le seguenti cose:

* Ti dice un insieme di azioni da fare quando qualcosa viene modificato.

* Ti avverte sul modo in cui certe modifiche dovrebbero essere operate.

Un altro esempio in blocked.c, quando viene introdotto un nuovo tipo di blocco.

**blocked.c:**
```c
 * When implementing a new type of blocking opeation, the implementation
 * should modify unblockClient() and replyToBlockedClientTimedOut() in order
 * to handle the btype-specific behavior of this two functions.
 * If the blocking operation waits for certain keys to change state, the
 * clusterRedirectBlockedClientIfNeeded() function should also be updated.
```

Il commento di checklist è utile anche in un contesto simile a quando vengono usati certi "commenti sul perché": quando non è ovvio perché un certo codice deve essere eseguito in un dato posto, dopo o prima di qualcosa. Ma mentre il commento sul perché può dirti perché una dichiarazione è lì, il commento di checklist usato nello stesso caso è più orientato a dirti quali regole seguire se vuoi modificarlo (in questo caso la regola è, seguire un dato ordine), senza rompere il comportamento del codice.

**cluster.c:**
```c
/* Update our info about served slots.
 *
 * Note: this MUST happen after we update the master/replica state
 * so that CLUSTER_NODE_MASTER flag will be set. */
```

I commenti di checklist sono molto comuni all'interno del kernel Linux, dove l'ordine di certe operazioni è estremamente importante.

### COMMENTI GUIDA

Abuso dei commenti guida a un livello tale che probabilmente la maggior parte dei commenti in Redis sono commenti guida. Inoltre, i commenti guida sono esattamente ciò che molte persone credono essere commenti completamente inutili.

* Non affermano ciò che non è chiaro dal codice.

* Non ci sono suggerimenti di design nei commenti guida.

I commenti guida fanno una sola cosa: coccolano il lettore, lo assistono mentre elabora ciò che è scritto nel codice sorgente fornendo una chiara divisione, ritmo, e introducendo ciò che stai per leggere.

L'unica ragione di esistere dei commenti guida è ridurre il carico cognitivo del programmatore che legge del codice.

**rax.c:**
```c
/* Call the node callback if any, and replace the node pointer
 * if the callback returns true. */
if (it->node_cb && it->node_cb(&it->node))
    memcpy(cp,&it->node,sizeof(it->node));

/* For "next" step, stop every time we find a key along the
 * way, since the key is lexicographically smaller compared to
 * what follows in the sub-children. */
if (it->node->iskey) {
    it->data = raxGetData(it->node);
    return 1;
}
```

Non c'è nulla che i commenti aggiungano al codice sopra. I commenti guida sopra ti assisteranno nella lettura del codice, inoltre ti confermeranno che lo stai comprendendo correttamente. Altri esempi.

**networking.c:**
```c
/* Log link disconnection with replica */
if ((c->flags & CLIENT_SLAVE) && !(c->flags & CLIENT_MONITOR)) {
    serverLog(LL_WARNING,"Connection with replica %s lost.",
        replicationGetSlaveName(c));
}

/* Free the query buffer */
sdsfree(c->querybuf);
sdsfree(c->pending_querybuf);
c->querybuf = NULL;

/* Deallocate structures used to block on blocking ops. */
if (c->flags & CLIENT_BLOCKED) unblockClient(c);
dictRelease(c->bpop.keys);

/* UNWATCH all the keys */
unwatchAllKeys(c);
listRelease(c->watched_keys);

/* Unsubscribe from all the pubsub channels */
pubsubUnsubscribeAllChannels(c,0);
pubsubUnsubscribeAllPatterns(c,0);
dictRelease(c->pubsub_channels);
listRelease(c->pubsub_patterns);

/* Free data structures. */
listRelease(c->reply);
freeClientArgv(c);

/* Unlink the client: this will close the socket, remove the I/O
 * handlers, and remove references of the client from different
 * places where active clients may be referenced. */
unlinkClient(c);
```

Redis è *letteralmente* pieno di commenti guida, quindi praticamente ogni file che apri conterrà molti di essi. Perché preoccuparsene? Di tutti i tipi di commento che ho analizzato finora in questo post sul blog, ammetto che questo è assolutamente il più soggettivo. Non valuto il codice senza tali commenti come meno buono, eppure credo fermamente che se le persone considerano il codice di Redis leggibile, parte della ragione è a causa di tutti i commenti guida.

I commenti guida hanno qualche utilità oltre a quelle dichiarate. Poiché dividono chiaramente il codice in sezioni isolate, un'aggiunta al codice è molto probabile che venga inserita nella sezione appropriata, invece di finire in qualche parte casuale. Avere dichiarazioni correlate vicine è un grande vantaggio per la leggibilità.

Assicurati anche di controllare il commento guida sopra prima che venga chiamata la funzione unlinkClient(). Il commento guida dice brevemente al lettore cosa farà la funzione, evitando la necessità di tornare indietro nella funzione se sei interessato solo al quadro generale.

### COMMENTI BANALI

I commenti guida sono strumenti molto soggettivi. Potresti apprezzarli o no. Io li adoro. Tuttavia, un commento guida può degenerare in un commento molto brutto: può facilmente trasformarsi in un "commento banale". Un commento banale è un commento guida in cui il carico cognitivo della lettura del commento è uguale o superiore a quello di leggere semplicemente il codice associato. La seguente forma di commento banale è esattamente ciò che molti libri ti diranno di evitare.

    array_len++;    /* Incrementa la lunghezza del nostro array. */

Quindi, se scrivi commenti guida, assicurati di evitare di scrivere quelli banali.

### COMMENTI DI DEBITO

I commenti di debito sono dichiarazioni di debiti tecnici codificati direttamente all'interno del codice sorgente stesso:

**t_stream.c:**
```c
/* Here we should perform garbage collection in case at this point
 * there are too many entries deleted inside the listpack. */
entries -= to_delete;
marked_deleted += to_delete;
if (entries + marked_deleted > 10 && marked_deleted > entries/2) {
    /* TODO: perform a garbage collection. */
}
```

Lo snippet sopra è estratto dall'implementazione degli stream di Redis. Gli stream di Redis permettono di eliminare elementi dal mezzo usando il comando XDEL. Questo può essere utile in diversi modi, specialmente nel contesto delle normative sulla privacy dove certi dati non possono essere conservati indipendentemente dalla struttura dati o dal sistema che stai utilizzando per memorizzarli. È un caso d'uso molto strano per una struttura dati principalmente di tipo append-only, ma se gli utenti iniziano a eliminare più del 50% degli elementi nel mezzo, lo stream inizia a frammentarsi, essendo composto da "macro nodi". Le voci vengono solo contrassegnate come eliminate, ma vengono recuperate solo una volta che tutte le voci in un dato macro nodo sono liberate. Quindi le tue eliminazioni di massa cambieranno il comportamento della memoria degli stream.

Al momento, questo sembra un non-problema, poiché non mi aspetto che gli utenti eliminino la maggior parte della cronologia in uno stream. Tuttavia, è possibile che in futuro potremmo voler introdurre la garbage collection: il macro nodo potrebbe essere compattato una volta che il rapporto tra le voci eliminate e le voci esistenti raggiunge un certo livello. Inoltre, i nodi vicini potrebbero essere incollati insieme dopo la garbage collection. Ero un po' preoccupato che in seguito non avrei più ricordato quali fossero i punti di ingresso per fare la garbage collection, quindi ho messo commenti TODO, e ho persino scritto la condizione di trigger.

Questo probabilmente non è fantastico. Un'idea migliore era invece scrivere, nel commento di design all'inizio del file, perché attualmente non stiamo eseguendo la GC. E quali sono i punti di ingresso per la GC, se vogliamo aggiungerla in seguito.

FIXME, TODO, XXX, "Questo è un hack", sono tutte forme di commenti di debito. Non sono fantastici in generale, cerco di evitarli, ma non è sempre possibile, e a volte invece di dimenticare per sempre un problema, preferisco mettere una nota all'interno del codice sorgente. Almeno si dovrebbe periodicamente fare un grep per tali commenti, e vedere se è possibile mettere le note in un posto migliore, o se il problema non è più rilevante o potrebbe essere risolto subito.

### COMMENTI DI BACKUP

Infine, i commenti di backup sono quelli in cui lo sviluppatore commenta versioni più vecchie di un blocco di codice o anche di un'intera funzione, perché non è sicuro del cambiamento che è stato operato nel nuovo. Ciò che è sconcertante è che questo accade ancora ora che abbiamo Git. Immagino che le persone abbiano una sensazione di disagio nel perdere quel frammento di codice, considerato più sano o stabile, in qualche commit vecchio di anni.

Ma il codice sorgente non è per fare backup. Se vuoi salvare una versione più vecchia di una funzione o parte di codice, il tuo lavoro non è finito e non può essere committato. O assicurati che la nuova funzione sia migliore della precedente, o tienila solo nel tuo albero di sviluppo finché non sei sicuro.

I commenti di backup concludono la mia classificazione. Proviamo a trarre qualche conclusione.

## I commenti come strumento di analisi

I commenti sono il debugging con il papero di gomma sotto steroidi, tranne che non stai parlando con un papero di gomma, ma con il futuro lettore del codice, il che è più intimidatorio di un papero di gomma, e può usare Twitter. Quindi nel processo cerchi davvero di capire se ciò che stai affermando *è accettabile*, onorevole, abbastanza buono. E se non lo è, fai i tuoi compiti, e trovi qualcosa di più decente.

È lo stesso processo che accade mentre si scrive la documentazione: lo scrittore tenta di fornire il succo di ciò che fa un dato pezzo di codice, quali sono le garanzie, gli effetti collaterali. Questa è spesso un'opportunità di caccia ai bug. È molto facile, mentre descrivi qualcosa, scoprire che ha dei buchi... Non puoi davvero descriverlo tutto perché non sei sicuro di un dato comportamento: tale comportamento emerge semplicemente dalla complessità, a caso. Non vuoi davvero questo, quindi torni indietro e sistemi tutto. Trovo questa una splendida ragione per scrivere commenti.

## Scrivere buoni commenti è più difficile che scrivere buon codice

Potresti pensare che scrivere commenti sia una forma di lavoro meno nobile. Dopotutto, *sai programmare*! Tuttavia considera questo: il codice è un insieme di dichiarazioni e chiamate di funzione, o qualunque sia il tuo paradigma di programmazione. A volte tali dichiarazioni non hanno molto senso, onestamente, se il codice non è buono. I commenti richiedono sempre di avere un processo di design in corso, e di capire il codice che stai scrivendo in un senso più profondo. Inoltre, per scrivere buoni commenti, devi sviluppare le tue capacità di scrittura. Le stesse capacità di scrittura ti assisteranno nella scrittura di email, documentazione, documenti di design, post sul blog e messaggi di commit.

Scrivo codice perché ho un senso urgente di condividere e comunicare più di ogni altra cosa. I commenti coadiuvano il codice, lo assistono, descrivono i nostri sforzi, e dopotutto amo scriverli tanto quanto amo scrivere il codice stesso.

(Grazie a Michel Martens per aver fornito feedback durante la scrittura di questo post sul blog)
