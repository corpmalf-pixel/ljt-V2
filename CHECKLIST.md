# LJT Paysage - Deployment & Verification Checklist

## Pre-Deployment Checklist

### Code & Files
- [ ] All files present (index.html, styles.css, script.js, etc.)
- [ ] No console errors in build.js
- [ ] robots.txt and sitemap.xml generated
- [ ] manifest.json validated
- [ ] CSS minification checked
- [ ] JavaScript minification optional (can be done)

### Server Preparation
- [ ] SSH access confirmed to holua.duckdns.org
- [ ] Root or sudo access available
- [ ] Firewall rules noted (ports 80, 443)
- [ ] Disk space checked (need ~100MB minimum)
- [ ] Nginx/Apache installation plan confirmed

---

## Deployment Checklist

### Phase 1: Server Setup
- [ ] System updated (`apt update && apt upgrade`)
- [ ] Nginx installed and running
- [ ] Certbot installed
- [ ] Firewall configured (80, 443 open)

### Phase 2: File Deployment
- [ ] Directory created: `/var/www/ljt-paysage/`
- [ ] Files copied to server
- [ ] Permissions set: `chown -R www-data:www-data`
- [ ] Nginx config copied to sites-available
- [ ] Symbolic link created in sites-enabled

### Phase 3: Nginx Configuration
- [ ] Config syntax tested: `nginx -t`
- [ ] Default site disabled: `rm sites-enabled/default`
- [ ] Nginx reloaded: `systemctl reload nginx`
- [ ] Site accessible on HTTP (temporary)

### Phase 4: SSL/HTTPS
- [ ] Domain accessible via holua.duckdns.org
- [ ] Certbot certificate generated
- [ ] Certificate paths confirmed in nginx.conf
- [ ] HTTPS redirect configured
- [ ] Nginx reloaded with SSL

### Phase 5: Security Headers
- [ ] HSTS header present
- [ ] X-Frame-Options set
- [ ] X-Content-Type-Options set
- [ ] X-XSS-Protection set
- [ ] CSP header set
- [ ] Referrer-Policy set

---

## Post-Deployment Verification

### Basic Connectivity
- [ ] Site loads on HTTPS: `https://holua.duckdns.org/`
- [ ] HTTP redirects to HTTPS: `curl -I http://holua.duckdns.org/`
- [ ] Response code 200 for HTML
- [ ] Response code 200 for CSS, JS
- [ ] All resources load (no 404 errors)

### SSL/HTTPS Verification
```bash
# Test SSL
curl -I https://holua.duckdns.org/

# Verify certificate
openssl s_client -connect holua.duckdns.org:443

# Check SSL labs rating (visit website)
https://www.ssllabs.com/ssltest/?d=holua.duckdns.org
```

- [ ] Certificate valid (not self-signed)
- [ ] Certificate not expired
- [ ] Certificate matches domain
- [ ] SSL Labs rating A+ (98/100 target)

### Headers Verification
```bash
curl -I https://holua.duckdns.org/ | grep -E "Strict-Transport|X-Frame|X-Content|X-XSS|CSP"
```

- [ ] Strict-Transport-Security present
- [ ] X-Frame-Options: SAMEORIGIN
- [ ] X-Content-Type-Options: nosniff
- [ ] X-XSS-Protection present
- [ ] Content-Security-Policy present

### Compression Verification
```bash
curl -H "Accept-Encoding: gzip" -I https://holua.duckdns.org/
```

- [ ] Content-Encoding: gzip present
- [ ] styles.css is gzipped
- [ ] script.js is gzipped

### Cache Headers Verification
```bash
curl -I https://holua.duckdns.org/styles.css | grep Cache
curl -I https://holua.duckdns.org/script.js | grep Cache
```

- [ ] Cache-Control present for static assets
- [ ] Expires headers set
- [ ] ETag headers set

### SEO Files Verification
```bash
curl -I https://holua.duckdns.org/robots.txt
curl -I https://holua.duckdns.org/sitemap.xml
```

- [ ] robots.txt accessible (200 OK)
- [ ] sitemap.xml accessible (200 OK)
- [ ] manifest.json accessible (200 OK)
- [ ] favicon.ico accessible (or 404 OK)

### Mobile Responsiveness
- [ ] Test on mobile device (iPhone, Android)
- [ ] Tap targets minimum 44x44px
- [ ] Text readable (no horizontal scroll)
- [ ] Viewport meta tag present
- [ ] Images responsive (max-width: 100%)

