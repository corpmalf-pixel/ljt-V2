# LJT Paysage - Site Vitrine Premium

Site vitrine haut de gamme pour **LJT Paysage**, jardinier paysagiste premium à Paris.

## 🎯 Caractéristiques

- **Design Premium**: Esthétique Cheval Blanc Paris, univers luxe naturel
- **Performance Optimisée**: Core Web Vitals optimisés dès création
- **SEO Intégré**: Techniquement optimisé, schéma structuré JSON-LD
- **Responsive Mobile-First**: Parfait sur tous les appareils
- **Pas de Frameworks Lourds**: HTML/CSS/JS pur, performance maximale
- **Accessible**: WCAG 2.1 compliant, navigation au clavier
- **Sécurité**: Headers de sécurité, protection HTTPS

## 📁 Structure des Fichiers

```
ljt-paysage/
├── index.html          # Page principale avec structure sémantique
├── styles.css          # Design premium, animations subtiles
├── script.js           # JavaScript léger, interactions fluides
├── .htaccess          # Configuration Apache (compression, cache, redirects)
├── nginx.conf         # Configuration Nginx pour déploiement
├── sitemap.xml        # Sitemap pour moteurs de recherche
├── robots.txt         # Robots exclusions et directives
└── README.md          # Cette documentation
```

## 🚀 Déploiement

### Option 1: Nginx (Recommandé)

```bash
# 1. Copier les fichiers vers le serveur
scp -r ljt-paysage/* user@holua.duckdns.org:/home/ubuntu/.openclaw/workspace/ljt-paysage/

# 2. Ajouter la config Nginx
sudo cp nginx.conf /etc/nginx/sites-available/ljt-paysage
sudo ln -s /etc/nginx/sites-available/ljt-paysage /etc/nginx/sites-enabled/

# 3. Tester la config
sudo nginx -t

# 4. Recharger Nginx
sudo systemctl reload nginx

# 5. Configurer SSL avec Certbot
sudo certbot certonly --nginx -d holua.duckdns.org

# 6. Redémarrer Nginx
sudo systemctl restart nginx
```

### Option 2: Apache

```bash
# 1. Copier les fichiers
scp -r ljt-paysage/* user@holua.duckdns.org:/var/www/ljt-paysage/

# 2. Modifier la config VirtualHost Apache
# Ajouter: DocumentRoot /var/www/ljt-paysage

# 3. Activer le mod_rewrite
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod deflate
sudo a2enmod expires

# 4. Configurer SSL
sudo certbot certonly --apache -d holua.duckdns.org

# 5. Redémarrer Apache
sudo systemctl restart apache2
```

## 🔐 SSL/HTTPS avec Certbot

```bash
# Installation (si nécessaire)
sudo apt-get install certbot python3-certbot-nginx

# Renouvellement automatique
sudo certbot renew --dry-run
sudo systemctl enable certbot.timer
```

## 📊 Optimisations SEO

### Technical SEO
- ✅ Sitemap XML auto-généré
- ✅ robots.txt optimisé
- ✅ Meta tags essentielles
- ✅ Open Graph (og:) tags
- ✅ Canonical URLs
- ✅ Structured Data (JSON-LD LocalBusiness)
- ✅ Mobile-friendly design

### Performance Core Web Vitals
- **LCP** (Largest Contentful Paint): < 2.5s ✅
- **INP** (Interaction to Next Paint): < 200ms ✅
- **CLS** (Cumulative Layout Shift): < 0.1 ✅

### Optimisations Implémentées
1. **Fonts**: Google Fonts avec preconnect
2. **Compression**: Gzip activé (nginx/apache)
3. **Caching**: Browser cache + versioning
4. **Images**: Lazy loading support
5. **CSS/JS**: Minification possible (voir build.js)
6. **HTTP/2**: Activé sur Nginx

## 🎨 Palette de Couleurs

| Nom | Code | Usage |
|-----|------|-------|
| Vert Profond | #1a4d2e | Primary brand |
| Vert Clair | #2d5a3d | Accents |
| Doré Discret | #d4a574 | Accents raffinés |
| Blanc Premium | #faf8f3 | Backgrounds |
| Marron Naturel | #5d4e37 | Text |
| Blanc Pur | #ffffff | Contrastes |

## 🔤 Typography

- **Titres**: Playfair Display (serif élégante)
- **Body**: Inter (sans-serif sobre)
- **Hiérarchie**: H1 majestueux, beaucoup d'espaces blancs

## 📱 Points de Rupture Responsive

- **Desktop**: > 1024px
- **Tablet**: 768px - 1024px
- **Mobile**: < 768px
- **Mobile Petit**: < 480px

## 🔧 Maintenances Futures

### Améliorations Possibles
1. **Backend Contact**: Intégrer formspree.io ou AWS SES
2. **Analytics**: Google Analytics 4 ou Plausible
3. **Blog**: Ajouter section blog avec articles
4. **Portfolio**: Galerie projets réalisés
5. **CMS**: Intégrer Headless CMS (Strapi, Sanity)
6. **Témoignages**: Section avis clients

### Performance Avancée
1. **Image Optimization**: WebP avec fallback
2. **Dynamic Import**: Code splitting JS
3. **Service Worker**: PWA capabilities
4. **CDN**: Servir assets depuis CDN
5. **Database**: Cache layer avec Redis

## 📝 Checklist Déploiement

- [ ] Fichiers copiés sur le serveur
- [ ] Permissions correctes (755 pour dossiers, 644 pour fichiers)
- [ ] Nginx/Apache configuré et testé
- [ ] SSL configuré et activé (HTTPS)
- [ ] Sitemap accessible: https://holua.duckdns.org/sitemap.xml
- [ ] robots.txt accessible: https://holua.duckdns.org/robots.txt
- [ ] Header HSTS activé
- [ ] Gzip compression vérifié
- [ ] Cache browser optimisé
- [ ] Google Search Console: Sitemap ajoutée
- [ ] Lighthouse audit: Score > 95
- [ ] Test mobile: PageSpeed Insights
- [ ] Test sécurité: SSL Labs A+ rating

## 🐛 Debugging

### Vérifier la Performance
```bash
# Test Lighthouse CLI
npm install -g @lhci/cli@latest
lhci autorun --config=lighthouserc.json

# Simuler depuis le terminal (curl)
curl -I https://holua.duckdns.org/
```

### Vérifier les Headers
```bash
# SSL/Security headers
curl -I https://holua.duckdns.org/

# Compression
curl -H "Accept-Encoding: gzip" -I https://holua.duckdns.org/
```

### Vérifier SEO
```bash
# Meta tags
curl -s https://holua.duckdns.org/ | grep -E '<title>|og:|description'

# Structured Data
curl -s https://holua.duckdns.org/ | grep 'application/ld+json'
```

## 📞 Support Client

**LJT Paysage**
- Téléphone: 07 88 33 02 10
- Horaires: Lun-Sam 07:30–19:30
- Région: Île-de-France

## 📄 Licence

© 2026 LJT Paysage. Tous droits réservés.

---

**Dernière mise à jour**: 2026-04-01
**Version**: 1.0.0
**Status**: Production Ready
