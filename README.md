
<!-- README.md is generated from README.Rmd. Please edit that file -->
StructuRly 0.1.0
================

StructuRly is an R package containing a [shiny](https://shiny.rstudio.com) application to produce detailed and interactive graphs of the results of a Bayesian cluster analysis obtained with the most common population genetic softwares used to investigate population structure, such as [STRUCTURE](https://web.stanford.edu/group/pritchardlab/structure.html) or [ADMIXTURE](http://software.genetics.ucla.edu/admixture/), widely used to infer the admixture ancestry of samples starting from the most common genetic markers such as SNPs, AFLPs, RFLPs and microsatellites (such as SSRs). More generally, StructuRly can generate graphs from any file containing admixture information of each sample (encoded in percentages in a range from 0 to 1). The main purpose for which StructuRly was conceived is to provide researchers with detailed graphical outputs to interpret their statistical results through the use of a software with a user-friendly interface, which can therefore be easily used by those who do not know a programming language. In fact, in a typical StructuRly output, the user will have the possibility to display information about the ID of each sample, the original membership assigned by the researcher to the sampled populations (or sub - populations) and the label relative to the sampling site, a variable, the latter, which is used in software for population analysis to support the data analysis algorithm. Furthermore, interactivity is a typical feature of StructuRly outputs, which allows the user to extrapolate even more information through a single chart.

However, StructuRly presents more different features in order to:

-   upload datasets wit raw genetic data to analyze them through hierarchical cluster analysis algorithms and view and download the dendrograms based on different distance matrices and linkage methods;

-   produce and customize a table ready to be imported into the STRUCTURE software for the Bayesian analysis (detailed references about the structure of the table to import in STRUCTURE and how to perform its main analysis are available at this [link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3665925/));

-   import the results of the STRUCTURE analysis into user's pre-formatted tables, in order to insert detailed information about the samples. Likewise, the user can import into StructuRly the results of the population analysis carried out in the ADMIXTURE software (files in **.Q** and **.fam** format), without having to re-format the output in another software (such as R);

-   produce, in addition to the barplot, also the interactive triangle plot, well known graphical STRUCTUER graphical output. Both graphs can show the admixture ancestry of the samples subdivided in a maximum of 20 different clusters;

-   to visually compare the partition obtained from the hierarchical cluster analysis and the one with the Bayesian analysis through a confusion matric and estimate an agreement value with three different agreement indeces.

Installation
------------

You can install the released version of StructuRly from [GitHub](https://github.com) in R with:

``` r
library(devtools)

install_github(repo = "nicocriscuolo/StructuRly", dependencies = TRUE)
```

Once the package is loaded and the dependencies installed, you can run the software in the default browser through the following functions:

``` r
library(StructuRly)

runStructuRly()
```

StructuRly 0.1.0 works on all types of browsers (Internet Explorer, Safari, Chrome, etc.) and in its current version it can also work locally and then offline. All you have to do is to install the updated versions of [R](https://www.r-project.org) and [RStudio](https://www.rstudio.com).

Data input
----------

StructuRly is divided into three different sections depending on the input file chosen. For any type of file, the **header of each variable is mandatory** and varies according to the type of variable that must be present in the input dataset, in particular:

1.  nella sezione **import raw genetic data** l'utente ha la possibilità di importare il suo file di dati nei formati **.csv** o **.txt**. Questo file può contenere tre variabili opzionali presenti nel seguente ordine e il cui header deve essere esattamente quello indicato di seguito:

-   **Sample\_ID**: è la variabile che contiene gli identificativi di ogni campione, quindi ogni nome presente in questa colonna sarà diverso dall'altro (anche se è buona norma usare solo numeri e lettere, i caratteri dei nomi dei campioni possono essere separati dai seguenti simboli "\_" e "-");

-   **Pop\_ID**: è una variabile categorica identificata da un numero intero che indica la popolazione di appartenenza, definita dall'utente, per ogni campione (e. g.: 1, 2, 3..);

-   **Loc\_ID**: un'altra variabile categorica identificata nuovamente da un numero intero che indica il sito di origine di ogni campione; tale variabile, se presente nella tabella prodotta grazie a StructuRly e quindi importata in STRUCTURE per l'analisi di popolazione può essere presa in considerazione dall'algoritmo Bayesiano come supporto all'elaborazione dei risultati.

Le successive variabili del dataset da importare in questa sezione sono obbligatorie, e devono contenere valori numerici relativi ai tipi di marker utilizzati. A seconda della ploidia dell'organismo analizzato, ci sarà un numero di colonne per ogni locus pari al numero degli alleli. Per uno stesso locus genetico, l'header della colonna deve contenere un unico nome del locus seguito dal simbolo "." e dal numero identificativo dell'allele, partendo da 1 (e. g.: "Locus\_1.1," "Locus\_1.2", ecc.). Inoltre, in questo tipo di file i missing values devono essere indicati soltanto con la sigla **NA**.

Di seguito è riportata un'immagine che rappresenta dei dati conservati in un file di elaborazione dati che, una volta convertito in formato .txt o .csv può essere correttamente letto da StructuRly:

![image\_1](https://user-images.githubusercontent.com/35098432/49116039-aa40e600-f29c-11e8-82dd-05958633d416.png)

1.  Nella sezione **import the admixture analysis** l'utente può importare un dataset ottenuto direttamente in seguito all'analisi dei suoi dati genetici attraverso software per l'analisi di popolazione come STRUCTURE o ADMIXTURE. Le caratteristiche di questo file non sono molto diverse da quello da importare nella precedente sezione:

-   le tre variabili facoltative (**Sample\_ID**, **Pop\_ID** e **Loc\_ID**) possono essere nuovamente inserite, in quest'ordine preciso, all'interno del file di importazione, con l'unica differenza che in questo caso le categorie delle variabili **Pop\_ID** e **Loc\_ID** non devono per forza essere rappresentate da numeri, ma anche da caratteri.

-   le variabili da inserire obbligatoriamente sono quelle dell'admixture proportion calcolate dai software sopra citati, e che saranno in numero uguale al numero di cluster scelti dall'utente prima di eseguire l'analisi Bayesiana. Ognuna di queste variabili deve essere identificata da un header in cui è presente la lettera "K" e il numero del relativo cluster in sequenza (e. g.: "K1", "K2", "K3", ecc.), cioè nell'ordine in cui il dataset è stato esportato dal software.

![image\_2](https://user-images.githubusercontent.com/35098432/49116054-b88f0200-f29c-11e8-9abf-0c050bdb1e45.png)

-   se la tua matrice di dati di admixture è stata ottenuta in seguito al software admixture, ci sono due modi di procedere per preparare il tuo dataset per StructuRly. Dall'analisi di un file in formato .bed o .ped otterrai un file in formato **.Q** che puoi sia importare in R e quindi modificare a tuo piacimento, esportandolo in formato .txt o .csv per poi importarlo in StructuRly (ad esempio dopo aver aggiunto le colonne identificative del nome dei campioni o della popolazione) oppure puoi importare in StructuRly direttamente il file in formato **.Q**. Questo file contiene soltanto le variabili con i valori dell'admixture ancestry in numero uguale a quello dei cluster scelti per dall'utente per l'analisi: se vuoi aggiungere a questo dataset i metadati dovrai importare in StructuRly anche il file in formato **.fam** che è generalmente accompagna i file in formato .bed e .ped. Se vuoi maggiori dettagli riguardo le informazioni codificate in questi tipi di formati, puoi trovarli a questo [link](https://www.cog-genomics.org/plink2/formats#fam). StructuRly utilizzerà automaticamente le prime due variabili del file in formato **.fam**, le quali sono generalmente utilizzate rispettivamente per insirire l'identificativo dei campioni e la popolazione putativa definita dall'utente.

Examples of the .txt, .csv, .Q and .fam files that you can import into StructuRly are present at the following link of the repository: [Sample datasets](https://github.com/nicocriscuolo/OliveR/tree/master/inst/CSV_data) (the .Q and the .fam files are obtained after an [ADMIXTURE](http://software.genetics.ucla.edu/admixture/) analysis with the sample files downloadable directly from the software website).

1.  The third section of StructuRly, **Compare partitions**, must be used after the files for the first two sections have been loaded and read by StructuRly to compare the partitions obtained from the hierarchical and Bayesian cluster analysis. Obviously the imported datasets must refer to data of same nature and the number of observations must be the same in both files.

Outputs download
----------------

Nella seguente immagine è raffigurato il principale output di StructuRly (il barplot dell'admixture ancestry) scaricato dall'applicazione dopo essere stato personalizzato. Le label dei campioni sull asse X sono colorate secondo la popolazione indicata nel file di input dall'utente, mentre i simboli nella parte superiore del plot indicano il sito di campionamento:

All StructuRly outputs can be downloaded as images in various high quality formats directly from the application user interface. However, to download the graphs related to the Triangle plot, obtained through a specific function of the *plotly* package (and not with those of *ggplot2*) you need to download the **orca** software in your computer and follow the instructions at this [link](https://github.com/plotly/orca#installation).

Example
-------

Below is a link to the [YouTube]() video of the application showing an example of using the software:

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/0FUFM6GNaYI&feature=youtu.be/0.jpg)](http://www.youtube.com/watch?v=0FUFM6GNaYI&feature=youtu.be)

Known bugs and issues
---------------------

-   in the interactive barplot the X axis labels are not colored according to the different populations entered by the user in the input file. To view colored labels, download the image in one of the different formats available. This bug is related to the functions of a third-party package and has been reported to the GitHub community at [link](https://github.com/ropensci/plotly/issues/1328);

-   in the comparison plot the separation lines of the heatmap cells are not visible when using the interactive graph for a bug present in the package used to produce this output. Again, download the output to view the complete chart;

-   the graphic output produced when the observations of the barplot are divided into sections according to the different populations or sampling sites (or both) may not be accurate when using a very large number (&gt; 60) of populations or sites defined a priori by the user. Moreover, in this case, you could see a light overlap between the axis title and the axis text of the barplot.

The slight bugs related to some characteristics of the graphs are shown only in the presence of the interactive plot, but the downloaded file won't present any problem.

Citation
--------

StructuRly was firstly presented during the [International BBCC meetings](https://www.bbcc-meetings.it) held in Naples in November 2018 and its implementation has been described inside the paper *StructuRly - A novel shiny app to produce elegant, detailed and interactive plot from STRUCTURE and ADMIXTURE outputs* (submitted). If you use this package for results included in your paper please cite:

    #> 
    #> To cite package 'StructuRly' in publications use:
    #> 
    #>   Nicola Criscuolo and Claudia Angelini (2018). StructuRly:
    #>   Elegant, detailed and interactive plots for STRUCTURE and
    #>   ADMIXTURE outputs. R package version 0.1.0.
    #>   https://github.com/nicocriscuolo/StructuRly
    #> 
    #> A BibTeX entry for LaTeX users is
    #> 
    #>   @Manual{,
    #>     title = {StructuRly: Elegant, detailed and interactive plots for STRUCTURE and ADMIXTURE outputs},
    #>     author = {Nicola Criscuolo and Claudia Angelini},
    #>     year = {2018},
    #>     note = {R package version 0.1.0},
    #>     url = {https://github.com/nicocriscuolo/StructuRly},
    #>   }

Contact
-------

For additional information regarding StructuRly, please consult the documentation or [email us](mailto:nico.criscuolo981@gmail.com).
