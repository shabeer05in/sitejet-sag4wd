# SAG4WD Website

SAG4WD website - Your premier off-road vehicle parts and customization specialist in Oman. Built with Sitejet CMS featuring custom mega menu, parts grid, and automated deployment to Contabo server.

## 🚀 Features

- **Clean Folder Structure**: Organized HTML, CSS, JavaScript, and assets
- **Responsive Design**: Mobile-friendly layout
- **Automated Deployment**: GitHub Actions workflow for SSH deployment to Contabo
- **Performance Optimized**: 
  - Gzip compression
  - Browser caching headers
  - Lazy loading images
- **SEO Ready**: 
  - Sitemap.xml
  - Robots.txt
  - Meta tags
  - Open Graph tags

## 📁 Project Structure

```
sitejet-sag4wd/
├── .github/
│   └── workflows/
│       └── deploy.yml          # Automated deployment workflow
├── assets/
│   ├── images/                 # Image assets
│   └── fonts/                  # Font files
├── css/
│   └── style.css               # Main stylesheet
├── js/
│   └── main.js                 # Main JavaScript file
├── .htaccess                   # Apache configuration (caching, compression, security)
├── index.html                  # Homepage
├── sitemap.xml                 # SEO sitemap
├── robots.txt                  # Search engine directives
└── README.md                   # This file
```

## 🛠️ Setup & Configuration

### Prerequisites

- GitHub repository access
- Contabo server with SSH access
- Apache web server running on Contabo

### GitHub Secrets Configuration

Add the following secrets to your GitHub repository (Settings → Secrets and variables → Actions):

1. **SSH_PRIVATE_KEY**: Your SSH private key for server access
2. **SSH_HOST**: Your Contabo server hostname or IP address
3. **SSH_USER**: SSH username (typically 'sag4wd' or 'root')
4. **SSH_PORT**: SSH port (default: 22)

#### Generating SSH Keys

```bash
# On your local machine
ssh-keygen -t ed25519 -C "github-deploy@sag4wd.com" -f ~/.ssh/sag4wd_deploy

# Copy public key to server
ssh-copy-id -i ~/.ssh/sag4wd_deploy.pub user@your-server.com

# Copy private key content for GitHub secret
cat ~/.ssh/sag4wd_deploy
```

### Server Setup

Ensure your Contabo server has the following directory structure:

```bash
/home/sag4wd/
├── public_html/    # Web root (deployed files go here)
└── backups/        # Automatic backups (created by deployment)
```

## 🚀 Deployment

### Automatic Deployment

The website automatically deploys to the Contabo server when you push to the `main` or `master` branch.

```bash
git add .
git commit -m "Update website content"
git push origin main
```

### Manual Deployment

Trigger a manual deployment from GitHub:
1. Go to Actions tab
2. Select "Deploy to Contabo Server" workflow
3. Click "Run workflow"

### Deployment Process

The deployment workflow:
1. ✅ Checks out the latest code
2. ✅ Updates sitemap.xml with current date
3. ✅ Creates a deployment package (excludes .git, .github, etc.)
4. ✅ Creates a backup of current deployment on server
5. ✅ Uploads and extracts files to `/home/sag4wd/public_html`
6. ✅ Sets proper file permissions
7. ✅ Verifies deployment success
8. ✅ Keeps last 5 backups for rollback

## 🔧 Local Development

### Running Locally

You can use any local web server. Here are a few options:

**Using Python:**
```bash
# Python 3
python3 -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000
```

**Using Node.js:**
```bash
npx http-server -p 8000
```

**Using PHP:**
```bash
php -S localhost:8000
```

Then visit: `http://localhost:8000`

## 🎨 Customization

### Adding New Pages

1. Create a new HTML file (e.g., `products.html`)
2. Add navigation link in `index.html` header
3. Update `sitemap.xml` with the new page URL
4. Commit and push changes

### Updating Styles

- Edit `css/style.css` for global styles
- CSS variables are defined in `:root` for easy theming

### Adding JavaScript Functionality

- Edit `js/main.js` for custom functionality
- Keep code modular and well-commented

## 📊 Performance Features

### Caching

The `.htaccess` file includes optimized caching headers:
- Images, fonts, media: 1 year
- CSS, JavaScript: 1 month
- HTML: No cache (always fresh)

### Compression

Gzip compression is enabled for:
- HTML, CSS, JavaScript
- XML, JSON
- SVG images
- Web fonts

### Security

Security headers included:
- X-Content-Type-Options: nosniff
- X-Frame-Options: SAMEORIGIN
- X-XSS-Protection: 1; mode=block
- Referrer-Policy: strict-origin-when-cross-origin

## 🔍 SEO

### Sitemap

The `sitemap.xml` is automatically updated with the current date on each deployment. Update page priorities and change frequencies as needed.

### Robots.txt

Configure search engine crawling rules in `robots.txt`.

## 🐛 Troubleshooting

### Deployment Fails

1. Check GitHub Actions logs
2. Verify SSH credentials in GitHub secrets
3. Ensure server is accessible via SSH
4. Check server disk space

### Website Not Loading

1. Verify files are in `/home/sag4wd/public_html`
2. Check Apache configuration
3. Verify file permissions (directories: 755, files: 644)
4. Check Apache error logs: `tail -f /var/log/apache2/error.log`

### .htaccess Not Working

1. Ensure `AllowOverride All` is set in Apache config
2. Enable required Apache modules:
   ```bash
   sudo a2enmod rewrite deflate expires headers
   sudo systemctl restart apache2
   ```

## 📝 Maintenance

### Backups

- Automatic backups are created on each deployment
- Stored in `/home/sag4wd/backups/`
- Last 5 backups are kept automatically

### Rollback

To rollback to a previous version:

```bash
ssh user@your-server.com
cd /home/sag4wd/backups
# List backups
ls -lt
# Restore a backup
tar -xzf backup_YYYYMMDD_HHMMSS.tar.gz -C /home/sag4wd/
```

## 📞 Support

For issues or questions:
- Create an issue in this repository
- Contact: info@sag4wd.com

## 📄 License

Copyright © 2025 SAG4WD. All rights reserved.
