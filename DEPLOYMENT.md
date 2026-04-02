# LJT Paysage - Guide de Déploiement Complet

## 🚀 Déploiement sur holua.duckdns.org

### Prérequis

- VPS/Serveur Linux (Ubuntu 20.04+)
- Accès root ou sudo
- Certbot pour SSL
- Nginx ou Apache
- SSH configuré

---

## Phase 1: Préparation du Serveur

### 1.1 Mettre à jour le système

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git nano
```

### 1.2 Installer Certbot (SSL/HTTPS)

```bash
sudo apt install -y certbot python3-certbot-nginx
# ou pour Apache:
# sudo apt install -y certbot python3-certbot-apache
```

### 1.3 Ouvrir les ports Firewall

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

---

## Phase 2: Déploiement avec Nginx (Recommandé)

### 2.1 Installer Nginx

```bash
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 2.2 Préparer le répertoire de site

```bash
# Créer le répertoire
sudo mkdir -p /var/www/ljt-paysage
cd /var/www/ljt-paysage

# Copier les fichiers depuis local
# (exécuter depuis votre machine locale:)
scp -r /path/to/ljt-paysage/* user@holua.duckdns.org:/var/www/ljt-paysage/

# Ou avec git (si vous versionnez):
git clone <repository> .
```

### 2.3 Définir les permissions

```bash
sudo chown -R www-data:www-data /var/www/ljt-paysage
sudo chmod -R 755 /var/www/ljt-paysage
sudo chmod 644 /var/www/ljt-paysage/*
```

### 2.4 Créer la configuration Nginx

```bash
# Copier la configuration
sudo cp nginx.conf /etc/nginx/sites-available/ljt-paysage

# Créer un lien symbolique
sudo ln -s /etc/nginx/sites-available/ljt-paysage /etc/nginx/sites-enabled/

# Désactiver la config par défaut
sudo rm /etc/nginx/sites-enabled/default
```

### 2.5 Tester la configuration Nginx

```bash
sudo nginx -t
# Devrait afficher: "syntax is ok"
```

### 2.6 Recharger Nginx

```bash
sudo systemctl reload nginx
```

---

## Phase 3: Configuration SSL avec Certbot

### 3.1 Générer le certificat SSL

```bash
sudo certbot certonly --nginx -d holua.duckdns.org
# ou avec --agree-tos pour auto-agree:
# sudo certbot certonly --nginx -d holua.duckdns.org --agree-tos -m contact@ljt-paysage.fr
```

### 3.2 Vérifier les chemins SSL

```bash
# Les certificats seront à:
ls -la /etc/letsencrypt/live/holua.duckdns.org/
# Vérifier que fullchain.pem et privkey.pem existent
```

### 3.3 Recharger Nginx avec SSL

```bash
sudo systemctl reload nginx
```

### 3.4 Configurer le renouvellement automatique

```bash
# Activer le timer systemd
sudo systemctl enable certbot.timer
sudo systemctl start certbot.timer

# Tester le renouvellement
sudo certbot renew --dry-run
```

---

## Phase 4: Déploiement avec Apache (Alternative)

### 4.1 Installer Apache

