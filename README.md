GUIDA Vecchia :

Update stack cloudformation GUIDA: 

L'update dei template nested di cloudformation risulta essere diverso rispetto all'utilizzo di SAM. 

1) Prendere il template modificato e aggiungerlo al bucket s3 chiamato sc365-infrastructure dentro la cartella templates
2) Andare su cloudformation => Andare nello stack chiamato main-prod => Cliccare su update => Use current template => Fare sempre next 

Se si aggiunge un nuovo file si dovrà modificare anche il template principale main.yaml
Per farlo invece di cliccare su use current template si dovrà fare l'upload del file come un classico cloudformation. 



GUIDA Nuova: 

- Abbiamo implementato la CI/CD che permetta di fare i passaggi della guida vecchia in modo automatico. 
- CodeBuild ci permette di eseguire una macchina virtuale all'interno dell'infrastruttura di AWS per eseguire dei comandi via CLI ( banalmente anche la build di un'applicazione)
- Per la corretta esecuzione è stato creato uno stack standalone utilizzando il template build-project il quale va a creare il nostro CodeBuild associato al PUSH del nostro repo. 
- In particolare, dentro il template buildproject c'è la sezione Trigger il quale ha il commpito di andare ad indicare il branch che deve controllare attraverso dei webhook. L'abilitazione dei log serve essenzialmente per vedere tutti i passaggi che fa il nostro codebuild ( Andare nella pagina di codebuild per vedere i log del progetto)
- All'interno del buildproject è possibile inserire un attributo per dirgli dove si trova il nostro build-spec se non viene specificato il nostro build-spec.yaml si deve trovare all'interno della root del repository. 
- Nel nostro build spec troviamo in fase di pre-build la copia di tutti i template all'interno del nostro bucket passatogli come parametro. In fase di build applica il nostro main.yaml utilizzando i parametri che si trovano dentro il file main-parameters.json. 