### Functionality Testing
- [ ] All navigation links work
- [ ] Anchor links scroll smoothly (#services, #contact)
- [ ] Form validation works
- [ ] Form submission shows success message
- [ ] CTA buttons clickable and styled
- [ ] Smooth scrolling enabled

### Performance Testing

#### Lighthouse
```bash
# Via Google Chrome DevTools (F12 → Lighthouse)
# Or command line:
npm install -g @lhci/cli
lhci autorun
```

- [ ] Performance score: >= 95
- [ ] Accessibility score: >= 95
- [ ] Best Practices score: >= 95
- [ ] SEO score: = 100
- [ ] PWA score: >= 80

#### PageSpeed Insights
```
Visit: https://pagespeed.web.dev/
Enter: https://holua.duckdns.org/
```

- [ ] Desktop score: >= 90
- [ ] Mobile score: >= 85
- [ ] LCP (Largest Contentful Paint): < 2.5s
- [ ] INP (Interaction to Next Paint): < 200ms
- [ ] CLS (Cumulative Layout Shift): < 0.1

#### Manual Performance Checks
```bash
# Check time to first byte
curl -o /dev/null -s -w '%{time_starttransfer}\n' https://holua.duckdns.org/

# Check full load time
curl -o /dev/null -s -w '%{time_total}\n' https://holua.duckdns.org/
```

- [ ] TTFB < 200ms
- [ ] Full page load < 2s
- [ ] No large resources (JS > 100KB, CSS > 50KB)

### SEO Verification

#### Meta Tags
```bash
curl -s https://holua.duckdns.org/ | grep -E '<title>|<meta name="description"|og:'
```

- [ ] Title tag present and descriptive
- [ ] Meta description present
- [ ] Meta keywords present
- [ ] og:title present
- [ ] og:description present
- [ ] og:type: "business.business"
- [ ] Canonical URL present

#### Structured Data
```bash
curl -s https://holua.duckdns.org/ | grep 'application/ld+json'
```

- [ ] LocalBusiness schema present
- [ ] Name: "LJT Paysage"
- [ ] Phone: "07 88 33 02 10"
- [ ] Address region: "Île-de-France"
- [ ] Service types listed

#### Heading Structure
```bash
curl -s https://holua.duckdns.org/ | grep -E '<h[1-6]' | head -20
```

- [ ] Single H1 tag
- [ ] H2 tags for sections
- [ ] Proper nesting (no H1 → H3 jumps)
- [ ] Descriptive heading text

#### Mobile SEO
- [ ] Viewport meta tag present
- [ ] Font size readable (16px+)
- [ ] No flash (required for mobile)
- [ ] Tap targets 44x44px minimum

### Google Search Console
- [ ] Property created for https://holua.duckdns.org
- [ ] Domain verified (HTML or DNS method)
- [ ] Sitemap submitted
- [ ] No critical indexing errors
- [ ] Coverage status: "All pages indexed"

### Browser Compatibility
- [ ] Chrome/Edge (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] iOS Safari
- [ ] Android Chrome

### Accessibility Testing

#### Keyboard Navigation
- [ ] Tab key navigates through all interactive elements
- [ ] Enter key activates buttons/links
- [ ] Escape key closes modals (if any)
- [ ] Focus indicator visible

#### Screen Reader (with NVDA/JAWS)
- [ ] Page title announced
- [ ] Headings announced with levels
- [ ] Links have descriptive text
- [ ] Images have alt text
- [ ] Form labels associated with inputs

#### Color Contrast
- [ ] Text contrast ratio >= 4.5:1 (AA)
- [ ] Large text contrast >= 3:1 (AA)
- [ ] No info conveyed by color alone

### Monitoring & Logging

#### Nginx Logs
```bash
sudo tail -f /var/log/nginx/ljt-paysage-access.log
sudo tail -f /var/log/nginx/ljt-paysage-error.log
```

- [ ] Access logs being written
- [ ] No error logs
- [ ] HTTP status codes healthy (200, 301, 404)

#### SSL Certificate Renewal
```bash
sudo certbot certificates
```

- [ ] Certificate expiry date noted
- [ ] Auto-renewal enabled
- [ ] Next renewal date future

---

## Post-Launch Monitoring

### Weekly
- [ ] Check access logs for errors
- [ ] Google Search Console: check for crawl errors
- [ ] Monitor certificate expiry countdown

### Monthly
- [ ] Run full Lighthouse audit
- [ ] Check SSL Labs rating
- [ ] Review PageSpeed Insights
- [ ] Verify Search Console indexing
- [ ] Check for broken links

### Quarterly
- [ ] Security audit (SSL, headers, etc.)
- [ ] Performance optimization review
- [ ] Core Web Vitals analysis
- [ ] Backlink profile review

---

## Troubleshooting Guide

### Issue: 502 Bad Gateway
```bash
# Check if Nginx is running
sudo systemctl status nginx

# Check Nginx config
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
```

### Issue: SSL Certificate Error
```bash
# Check certificate status
sudo certbot certificates

# Renew manually
sudo certbot renew --force-renewal
```

### Issue: Slow Load Time
```bash
# Check if Gzip is enabled
curl -H "Accept-Encoding: gzip" -I https://holua.duckdns.org/

# Check server resources
free -h
df -h
top
```

### Issue: 404 Errors
```bash
# Check file permissions
sudo ls -la /var/www/ljt-paysage/

# Check index.html exists
sudo test -f /var/www/ljt-paysage/index.html && echo "Found" || echo "Not found"
```

---

## Sign-Off

- [ ] All checks completed
- [ ] All issues resolved
- [ ] Client approval obtained
- [ ] Documentation delivered
- [ ] Support plan in place

**Deployment Date:** ___________  
**Deployed By:** ___________  
**Verified By:** ___________  

---

## Next Steps

1. **Day 1**: Monitor for any immediate issues
2. **Week 1**: Submit sitemap to Google Search Console
3. **Month 1**: Analyze first month of data, optimize if needed
4. **Ongoing**: Regular monitoring and updates

**Contact for Support:** 07 88 33 02 10 (LJT Paysage)