```bash
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

### 4.2 Préparer le répertoire

```bash
sudo mkdir -p /var/www/ljt-paysage
sudo cp -r ljt-paysage/* /var/www/ljt-paysage/
sudo chown -R www-data:www-data /var/www/ljt-paysage
```

### 4.3 Créer la configuration VirtualHost

```bash
sudo nano /etc/apache2/sites-available/ljt-paysage.conf
```

Ajouter:

```apache
<VirtualHost *:80>
    ServerName holua.duckdns.org
    ServerAlias www.holua.duckdns.org
    DocumentRoot /var/www/ljt-paysage
    
    <Directory /var/www/ljt-paysage>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/ljt-paysage-error.log
    CustomLog ${APACHE_LOG_DIR}/ljt-paysage-access.log combined
</VirtualHost>
```

### 4.4 Activer les modules nécessaires

```bash
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod deflate
sudo a2enmod expires
sudo a2enmod ssl
```

### 4.5 Activer le site

```bash
sudo a2ensite ljt-paysage
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### 4.6 Configurer SSL

```bash
sudo certbot --apache -d holua.duckdns.org
```

---

## Phase 5: Vérifications Post-Déploiement

### 5.1 Vérifier HTTPS

```bash
# Tester la connexion SSL
curl -I https://holua.duckdns.org/

# Vérifier le certificat
openssl s_client -connect holua.duckdns.org:443 -servername holua.duckdns.org
```

### 5.2 Vérifier les headers de sécurité

```bash
curl -I https://holua.duckdns.org/ | grep -E "Strict-Transport-Security|X-Frame|X-Content"
```

### 5.3 Vérifier la compression Gzip

```bash
curl -H "Accept-Encoding: gzip" -I https://holua.duckdns.org/
# Doit afficher "Content-Encoding: gzip"
```

### 5.4 Vérifier le cache browser

```bash
curl -I https://holua.duckdns.org/styles.css | grep -E "Cache-Control|ETag|Expires"
```

### 5.5 Vérifier l'accès aux fichiers SEO

```bash
curl -I https://holua.duckdns.org/robots.txt
curl -I https://holua.duckdns.org/sitemap.xml
```

---

## Phase 6: Optimisations SEO

### 6.1 Ajouter le site à Google Search Console

```
1. Aller à https://search.google.com/search-console
2. Ajouter la propriété: https://holua.duckdns.org/
3. Vérifier la propriété (HTML ou DNS)
4. Soumettre le sitemap: sitemap.xml
```

### 6.2 Vérifier avec PageSpeed Insights

```
https://pagespeed.web.dev/
Entrer: https://holua.duckdns.org/
```

### 6.3 Vérifier avec Lighthouse

```bash
# Via Chrome: F12 → Lighthouse
# Ou en CLI:
npm install -g @lhci/cli
lhci autorun
```

### 6.4 Vérifier avec SSL Labs

```
https://www.ssllabs.com/ssltest/
Entrer: holua.duckdns.org
Target: A+ rating
```

---

## Phase 7: Monitoring & Maintenance

### 7.1 Logs d'accès

```bash
# Nginx
sudo tail -f /var/log/nginx/ljt-paysage-access.log

# Apache
sudo tail -f /var/log/apache2/ljt-paysage-access.log
```

### 7.2 Logs d'erreur

```bash
# Nginx
sudo tail -f /var/log/nginx/ljt-paysage-error.log

# Apache
sudo tail -f /var/log/apache2/ljt-paysage-error.log
```

### 7.3 Renouvellement SSL (automatique)

```bash
# Vérifier le statut
sudo systemctl status certbot.timer

# Forcer le renouvellement (si nécessaire)
sudo certbot renew --force-renewal
```

### 7.4 Mises à jour système (scheduled)

```bash
# Configurer une tâche cron pour les mises à jour
sudo crontab -e

# Ajouter:
# 0 2 * * 0 apt update && apt upgrade -y
```

---

## Phase 8: Sauvegardes

### 8.1 Sauvegarder les fichiers du site

```bash
# Créer une sauvegarde locale
tar -czf ljt-paysage-backup.tar.gz /var/www/ljt-paysage/

# Télécharger la sauvegarde
scp user@holua.duckdns.org:/path/to/ljt-paysage-backup.tar.gz .
```

### 8.2 Sauvegarder les certificats SSL

```bash
# Attention: Les certificats sont sensibles!
sudo tar -czf letsencrypt-backup.tar.gz /etc/letsencrypt/

# Stocker en sécurité (crypté)
```

### 8.3 Configurer des sauvegardes automatiques

```bash
# Exemple avec BackBlaze B2 ou AWS S3
# (À configurer selon votre infrastructure)
```

---

## Checklist de Déploiement Final

- [ ] Système mis à jour
- [ ] Nginx/Apache installé et configuré
- [ ] SSL certificat généré (Certbot)
- [ ] Fichiers copiés et permissions correctes
- [ ] Headers de sécurité activés
- [ ] Compression Gzip fonctionnelle
- [ ] Cache browser configuré
- [ ] robots.txt et sitemap.xml accessibles
- [ ] HTTPS redirige HTTP (301)
- [ ] Certificat SSL valide (A+ sur SSL Labs)
- [ ] Site accessible sur https://holua.duckdns.org
- [ ] Lighthouse score > 95
- [ ] PageSpeed Insights score > 90
- [ ] Sitemap soumise à Google Search Console
- [ ] Logs monitoring en place
- [ ] Renouvellement SSL automatisé
- [ ] Sauvegardes configurées

---

## 🆘 Troubleshooting

### Problème: "Connection refused" sur port 80/443

```bash
# Vérifier les ports ouverts
sudo netstat -tlnp | grep LISTEN

# Vérifier le firewall
sudo ufw status

# Réouvrir les ports si nécessaire
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Problème: "Permission denied" sur fichiers

```bash
# Corriger les permissions
sudo chown -R www-data:www-data /var/www/ljt-paysage
sudo chmod -R 755 /var/www/ljt-paysage
```

### Problème: Certificat SSL non trouvé

```bash
# Vérifier le statut Certbot
sudo certbot certificates

# Régénérer si nécessaire
sudo certbot certonly --nginx -d holua.duckdns.org --force-renewal
```

### Problème: Site lent

```bash
# Vérifier la compression Gzip
gzip -l *.gz

# Vérifier les requêtes N+1 ou redondantes
curl -X GET https://holua.duckdns.org/ -w '@curl-format.txt'
```

---

## 📞 Support Technique

Pour toute question ou assistance :
- Email: contact@ljt-paysage.fr
- Téléphone: 07 88 33 02 10
- Horaires: Lun-Sam 07:30–19:30

---

**Dernière mise à jour**: 2026-04-01  
**Version**: 1.0.0  
**Status**: Production Ready
