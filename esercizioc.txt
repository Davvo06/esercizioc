#!/bin/bash

# Configurazione
SERVER="indirizzo_del_tuo_server"  # Sostituisci con l'indirizzo del tuo server SSH
USER="nome_utente"  # Sostituisci con il nome utente per l'accesso SSH
REMOTE_FILE="/percorso/remoto/comandi.txt"  # Sostituisci con il percorso remoto del file
LOCAL_FILE="comandi.txt"
INTERVAL=30

# Funzione per scaricare il file tramite SSH
scarica_file() {
    scp ${USER}@${SERVER}:${REMOTE_FILE} ${LOCAL_FILE}
}

# Funzione per leggere e eseguire l'ultimo comando
esegui_ultimo_comando() {
    if [ -f ${LOCAL_FILE} ]; then
        # Leggi l'ultimo comando dal file
        ultimo_comando=$(tail -n 1 ${LOCAL_FILE})

        # Esegui il comando
        echo "Eseguo il comando: ${ultimo_comando}"
        # Qui puoi aggiungere la logica per eseguire il comando
        # Ad esempio, puoi usare un case statement per gestire diversi comandi
        case ${ultimo_comando} in
            "ACCENDI.luce_sala")
                echo "Accendendo la luce della sala..."
                # Aggiungi qui il codice per accendere la luce della sala
                ;;
            "ACCENDI.riscaldamento")
                echo "Accendendo il riscaldamento..."
                # Aggiungi qui il codice per accendere il riscaldamento
                ;;
            *)
                echo "Comando non riconosciuto: ${ultimo_comando}"
                ;;
        esac
    else
        echo "File dei comandi non trovato!"
    fi
}

# Loop infinito per scaricare il file e eseguire il comando ogni 30 secondi
while true; do
    scarica_file
    esegui_ultimo_comando
    sleep ${INTERVAL}
done
