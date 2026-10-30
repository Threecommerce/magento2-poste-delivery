# Changelog

Tutte le modifiche rilevanti a questo modulo sono documentate in questo file.

Il formato è basato su [Keep a Changelog](https://keepachangelog.com/it/1.1.0/)
e questo progetto aderisce al [Semantic Versioning](https://semver.org/lang/it/).

## [1.0.1] - 2026-10-20
### Corretto
- Il commento di "Public Tracking URL" in configurazione mostrava il testo senza il segnaposto `{{waybill}}` (interpretato come direttiva di template).

## [1.0.0] - 2026-10-20

### Added
- **Metodo di spedizione Poste Delivery Business** (`carriers/postedelivery`) con due servizi, Standard
  (`APT000902`) ed Express (`APT000901`), prezzo fisso per servizio, soglia di spedizione gratuita e solo Italia.
  **Spento di default**; l'opzione *Mostra al checkout* permette di riservarlo agli ordini creati da admin.
- **Creazione della lettera di vettura** (API `parcel/waybill`) con salvataggio dell'etichetta in
  `var/Threecommerce/poste/labels/` (A4, 10x11 o ZPL), spedizione Magento e numero di tracking collegati
  al corriere, email di spedizione al cliente opzionale.
  - Pulsante **Crea Lettera di Vettura Poste** nell'ordine in admin (qualsiasi metodo di spedizione) e
    **Etichetta Poste** per scaricarla.
  - Creazione **automatica** via cron (ogni 5 minuti) per gli ordini con metodo Poste, **disattivata di default**,
    con data di partenza (impostata all'installazione), stati ordine, fattura obbligatoria opzionale e numero
    massimo di tentativi.
- **Tracking** (API `parcel/tracking`): cron ogni 30 minuti sulle spedizioni aperte, ultimo stato mostrato
  nell'ordine, commento sull'ordine alla consegna, link pubblico di tracking configurabile.
- Configurazione completa in *Negozi → Configurazione → Threecommerce Plugins → Poste Delivery Business*:
  ambiente (collaudo/produzione), credenziali (Secret ID cifrato), centro di costo, dati del mittente, valori
  predefiniti di spedizione, creazione automatica e tracking. Le credenziali sono verificate al salvataggio.
- ACL: configurazione, creazione lettera di vettura, download etichetta.

### Limiti noti
- Solo spedizioni nazionali (`ITA1`), un collo per ordine.
- Gli ordini in contrassegno non sono gestiti (il servizio accessorio richiede i codici del file
  "Esempi Compilazione Codici Accessori" di Poste).
- Con Magento multi-sorgente (MSI) la spedizione automatica richiede una sola sorgente.
