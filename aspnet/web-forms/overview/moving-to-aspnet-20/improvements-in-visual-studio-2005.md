---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Miglioramenti in Visual Studio 2005 Documenti Microsoft
author: rick-anderson
description: Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: e98771614bf4e0095f8ff596e7cdb26c8c9de1ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542989"
---
# <a name="improvements-in-visual-studio-2005"></a>Miglioramenti in Visual Studio 2005

da parte [di Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web.

Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web. Per quanto potenti siano Visual Studio .NET 2002 e 2003, ci sono state molte lamentele nel modo in cui i progetti Web sono stati gestiti. Visual Studio 2005 aggiunge un numero significativo di nuove funzionalità per risolvere questi reclami. Per coloro che preferiscono il modo in cui Visual Studio .NET 2003 ha gestito la compilazione delle applicazioni Web, vedere [Progetti di applicazione Web](https://go.microsoft.com/fwlink/?LinkId=57870).

In questo modulo vengono illustrati i miglioramenti apportati alla creazione, alla gestione e allo sviluppo di progetti Web. In un modulo successivo vengono illustrati i miglioramenti apportati alla creazione di progetti Web e alla distribuzione.

## <a name="frontpage-server-extensions"></a>Estensioni del server di FrontPage

Visual Studio .NET 2002 e 2003 richiedevano le estensioni del server di FrontPage per creare o compilare progetti Web. Gli sviluppatori avevano una scelta tra due diverse modalità di accesso (FrontPage Server Extensions o Modalità di accesso ai file), entrambe utilizzavano le estensioni del server di FrontPage per eseguire attività quali l'impostazione della radice dell'applicazione in IIS e così via.

Visual Studio 2005 rimuove la dipendenza dalle estensioni del server di FrontPage per i progetti locali. Visual Studio 2005 accede ora direttamente alla metabase di IIS anziché utilizzare le estensioni del server di FrontPage. Visual Studio 2005 aggiunge inoltre il supporto per FTP che consente l'accesso remoto al progetto senza richiedere le estensioni del server di FrontPage.

Per gli sviluppatori che desiderano utilizzare le estensioni del server di FrontPage nei propri progetti, l'opzione è ancora disponibile. Tuttavia, sulla base di un forte feedback da parte della ASP.NET comunità di sviluppatori, non è un requisito.

> [!NOTE]
> Le estensioni del server di FrontPage sono ancora necessarie per la creazione, l'apertura e così via di progetti remoti.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Visual Studio 2005 viene fornito con un nuovo server Web denominato ASP.NET development Server. (Questo server Web era precedentemente noto come Cassini.)

Il ASP.NET Development Server offre diversi vantaggi.

- È ora possibile per gli utenti non amministratori sviluppare ed eseguire il debug su un server Web.
- Il server di sviluppo ASP.NET esegue dinamicamente il mapping delle directory virtuali a qualsiasi posizione nel file system, consentendo percorsi di progetto flessibili.
- Gli utenti di Windows XP Professional che utilizzano già IIS potranno creare nuove applicazioni Web che non influiranno sulla struttura di file o cartelle del sito Web predefinito in IIS.

Non è necessaria alcuna configurazione speciale per sfruttare i vantaggi del server di sviluppo ASP.NET. Quando viene eseguito il debug o l'esplorazione di un progetto Web ospitato nel file system, Visual Studio 2005 avvierà automaticamente un'istanza del server di sviluppo ASP.NET su una porta casuale per soddisfare la richiesta.

Ulteriori informazioni verranno trattate nel server di sviluppo ASP.NET più avanti in questo modulo.

## <a name="improved-file-management"></a>Gestione dei file migliorata

In Visual Studio 2002 e 2003, un file di progetto (con estensione vbproj per VB.NET e csproj per C) archiviava le informazioni su tutti i file dell'applicazione Web. La visualizzazione di Esplora soluzioni si basa sulle informazioni sul file nel file di progetto. Per questo motivo, Esplora soluzioni spesso visualizza informazioni imprecise nei casi in cui sono stati utilizzati editor esterni. Visual Studio 2002 e 2003 spesso sovrascrivono le modifiche ai file o non visualizzano la versione più recente dei file.

Visual Studio 2005 non rispetta il file di progetto. Legge invece le informazioni su file e cartelle direttamente dal disco, ottenendo una visualizzazione accurata dei file nel progetto. Poiché la cartella Riferimenti in Visual Studio 2002 e 2003 non rappresenta una cartella effettiva nell'applicazione Web, Visual Studio 2005 rimuove anche la cartella Riferimenti da Esplora soluzioni. Per accedere ai riferimenti per il progetto in Visual Studio 2005, è necessario utilizzare le pagine delle proprietà per il progetto.

## <a name="creating-web-projects"></a>Creazione di progetti Web

Gli sviluppatori Web hanno molte nuove opzioni disponibili per la creazione di progetti in Visual Studio 2005. I siti Web possono ora essere creati in qualsiasi punto del file system e possono quindi essere sottoposti a debug o sfogliati utilizzando il nuovo server di sviluppo ASP.NET. Gli sviluppatori possono anche creare nuovi siti Web tramite FTP.

Fare clic qui per visualizzare una procedura dettagliata video sulla creazione di progetti Web in Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Progetti del file system

Come illustrato nella procedura dettagliata video, è possibile scegliere di creare siti Web nel file system nel computer locale o in una posizione remota tramite una condivisione file. I siti Web creati nel file system vengono esplorati e sottoposti a debug utilizzando il server di sviluppo ASP.NET.

> [!NOTE]
> Il ASP.NET Development Server può causare confusione per i clienti. Se un progetto Web viene creato nel file system nella struttura di directory IISs (ad esempio c:/inetpub/wwwroot), il sito Web verrà comunque esplorato tramite il ASP.NET Development Server quando viene avviato dall'interno di Visual Studio 2005. Pertanto, qualsiasi configurazione IIS (ad esempio metodi di autenticazione) non è applicabile.

Il progetto Web predefinito rimuove inoltre gran parte dell'overhead includendo solo una pagina Default.aspx, default.cs file e una cartella App/_Data. Il file web.config e le cartelle speciali (ad esempio app/_code) vengono aggiunti in base alle esigenze. Il progetto Web include solo i file e le cartelle necessari.

### <a name="http-projects"></a>Progetti HTTP

I progetti HTTP possono essere progetti creati in un sito Web IIS locale o in un sito Web remoto. Il percorso predefinito `http://localhost`del progetto è . Se si fa clic sul pulsante Sfoglia, sono disponibili due opzioni HTTP: IIS locale e Sito remoto. La differenza principale in queste due opzioni è il metodo in cui le informazioni del sito Web vengono visualizzate nella finestra di dialogo Scegli percorso e nella modalità di copia dei file nel server Web.

L'opzione IIS locale legge le informazioni sul sito dalla metabase nel computer locale e i file vengono copiati utilizzando il file system. L'opzione Sito remoto utilizza le estensioni del server di FrontPage e le informazioni e i file del sito vengono copiati utilizzando le chiamate RPC HTTP e delle estensioni del server di FrontPage.

> [!NOTE]
> Per determinare le informazioni sulla versione non vengono più utilizzati il file data _tmp.htm e get/_aspx/_ver.aspx.

L'opzione HTTP predefinita è IIS locale. Questa opzione legge la metabase di IIS per determinare quali siti sono disponibili e il percorso in cui creare il contenuto. È possibile selezionare una cartella o una directory virtuale diversa selezionandola nella visualizzazione albero. È inoltre possibile creare una nuova directory virtuale, contrassegnare le cartelle come applicazioni, nonché eliminare le directory virtuali esistenti da questa finestra di dialogo.

![Finestra di dialogo Scegli posizione](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: Finestra di dialogo Scegli posizione

A differenza delle versioni precedenti di Visual Studio, se si seleziona la casella di controllo **Usa Secure Sockets Layer** e il certificato SSL non corrisponde all'URL che si sta esplorando, verrà visualizzata una finestra di dialogo Avviso di sicurezza in cui viene chiesto se si desidera procedere. Utilizzando Visual Studio .NET 2003, se il certificato non è corrispondente, la creazione del progetto avrà esito negativo.

![Avviso di sicurezza relativo al certificato SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2:** Avviso di sicurezza relativo al certificato SSL

### <a name="note-on-host-headers"></a>Nota sulle intestazioni host

Se si crea un'applicazione Web in un sito associato a un indirizzo IP specifico, è necessario assicurarsi che sia configurata un'intestazione host. In caso contrario, Visual `http://localhost`Studio creerà il sito in , ma l'indirizzo IP non verrà risolto correttamente quando il sito viene esplorato o sottoposto a debug dall'interno dell'IDE.

Se si seleziona l'opzione Sito remoto, la finestra di dialogo cambia per consentire l'immissione dell'URL di destinazione per il nuovo sito Web. Questo URL deve trovarsi in un server in cui sono abilitate le estensioni del server di FrontPage. Se si desidera utilizzare il server Web locale utilizzando le estensioni del server di FrontPage, è possibile utilizzare l'opzione Sito remoto e specificare un URL locale.

![Creazione di un sito Web in un server remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3:** Creazione di un sito Web su un server remoto

Quando si crea un'applicazione in un sito remoto tramite SSL, se il certificato SSL non corrisponde, la finestra di dialogo di conferma è leggermente diversa dalla finestra di dialogo visualizzata quando si utilizza l'opzione IIS locale.

![Avviso di sicurezza del sito remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4:** Avviso di sicurezza del sito remoto

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 introduce l'opzione per creare siti Web tramite FTP. Quando si utilizza questa opzione, l'IDE crea i file localmente nella cartella temporanea degli utenti e quindi utilizza FTP per spostare i file nel percorso FTP.

> [!NOTE]
> Il percorso della cartella temporanea è&lt;c:/Documents and Settings/ User&gt;&lt;/Local Settings/Temp/VWDWebCache/ Server&gt;/_&lt;nome dell'applicazione&gt;

Quando si utilizza l'opzione FTP, verrà visualizzata una finestra di dialogo Scegli posizione. Immettere le informazioni di connessione FTP necessarie in questa finestra di dialogo come illustrato di seguito.

![Finestra di dialogo Scegli posizione per FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: La finestra di dialogo Scegli posizione per FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Lab: configurare il sito FTP e creare un progetto

La procedura seguente consente di configurare il sito FTP in modo che un utente disponga di un percorso in cui solo loro può essere caricato tramite FTP.

### <a name="install-the-ftp-service"></a>Installare il servizio FTP

1. Aprire Installazione applicazioni, selezionare Installazione componenti di Windows
2. Selezionare Internet Information Services (Server applicazioni in Windows 2003) e fare clic su **Dettagli**.
3. Controllare **il servizio FTP (File Transfer Protocol)** e fare clic su **OK**.
4. Fare clic su **Avanti** per installare il servizio FTP.

### <a name="create-a-new-folder-for-content"></a>Creazione di una nuova cartella per il contenuto

1. In Esplora risorse creare una nuova cartella denominata **User1** all'interno di c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurare le cartelle e le autorizzazioni per le cartelle.

1. Aprire lo snap-in Internet Information Services da Strumenti di amministrazione. Si dia ora una cartella Siti FTP sotto il nodo del nome del computer.
2. Espandere **Siti FTP**.
3. Fare clic con il pulsante destro del mouse sul **sito FTP predefinito**, selezionare **Nuovo**, quindi **Directory virtuale**, quindi fare clic su **Avanti**.
4. Immettere **User1** come nome della directory virtuale e fare clic su **Avanti**.
5. Immettere **c:/inetpub/wwwroot/User1** per il percorso e fare clic su **Avanti**.
6. Fare clic su **Avanti** e quindi **su Fine** per completare la procedura guidata.
7. Fare clic con il pulsante destro del mouse sulla directory virtuale **User1** in Sito FTP predefinito e scegliere **Proprietà**.
8. Selezionare la casella di controllo **Scrivi** e fare clic **su OK** per chiudere la finestra di dialogo.
9. Fare clic con il pulsante destro del mouse su **Sito FTP predefinito** e selezionare **Proprietà**.
10. Nella scheda **Account di protezione** deselezionare Consenti connessioni **anonime**.
11. Fare clic su **Sì** nella finestra di dialogo in cui viene chiesto se si desidera continuare.
12. Fare clic su **OK** per chiudere la finestra di dialogo.
13. Espandere il **sito Web predefinito** nel nodo Siti **Web.**
14. Fare clic con il pulsante destro del mouse sulla directory **User1** e selezionare **Proprietà**
15. Nella sezione **Impostazioni applicazione** fare clic su **Crea** per contrassegnare la cartella come applicazione.
16. Fare clic su **OK** per chiudere la finestra di dialogo.
17. Chiudere lo snap-in Internet Information Services.

### <a name="create-web-project"></a>Creare un progetto Web

1. Aprire Visual Studio 2005.
2. Scegliere Nuovo sito **Web**dal menu **File** .
3. Nell'elenco a discesa **Posizione,** selezionare **FTP**.
4. Fare clic su **Sfoglia**.
5. Immettere **localhost** nella casella di testo **Server.**
6. Immettere **Utente1** nella casella di testo Directory.
7. Fare clic su **Apri**. Il percorso FTP verrà inserito nella finestra di dialogo Nuovo sito Web.
8. Fare clic su **OK**.
9. Deselezionare **Accesso anonimo** nella finestra di dialogo Di accesso FTP, immettere le credenziali e fare clic su **OK**.
10. Qual è l'URL del progetto? L'URL del progetto verrà visualizzato in Esplora soluzioni.
11. Scegliere Compila sito **Web** o **Compila soluzione**dal menu **Compila** .
12. Fare clic con il pulsante destro del mouse su Default.aspx in Esplora soluzioni e selezionare **Visualizza nel browser**.
13. Nella finestra di dialogo `http://localhost/user1` URL sito Web obbligatorio immettere l'URL e fare clic su **OK**.

> [!NOTE]
> Se viene visualizzato un errore che indica un impossibilità di caricare il tipo /_Default, assicurarsi che l'esecuzione di ASP.NET 2.0 nel sito Web e non di una versione precedente. È possibile farlo dalla scheda ASP.NET in Internet Information Services.

## <a name="opening-web-projects"></a>Apertura di progetti Web

L'apertura di progetti Web è simile alla creazione di progetti. Le sezioni seguenti richiamano le aree da tenere d'occhio mentre si lavora all'interno dell'IDE. Viene inoltre illustrato l'utilizzo di progetti Web tramite HTTP e FTP.

Per aprire un progetto Web, scegliere Apri sito Web dal menu File. Verrà visualizzata la stessa finestra di dialogo Scegli percorso descritta in precedenza e sono disponibili le stesse quattro opzioni: File System, IIS locale, FTP e Sito remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>File system

Come indicato in precedenza in questo modulo, Visual Studio non utilizza più un file di progetto. Pertanto, se si sceglie di aprire un sito Web dal file system, è effettivamente possibile scegliere la cartella desiderata, anche se la cartella scelta non è stata creata inizialmente come progetto Web in Visual Studio. Ad esempio, è possibile scegliere di aprire la cartella Documenti come sito Web e Visual Studio verrà fortunatamente aprire e visualizzare i file come illustrato di seguito.

![Documenti personali aperti come sito Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Documenti* aperti come sito Web

Poiché Visual Studio crea file e cartelle aggiuntivi solo quando necessario, non vengono aggiunti file o cartelle aggiuntivi al percorso aperto. Un effetto collaterale di questa architettura è che impedisce la nidificazione di siti Web nel file system. Si consideri, ad esempio, la struttura di directory seguente.

Web project presso C:/MyWebSite

Un altro progetto web su C:/MyWebSite/Nested

Quando si apre il sito Web in c:/MyWebSite, la cartella Nested verrà visualizzata come sottocartella di tale applicazione.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Quando si aprono siti Web tramite HTTP, le impostazioni vengono lette dalla metabase di IIS (IIS locale) o tramite le estensioni del server di FrontPage (sito remoto). Se sono presenti applicazioni Web annidate, anche queste vengono visualizzate con un'icona che le identifica come applicazione. Se si ha familiarità con l'utilizzo di applicazioni Web in FrontPage, il comportamento in Visual Studio 2005 è simile.

Anche se Visual Studio verrà visualizzata un'icona per le applicazioni annidate sotto l'applicazione attualmente aperta all'interno dell'IDE, non sarà possibile espanderli per visualizzare il contenuto. È possibile, tuttavia, fare doppio clic su di essi per aprirli. In questo caso, verrà visualizzata una finestra di dialogo che richiede di aprire l'applicazione web (e sostituire la soluzione attualmente aperta) o aggiungere l'applicazione Web alla soluzione corrente.

![Facendo doppio clic sull'icona di un'applicazione nidificata si visualizza questa finestra di dialogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: Facendo doppio clic sull'icona di un'applicazione nidificata viene visualizzato questa finestra di dialogo

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sito FTP

Quando si apre un sito tramite FTP, i file vengono tutti copiati localmente nella cartella temporanea. Il percorso completo per il percorso di archiviazione locale viene visualizzato nel riquadro Proprietà per il progetto e viene creato utilizzando il formato seguente.

C:/Documents and&lt;Settings/ Utente&gt;/Impostazioni locali/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nome applicazione&gt;

Quando si usa FTP, Visual Studio dovrà specificare l'URL di base per il progetto in modo da poterlo esplorare come illustrato di seguito. Se non si specifica un URL di base, Visual Studio lo richiederà la prima volta che si tenta di esplorare una pagina nel sito Web.

![Specifica di un URL di base per i siti FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: Specifica di un URL di base per i siti FTP

## <a name="improvements-in-compilation"></a>Miglioramenti nella compilazione

L'utilizzo di applicazioni Web in Visual Studio 2005 è notevolmente più veloce rispetto alle versioni precedenti. Ciò è dovuto in non poco alle modifiche nell'architettura di compilazione.

In Visual Studio 2002 e 2003 le applicazioni Web venivano compilate in un unico assembly primario che si trova nella cartella /bin. In Visual Studio 2005 è stata aggiunta una cartella App/_Code. Le classi e altro codice non dell'interfaccia utente vengono aggiunti alla cartella App/_Code. Quando Visual Studio compila il progetto, tutti i file nella cartella App/_Code vengono compilati in un singolo file App/_Code.dll. Il risultato di questa modifica è che le compilazioni successive sono molto più veloci rispetto alle versioni precedenti.

> [!NOTE]
> L'utilità della riga di comando MSBuild può essere utilizzata anche per compilare applicazioni Web ASP.NET. Tale strumento sarà coperto nel modulo 9.

Un altro miglioramento della compilazione è la nuova opzione Pagina di compilazione del menu Compila.Another compilation enhancement is the new Build Page option on the Build menu. Questa funzionalità consente a uno sviluppatore di ricostruire solo la pagina corrente (insieme, naturalmente, e le dipendenze) in modo che le modifiche possono essere compilate più rapidamente. Dal punto di controllo, in C, non è in grado di offrire la compilazione in background ai fini dell'aggiornamento di IntelliSense e così via, ne trarranno immensamente vantaggio da questa funzionalità perché consentirà di aggiornare rapidamente IntelliSense ricompilando una singola pagina.

Le proprietà di compilazione per un progetto consentono di configurare il tipo di compilazione che si verifica prima dell'esecuzione della pagina di avvio. Gli sviluppatori possono scegliere di compilare solo la pagina corrente in modo che Visual Studio possa avviare il debug delle applicazioni più rapidamente dopo le modifiche al codice.

![Azione di avvio della pagina di compilazioneThe Build Page Start Action](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: Azione di avvio della pagina di compilazione

Un altro grande miglioramento di Visual Studio e l'architettura ASP.NET è nell'area di modifica e continuazione. In Visual Studio 2005, gli sviluppatori possono avviare il debug di un progetto e apportare modifiche al codice nel progetto senza disconnettere il debugger. Infatti, è possibile avviare letteralmente il debug di un progetto, aggiungere una nuova classe, aggiungere codice a tale classe, aggiungere codice alla pagina che crea una nuova istanza di tale classe ed eseguire un metodo della classe, il tutto senza scollegare il debugger. L'esecuzione del nuovo codice è letteralmente facile come aggiornare il browser!

Fare clic qui per visualizzare una procedura dettagliata video della funzionalità di modifica e continuazione in Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La robusta funzionalità di modifica e continuazione in ASP.NET 2.0 e Visual Studio 2005 è dovuta a una modifica dell'architettura per applicazioni ASP.NET. In ASP.NET 1.x le applicazioni create in Visual Studio 2002/2003 sono state compilate in un assembly primario archiviato nella cartella /bin. Tutte le classi, le pagine e così via per l'applicazione sono state compilate in tale DLL. Quindi, in fase di esecuzione, ASP.NET avrebbe compilato tutti i controlli, markup e ASP.NET codice all'interno delle pagine e copiare tali DLL nella cartella temporanea ASP.NET.

In Visual Studio 2005 utilizzando ASP.NET 2.0, i due modelli di compilazione descritti in precedenza (uno per Visual Studio e uno per ASP.NET in fase di esecuzione) sono stati uniti in un unico modello di compilazione comune. Ciò significa che tutti i problemi di compilazione vengono ora rilevati durante la fase di sviluppo anziché in fase di esecuzione. Consente inoltre la finestra di progettazione e il supporto IntelliSense per funzionalità quali controlli utente e pagine master.

Fare clic qui per visualizzare una procedura dettagliata video del supporto della finestra di progettazione per i controlli utente.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Quando un controllo utente viene rimosso @Register da una pagina, la direttiva rimane nel markup e deve essere rimossa manualmente per evitare errori del parser se il controllo utente viene eliminato dal sito Web.

Un altro miglioramento nel modello di compilazione di Visual Studio è la funzionalità Pubblica sito Web. Poiché la funzionalità Pubblica precompila il sito Web, gli sviluppatori possono usufruire delle prestazioni aggiuntive di non dover compilare nulla su richiesta. Precompila inoltre tutto il codice sorgente nella cartella App/_Code in una DLL in modo che non deve essere distribuito alcun codice sorgente.

![Finestra di dialogo Pubblica sito Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: Finestra di dialogo Pubblica sito Web

> [!NOTE]
> L'utilità aspnet/_compile.exe può essere utilizzata anche per precompilare un'applicazione Web ASP.NET. Tale strumento sarà coperto nel modulo 9.

Quando si pubblica un sito Web, i file precompilati vengono archiviati nella cartella File di ASP.NET temporanei, come illustrato di seguito. I file con estensione *.compiled* sono file XML che definiscono le dipendenze per particolari DLL. Tutti i controlli Webform o utente vengono compilati in DLL casuali che iniziano con *App/_Web/_*.

Se si lascia selezionata la casella di controllo *Consenti all'aggiornabile del sito precompilato,* il markup all'interno dei web form e dei controlli utente non verrà precompilato in una DLL che consente di apportare modifiche dopo la distribuzione. Se si preferisce bloccare il markup in modo che le modifiche al contenuto distribuito non siano consentite, deselezionare questa casella.

La casella di controllo *Usa denominazione fissa e assembly* a pagina singola consente di disabilitare la compilazione batch in modo che ogni pagina venga compilata in un assembly con nome fisso. Lasciando questa casella deselezionata è possibile sfruttare i vantaggi della compilazione batch.

La casella di controllo *Abilita denominazione sicura negli assembly precompilati* consente di assegnare un nome sicuro agli assembly precompilati.

> [!NOTE]
> In ASP.NET 1.x, gli assembly con nome sicuro dovevano essere installati nella Global Assembly Cache (GAC). Nella ASP.NET 2.0 non è necessario installare assembly con nome sicuro nella Global Assembly Cache.

![Un ASP.NET applicazioni precompilate file](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: Un ASP.NET applicazioni precompilate file

> [!NOTE]
> Nell'applicazione precedente, non esisteva alcun file web.config. Se ci fosse stato, sarebbe stato chiamato *PrecompiledApp.config* dopo il processo di pubblicazione del sito Web.

## <a name="improvements-in-deployment"></a>Miglioramenti nella distribuzione

Come con Visual Studio 2002 e 2003, Visual Studio 2005 offre una funzionalità Copia progetto. Tuttavia, la funzionalità è stata integrata in Visual Studio 2005 ed è ora denominata Copia sito Web.

La finestra di dialogo Copia sito Web viene suddivisa in un frame sinistro e un frame destro. Il frame sinistro è denominato sito Web di origine e il frame destro è denominato sito Web remoto. Una cosa che può confondere alcuni sviluppatori è che il sito visualizzato nel frame di destra non è necessariamente un sito remoto. Potrebbe essere un sito nel file system locale o nell'istanza locale di IIS. Inoltre, il sito visualizzato nel frame a sinistra non è necessariamente il sito Web di origine perché la finestra di dialogo consente di pubblicare dal sito Web remoto *al* sito Web di origine.

Se si copia un progetto in un sito Web remoto, è necessario che in tale sito siano installate le estensioni del server di FrontPage. In caso contrario, sarà necessario connettersi tramite FTP. D'altra parte, se si copia un progetto nell'istanza IIS locale, le estensioni del server di FrontPage non sono necessarie.

> [!NOTE]
> Se si tenta di creare un nuovo sito Web nell'istanza IIS locale e sono installate le estensioni del server di FrontPage 2002, verrà visualizzato un messaggio di errore che informa che la creazione di siti Web non è supportata in un server SharePoint. In tal caso, è possibile installare le estensioni del server di FrontPage 2000 o rimuovere le estensioni del server di FrontPage.

Fare clic qui per una procedura dettagliata video della funzionalità Copia sito Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Miglioramenti nel debugImprovements in Debugging

Esistono quattro miglioramenti chiave nel debug in Visual Studio 2005.

- È possibile eseguire il debug in locale come utente non amministratore.
- L'attributo Debug per l'elemento Compilation è ora false per impostazione predefinita.
- L'impostazione e la configurazione del debug remoto sono più semplici di prima.
- È ora possibile eseguire il debug di un sito Web aperto tramite un percorso FTP.

## <a name="debugging-as-a-non-administrator"></a>Debug come non amministratore

L'aggiunta del ASP.NET Development Server consente agli utenti non amministratori di eseguire facilmente il debug di ASP.NET applicazioni fin dall'inizio. Quando viene eseguito il debug di un'applicazione ASP.NET in esecuzione nel file system locale, Visual Studio avvia il server di sviluppo ASP.NET nel contesto dell'utente connesso. Tale utente può quindi eseguire il debug dell'applicazione senza alcuna configurazione aggiuntiva.

## <a name="debug-is-false-by-default"></a>Il debug è False per impostazione predefinita

In ASP.NET 1.x, l'attributo *debug* nell'elemento *compilation* del file web.config è stato impostato su *true* per impostazione predefinita. È sempre stato consigliabile che gli sviluppatori impostare questo attributo su *false* prima di distribuire un'applicazione nell'ambiente di produzione, ma poiché la maggior parte degli sviluppatori non comprende appieno le conseguenze di lasciare l'attributo di debug impostato su true, hanno semplicemente lasciato così com'è.

Il problema più grave con l'impostazione dell'attributo debug su true è che disabilita il modello di compilazione batch ASP.NET. Pertanto, ogni pagina viene compilata in una DLL separata. Se un'applicazione Web è costituita da migliaia di pagine (non inaudito con qualsiasi mezzo), ciò significa che diverse migliaia di piccole DLL verranno create da tale applicazione. Sebbene queste DLL siano di piccole dimensioni, non vengono caricate in una posizione particolare in memoria. Pertanto, causano la frammentazione nella memoria di sistema e possono contribuire alle occorrenze OutOfMemoryException.Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.

In ASP.NET 2.0, l'attributo debug è impostato su false per impostazione predefinita. Come si è già visto, quando uno sviluppatore esegue il debug di un'applicazione ASP.NET in Visual Studio 2005, viene richiesto di aggiungere un file web.config con il debug abilitato. In questo modo comporta gli stessi svantaggi che erano presenti in ASP.NET 1.x, ma ora lo sviluppatore è chiaramente avvertito che l'attributo deve essere reimpostato su false prima di spostare l'applicazione in produzione.

## <a name="remote-debugging-setup-and-configuration"></a>Installazione e configurazione del debug remoto

In Visual Studio 2002/2003, il debug remoto si basava su Gestione debug computer (mdm.exe) e sul processo vs7jit.exe. Per questo, la risoluzione dei problemi di debug remoto era spesso una scatola nera per i clienti e spesso non era molto meglio per PSS.

Visual Studio 2005 rimuove la dipendenza dai processi mdm.exe e vs7jit.exe. Al contrario, ora utilizza il servizio Monitor di Debug remoto (msvsmon.exe.)

Il requisito per il debug in Visual Studio 2005 in remoto è abbastanza semplice. È necessario eseguire msvsmon.exe sul server remoto prima del debug. È possibile installare Remote Debug Monitor dal CD di Visual Studio oppure è sufficiente eseguire msvsmon.exe da una condivisione senza installare alcun elemento nel server Web.

Quando si esegue msvsmon.exe, è probabile che si lamenterà delle porte bloccate per il debug remoto. Fortunatamente, è possibile sbloccare facilmente le porte da destra all'interno della finestra di dialogo di avviso come illustrato di seguito.

![Notifica del blocco del debug remoto in Windows Firewall](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: Notifica che Windows Firewall sta bloccando il debug remoto

Dopo aver sbloccato le porte necessarie per il debug, verrà visualizzato Remote Debugging Monitor come illustrato di seguito. Da questa interfaccia è possibile monitorare facilmente le connessioni e modificare le autorizzazioni di debug.

![Monitoraggio debug remoto](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13:** Monitor di debug remoto

È anche possibile eseguire il debug remoto di un'applicazione Web aperta tramite FTP. I passaggi sono gli stessi di quelli precedentemente trattati. Tuttavia, è necessario specificare un URL di base per l'esplorazione del progetto FTP come descritto in precedenza in questo modulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Debug remoto con Visual Studio 2005

Questo lab illustra il debug remoto con Visual Studio 2005.

Fare clic qui per una procedura dettagliata video di questo lab.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Questa esercitazione richiede due computer, uno che esegue Visual Studio 2005 e l'altro che esegue IIS 5 o versione successiva.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web nel server remoto.

> [!NOTE]
> È possibile creare il sito Web in un'istanza IIS remota o tramite FTP.

1. Dal server Web remoto, individuare msvsmon.exe nel computer di sviluppo utilizzando un percorso UNC ed eseguirlo.  
 Il percorso predefinito per msvsmon.exe è //server/c/Programmi/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Se viene richiesto di sbloccare le porte per il debug remoto, eseguire questa operazione.
3. Dal computer di sviluppo aprire il code-behind per Default.aspx e impostare un punto di interruzione nel metodo Page/_Load.
4. Avviare il debug dal computer di sviluppo.

È necessario raggiungere il punto di interruzione come previsto.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Come abbiamo già discusso, Visual Studio 2005 viene fornito con un server Web denominato ASP.NET Development Server. Il server di sviluppo ASP.NET viene talvolta definito Cassini. Questo server Web è un mezzo pratico per sfogliare ed eseguire il debug di applicazioni Web in esecuzione sul file system.

Il server di sviluppo ASP.NET è un server Web con restrizioni. Non consente connessioni remote, non consente richieste da parte di alcun utente diverso dall'utente che ha avviato il server Web. Inoltre, non è in grado di servire le pagine ASP. Vengono servite solo ASP.NET risorse e risorse HTML (incluse immagini, file CSS e così via).

Il server di sviluppo ASP.NET può essere avviato tramite la riga di comando eseguendo il file WebDev.WebServer.exe*/*/*/* che si trova in c:/Windows/Microsoft.NET/Framework/v2.0./ / Nella finestra di dialogo seguente vengono visualizzati i parametri disponibili.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**

> [!NOTE]
> Il server di sviluppo ASP.NET non è supportato se avviato in modo esplicito tramite la riga di comando.
