# Linky Téléinfo

## Coté Hard


## Coté Soft

J'ai installé ça sur le Raspberry pi: https://github.com/hallard/teleinfo

Quelques notes:

Schema MySQL:
```sql
CREATE TABLE teleinfo (DATE DATETIME, ADCO INTEGER, OPTARIF VARCHAR(64), ISOUSC INTEGER, BASE INTEGER, PTEC VARCHAR(64), IINST1 INTEGER, IMAX1 INTEGER, PAPP INTEGER, HHPHC VARCHAR(64), MOTDETAT INTEGER);

CREATE USER 'grafanaReader' IDENTIFIED BY 'grafana';
GRANT SELECT ON teleinfo.teleinfo TO 'grafanaReader';
```


Pour l'affichage, j'ai utilisé Grafana



Spec Enedis:

- https://www.enedis.fr/sites/default/files/Enedis-NOI-CPT_54E.pdf
