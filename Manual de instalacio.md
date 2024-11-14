---

# Instal·lació i Configuració d'una Cloud a Ubuntu

Aquest manual et guiarà pas a pas per instal·lar i configurar un servidor web a Ubuntu amb `apache2`, `mysql` i les llibreries de `php`, per després implementar una cloud.

## 1. Preparació de la màquina

Primer, assegura't que el teu sistema operatiu estigui actualitzat per evitar problemes amb dependències.

```bash
sudo apt update
sudo apt upgrade
```
<img src="Images/sudo apt upupg.PNG" alt="Utilitzar comandes en terminal">

## 2. Instal·lació de `apache2`

Executa la següent comanda per instal·lar el servidor `apache2`:

```bash
sudo apt install -y apache2
```

## 3. Instal·lació de `MySQL`

Instal·la `MySQL` per configurar les dades de la teva aplicació:

```bash
sudo apt install -y mysql-server
```

## 4. Instal·lació de llibreries `PHP`

`PHP` és el llenguatge que utilitzen moltes aplicacions web. Per instal·lar-lo juntament amb algunes llibreries necessàries, executa:

```bash
sudo apt install -y php libapache2-mod-php
sudo apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

## 5. Reiniciar el servidor `apache2`

Després d'instal·lar `PHP` i les llibreries, reinicia el servidor web per aplicar els canvis:

```bash
sudo systemctl restart apache2
```

## 6. Configuració de `MySQL`

Accedeix a la consola de `MySQL`:

```bash
sudo mysql
```

### Crear una base de dades i un usuario

1. **Crear la base de dades**:
   ```sql
   CREATE DATABASE bbdd;
   ```

2. **Crear un usuario i assignar permisos**:
   ```sql
   CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY '0wnC10ud';
   GRANT ALL ON bbdd.* TO 'usuario'@'localhost';
   ```

3. **Sortir de `MySQL`**:
   ```sql
   exit
   ```

### Comprovar la connexió

Prova de connectar-te a la base de dades amb l'usuario que acabes de crear:

```bash
mysql -u usuario -p
```

## 7. Permetre connexions des d'una màquina remota (Opcional)

Si vols permetre connexions externes, edita l'arxiu de configuració de `MySQL`:

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

Canvia la línia `bind-address = 127.0.0.1` per:

```bash
bind-address = 0.0.0.0
```

Reinicia `MySQL`:

```bash
sudo systemctl restart mysql
```

## 8. Descomprimir i moure els fitxers de l'aplicació web

1. **Copiar l'arxiu ZIP al directori del servidor**:
   ```bash
   sudo cp ~/Baixades/app-web.zip /var/www/html
   ```

2. **Anar al directori `/var/www/html`**:
   ```bash
   cd /var/www/html
   ```

3. **Descomprimir l'arxiu**:
   ```bash
   sudo unzip app-web.zip
   ```

4. **Moure els fitxers descomprimits a `/var/www/html`**:
   ```bash
   sudo cp -R app-web/. /var/www/html
   ```

5. **Eliminar la carpeta descomprimida i l'arxiu ZIP**:
   ```bash
   sudo rm -rf app-web/
   sudo rm -rf app-web.zip
   ```

## 9. Eliminar l'arxiu `index.html` per defecte

Per evitar conflictes amb la pàgina per defecte d'`apache2`, elimina l'arxiu `index.html`:

```bash
sudo rm -rf /var/www/html/index.html
```

## 10. Aplicar permisos als fitxers

Configura els permisos i la propietat dels fitxers perquè `apache2` els pugui gestionar correctament:

```bash
sudo chmod -R 775 /var/www/html
sudo chown -R usuario:www-data /var/www/html
```

## 11. Accés i configuració al navegador

Obre el teu navegador i accedeix a `http://localhost`. Hauries de veure l'instal·lador de la teva aplicació web.

**Informació d'accés per a la base de dades**:
- **usuario**: `usuario`
- **Contrasenya**: `0wnC10ud`
- **Base de dades**: `bbdd`
- **Domini**: `localhost`
