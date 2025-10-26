# Deployment Checklist

Use this checklist to set up and deploy your SAG4WD website to the Contabo server.

## Pre-Deployment Setup

### 1. Generate SSH Keys

```bash
# On your local machine
ssh-keygen -t ed25519 -C "github-deploy@sag4wd.com" -f ~/.ssh/sag4wd_deploy

# This creates two files:
# - ~/.ssh/sag4wd_deploy (private key)
# - ~/.ssh/sag4wd_deploy.pub (public key)
```

### 2. Add Public Key to Contabo Server

```bash
# Copy the public key to your server
ssh-copy-id -i ~/.ssh/sag4wd_deploy.pub user@your-contabo-server.com

# Or manually:
# 1. Login to your server
# 2. Add the public key to ~/.ssh/authorized_keys
cat ~/.ssh/sag4wd_deploy.pub | ssh user@your-contabo-server.com "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### 3. Set Up GitHub Repository Secrets

Navigate to: `Settings → Secrets and variables → Actions → New repository secret`

Add these secrets:

- [ ] **SSH_PRIVATE_KEY**
  - Copy the entire content of `~/.ssh/sag4wd_deploy` (private key)
  - Command: `cat ~/.ssh/sag4wd_deploy | pbcopy` (macOS) or `cat ~/.ssh/sag4wd_deploy`

- [ ] **SSH_HOST**
  - Your Contabo server IP or hostname
  - Example: `123.45.67.89` or `sag4wd.com`

- [ ] **SSH_USER**
  - SSH username for your server
  - Usually: `sag4wd` or `root`

- [ ] **SSH_PORT** (optional)
  - SSH port (default: 22)
  - Only add if using non-standard port

## Server Setup

### 4. Prepare Server Directories

```bash
# SSH into your Contabo server
ssh user@your-contabo-server.com

# Create necessary directories
mkdir -p /home/sag4wd/public_html
mkdir -p /home/sag4wd/backups

# Set proper ownership
chown -R sag4wd:sag4wd /home/sag4wd
```

### 5. Configure Apache

```bash
# Enable required Apache modules
sudo a2enmod rewrite
sudo a2enmod deflate
sudo a2enmod expires
sudo a2enmod headers

# Restart Apache
sudo systemctl restart apache2

# Verify Apache is running
sudo systemctl status apache2
```

### 6. Configure Virtual Host (if needed)

```bash
# Edit or create Apache virtual host configuration
sudo nano /etc/apache2/sites-available/sag4wd.conf
```

Add this configuration:

```apache
<VirtualHost *:80>
    ServerName sag4wd.com
    ServerAlias www.sag4wd.com
    DocumentRoot /home/sag4wd/public_html

    <Directory /home/sag4wd/public_html>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/sag4wd_error.log
    CustomLog ${APACHE_LOG_DIR}/sag4wd_access.log combined
</VirtualHost>
```

```bash
# Enable the site
sudo a2ensite sag4wd.conf

# Reload Apache
sudo systemctl reload apache2
```

### 7. Set Up SSL Certificate (Optional but Recommended)

```bash
# Install Certbot
sudo apt update
sudo apt install certbot python3-certbot-apache

# Get SSL certificate
sudo certbot --apache -d sag4wd.com -d www.sag4wd.com

# Certbot will automatically configure Apache for HTTPS
```

## Initial Deployment

### 8. Test Deployment

- [ ] Push code to `main` or `master` branch
- [ ] Go to GitHub Actions tab
- [ ] Monitor the "Deploy to Contabo Server" workflow
- [ ] Check for any errors

### 9. Verify Deployment

```bash
# SSH into your server
ssh user@your-contabo-server.com

# Check deployed files
ls -la /home/sag4wd/public_html/

# You should see:
# - index.html
# - .htaccess
# - sitemap.xml
# - robots.txt
# - css/
# - js/
# - assets/
```

### 10. Test Website

- [ ] Open browser and visit: `http://your-server-ip` or `https://sag4wd.com`
- [ ] Check homepage loads correctly
- [ ] Verify CSS and JavaScript are working
- [ ] Check mobile responsiveness
- [ ] Test navigation links
- [ ] Verify sitemap: `https://sag4wd.com/sitemap.xml`
- [ ] Verify robots.txt: `https://sag4wd.com/robots.txt`

## Post-Deployment Verification

### 11. Performance Checks

- [ ] **Compression Test**
  ```bash
  curl -H "Accept-Encoding: gzip" -I https://sag4wd.com
  # Look for: Content-Encoding: gzip
  ```

- [ ] **Cache Headers Test**
  ```bash
  curl -I https://sag4wd.com/css/style.css
  # Look for: Cache-Control headers
  ```

- [ ] **Security Headers Test**
  ```bash
  curl -I https://sag4wd.com
  # Look for: X-Content-Type-Options, X-Frame-Options, etc.
  ```

### 12. SEO Verification

- [ ] Submit sitemap to Google Search Console
- [ ] Submit sitemap to Bing Webmaster Tools
- [ ] Verify robots.txt is accessible
- [ ] Check meta tags in page source

## Maintenance

### 13. Regular Tasks

- [ ] Monitor GitHub Actions for deployment failures
- [ ] Check Apache logs for errors: `sudo tail -f /var/log/apache2/error.log`
- [ ] Review server backups in `/home/sag4wd/backups/`
- [ ] Update sitemap.xml when adding new pages
- [ ] Keep server software updated: `sudo apt update && sudo apt upgrade`

### 14. Backup Management

Backups are automatically created on each deployment in `/home/sag4wd/backups/`

To restore a backup:
```bash
cd /home/sag4wd/backups
ls -lt  # List backups
tar -xzf backup_YYYYMMDD_HHMMSS.tar.gz -C /home/sag4wd/
```

## Troubleshooting

### Deployment fails

1. Check GitHub Actions logs for specific error
2. Verify SSH credentials in GitHub secrets
3. Test SSH connection manually: `ssh -i ~/.ssh/sag4wd_deploy user@server`
4. Check server disk space: `df -h`

### Website not loading

1. Check Apache status: `sudo systemctl status apache2`
2. Check Apache error log: `sudo tail -f /var/log/apache2/error.log`
3. Verify file permissions: `ls -la /home/sag4wd/public_html`
4. Check .htaccess syntax: `apache2ctl configtest`

### .htaccess not working

1. Verify AllowOverride is enabled in Apache config
2. Check if required modules are enabled: `apache2ctl -M`
3. Enable modules if needed:
   ```bash
   sudo a2enmod rewrite deflate expires headers
   sudo systemctl restart apache2
   ```

## Security Checklist

- [ ] Use strong SSH key (ed25519 or RSA 4096-bit)
- [ ] Never commit private keys to repository
- [ ] Use GitHub secrets for sensitive data
- [ ] Keep server software updated
- [ ] Enable firewall (ufw) on server
- [ ] Use HTTPS (SSL certificate)
- [ ] Regularly review server access logs
- [ ] Keep backups secure

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [Certbot Documentation](https://certbot.eff.org/)
- [Web Performance Best Practices](https://web.dev/performance/)

---

**Repository:** sitejet-sag4wd
