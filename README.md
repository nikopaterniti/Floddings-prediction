## Previsione livelli WSE
Generazione matrice binaria celle asciutte da portata variabile in input.
- Simulazione 16-01-2021 [INPUT](https://github.com/halixness/swe-nn-tesi/tree/main/00-task/input), [DECODE](https://drive.google.com/file/d/1RSLllav9MpYREqR1eEFX1UHm2I5UWh6_/view?usp=sharing), [OUTPUT](https://github.com/halixness/swe-nn-tesi/tree/main/00-task/output/output_16-01-2021_09.35)

#### Passaggi:
- Script generatore portata:
  - [X] *(N)* Modifica e genera i file BCC (START: 0,01840, END: 0,1840, STEPS: 100)
  - *(N)* Generare dataset (file numpy con array di portate)
- Simulazioni:
  - [X] *(S)* Creare script automatici per le varie N versioni del file BCC
  - [X] *(S)* Lanciare le N simulazioni
  - [X] *(S)* Effettuare il decode con diversi livelli di risoluzione (**2x, 3x**)
  - [X] *(D)* Da ogni simulazione plottare LAST_WSE e LAST_DEP
  - [X] *(D)* Da ogni simulazione calcolare LAST_WSE - LAST_BTM, poi generare matrice binaria (**soglia = 5e-3, 1e-3**) e plottare queste matrici binarie

#### Modelli sviluppati

| Modello | Output | Train Accuracy | Test Accuracy | Metric | Stato |
|---------|--------|----------------|---------------|--------|--------|
| [Feed forward](https://colab.research.google.com/drive/1ZSE7LwpSSqlcNsjjxw74E4u5PI-tlu5u?usp=sharing) | Matrice Binaria |  97,3%  | 96%  | Cosine Distance | DA VERIFICARE |
| [LSTM + CNN](https://colab.research.google.com/drive/1C3rrqjSyKm-ucwMJx4tKUz2XPnEfMbwc?usp=sharing) | Matrice MAXWSE |  94,9%  | 96,1%  | Cosine Distance | DA VERIFICARE |

## Generazione dei file da simulazione

### Prerequisiti per avviare il progetto
- La cartella `$SCRATCH` deve contenere i seguenti elementi:
  - **swegpu**
  - **input/** 
    - BCC
    - *tutti i file input alla simulazione*
  - **output/** 
    - *output_XX-XX-XXXX_XX.XX* cartella simulazione
      - *decoded_XXX*
    
- Creare la cartella `/hpc/scratch/<username>/tesi`

### Per avviare il progetto
Eseguire `./dispatch-simulations.sh`.
Si prevedono tempi di esecuzione di circa 15 minuti

### Risultati dello script
Recuperabile sul cluster, nella cartella `$SCRATCH/tesi` in scratch dell'utente, sotto `output_<data_avvio_sbatch>`
Copiare queste cartelle output dentro `$SCRATCH/output/`.

## Generazione dei file target

Il programma `generate_target.py` permette la generazione delle matrici target e dei plot BTM-WSE per ogni simulazione.
Usage: `python generate_target.py -o output_xx_xx_xxxx-xx.xx -n numero_files_bcc`

### Risultati dello script
Il programma genererá due cartelle sotto `output/output_xx_xx_xxxx-xx.xx`:
- `plots`, contenente i plot delle matrici \*LAST.BTM e \*LAST.MAXWSE per ogni simulazione, nel formato `btm/maxwse_XX.png`
- `targets`, contenente `5mm` e `1mm`, ciascuna con una serie di matrici binarie per ogni numero simulazione. Con il flag `-npy 1` lo script esporterá anche gli array di matrici `5mm` e `1mm` in due comodi file `.npy`.
