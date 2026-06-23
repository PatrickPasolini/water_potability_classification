# water_potability_classification

**Autore:** Patrick Pasolini Vitali **Contesto:** Progetto sviluppato per il corso di Machine Learning and Data Mining 

## 💧 Obiettivo del Progetto

Questo progetto affronta un problema di classificazione binaria nel dominio della qualità dell'acqua: l'obiettivo è prevedere se un campione d'acqua sia potabile o non potabile a partire da una serie di misurazioni chimico-fisiche. La valutazione automatica della potabilità è un task di grande rilevanza pratica, utile per supportare il monitoraggio ambientale e la tutela della salute pubblica.

## 📊 Dataset

Il dataset utilizzato è composto da 3276 istanze e comprende 9 feature numeriche continue e una variabile target binaria:

* **Features:** `pH`, `Hardness`, `Solids` (TDS), `Chloramines`, `Sulfate`, `Conductivity`, `Organic carbon` (TOC), `Trihalomethanes` (THM), `Turbidity`.
* **Target (`Potability`):** Valore `1` (potabile) e `0` (non potabile).



## 🔬 Metodologia e Preprocessing

L'intero progetto è stato guidato da un rigoroso principio metodologico volto a evitare il fenomeno del *data leakage* (ovvero il trasferimento involontario di informazioni dal test set ai dati di addestramento).

* **Analisi Esplorativa (EDA):** Ha messo in luce che le variabili sono scarsamente correlate tra loro e la loro correlazione con il target è praticamente nulla. Le distribuzioni delle due classi risultano, inoltre, completamente sovrapposte su tutte le feature.


* **Data Splitting:** La suddivisione in training set (75%) e test set (25%) è stata eseguita in modalità stratificata **prima** di avviare qualsiasi operazione di stima o pulizia dei dati.


* **Uso delle Pipeline:** Ogni step di preprocessing è stato incapsulato in una `Pipeline` di Scikit-Learn. Questo assicura che trasformazioni come l'imputazione e lo scaling vengano ricalcolate solo sulla porzione di training in ogni iterazione della cross-validation.


* **Gestione Valori Mancanti e Outlier:** I *missing values* sono stati imputati utilizzando la mediana globale. Per uniformare le scale e gestire la presenza di outlier, è stato applicato il `RobustScaler`.


* **Sbilanciamento delle Classi:** Il dataset presenta il 61% di istanze non potabili e il 39% potabili. Il problema è stato gestito bilanciando i pesi nella funzione di costo (`class_weight='balanced'`) o tramite tecniche di oversampling (SMOTE) applicate esclusivamente ai dati di addestramento.

## 🤖 Modelli Addestrati

Dopo uno screening iniziale che ha confermato la debolezza dei modelli lineari per questo tipo di problema , sono stati selezionati e ottimizzati tramite *Grid Search CV* e *Optuna* i seguenti modelli non lineari:

* **Random Forest** 
* **XGBoost** 
* **Support Vector Machine (SVM) con kernel RBF** 
* **Multi Layer Perceptron (MLP)** 
* **Rete Neurale Profonda in Keras (ottimizzata con Optuna)** 

## 📈 Risultati e Conclusioni

I risultati hanno dimostrato che tutti i modelli raggiungono prestazioni simili, stabilizzandosi attorno a un valore di F1 macro di circa 0.63 sul test set. Tra questi, il modello **Random Forest bilanciato** ha offerto il miglior compromesso complessivo tra cross-validation e test (F1 macro test: 0.629).

L'analisi ha portato a un'importante conclusione: **il dataset presenta un limite intrinseco** (con molta probabilità si tratta di dati sintetici) e non possiede sufficiente informazione per costruire una regola di classificazione realmente generalizzabile.

Il reale valore di questo progetto non risiede nell'aver raggiunto una percentuale di accuratezza illusoria, bensì nel **rigore analitico e metodologico** impiegato. Grazie all'uso corretto di *split preventivi* e *pipeline*, è stato possibile dimostrare l'effettiva assenza di segnale nei dati, smascherando i risultati ottimistici (con accuratezze fino all'81%) presenti in altri studi che, al contrario, soffrono di grave *data leakage* a causa di imputazioni svolte incautamente prima di dividere il dataset.
