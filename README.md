# Role wikijs

Role slouží pro nasazení webové aplikace [wikijs](https://wiki.js.org) postavené na `node`.
Aplikací spouštíme pomocí `node` serveru (systemd služba). Vzdálený přístup k ním
je třeba řešit pomocí reverzní proxy.

Aplikace vyžaduje databázi, proto `wikijs` role závisí na roli `mariadb`.

Setup stránka aplikace [vyžaduje](https://github.com/Requarks/wiki/issues/2239),
aby ID záznamů v databázi byli vytvářeny vždy s inkrementem 1. Tím vzniká problém, pokud
databáze má nastavený ID `auto_increment_offset` a `auto_increment_increment` na něco
jiného než výchozí hodnoty. Během prvotního nastavení aplikace je třeba si s tím poradit.


## Proměnné

Konfigurace `wikijs` se uskutečňuje pomocí slovníku `wikijs`.

| Proměnná       | Povinná | Výchozí         | Popis |
| -------------- | ------- | --------------- | ----- |
| user           | ne      | wikijs          | Systémový uživatel, pod kterým poběží systemd služba |
| group          | ne      | wikijs          | Skupina (uživatele), pod kterou poběží systemd služba |
| groups         | ne      |                 | Další skupiny, kterých bude `user` členem |
| version        | ano     |                 | Verze Wiki.js, kterou chceme nasadit |
| install_dir    | ne      | /srv/www/wikijs | Adresář, kam se aplikace nainstaluje |
| port           | ne      | 3465            | TCP port, na kterém bude `node` server poslouchat |
| offline        | ne      | false           | Režim Wiki.js, kdy není přímo připojena k Internetu. Lze využít i pro nasazení v síti s HTTP proxy |
| assets_dir     | ne      |                 | Adresář na dostupném filesystému pro ukládání souborů aplikací (Nastavuje se skrz webové rozhraní aplikace) |
|                |         |                 |      |
| database       | ano     |                 | Slovník pro nastavení přístupu do MySQL databáze |
| .name          | ne      | wikijs          | Jméno databáze |
| .user          | ne      | wikijs          | Jméno databázového uživatele pro přístup k databázi |
| .password      | ano     |                 | Heslo databázového uživatele |



## Příklady použití

```yaml
wikijs:
  version: '2.4.107'
  database:
    password: 'heslo'
  offline: true
  assets_dir: '/srv/www/wikijs/data'
```
