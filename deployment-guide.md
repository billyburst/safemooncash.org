# Deploying Safemoon Cash Website to Namecheap

## Method 1: FTP/File Manager Upload (Easiest)

### Steps:
1. Log into your Namecheap account
2. Go to cPanel (usually at yourdomain.com/cpanel)
3. Click on **File Manager**
4. Navigate to `public_html` folder (or `public_html/subdomain` if using subdomain)
5. Delete any default index.html files
6. Upload your 3 files:
   - index.html
   - about.html
   - contact.html
7. Visit your domain to see the site live!

### To Update Later:
- Simply re-upload the files you changed
- Or use "Edit" in File Manager to make small changes

---

## Method 2: Git Deployment via SSH (Professional)

This method lets you update your site by running `git pull` instead of manually uploading files.

### Prerequisites:
- SSH access enabled on your Namecheap hosting
- Git installed on your hosting (most modern hosts have this)

### Initial Setup:

1. **Enable SSH Access:**
   - Log into Namecheap cPanel
   - Find "SSH Access" or "Terminal"
   - Enable SSH if not already enabled

2. **Get Your SSH Details:**
   - Host: Usually your domain or server IP
   - Username: Your cPanel username
   - Port: Usually 22
   - Password: Your cPanel password

3. **Connect via SSH:**
   ```bash
   ssh username@yourdomain.com
   # or
   ssh username@server-ip-address
   ```

4. **Navigate to Web Root:**
   ```bash
   cd public_html
   ```

5. **Clone Your Repository:**
   ```bash
   # If public repo:
   git clone https://github.com/billyburst/safemooncash.org.git .

   # If private repo (need personal access token):
   git clone https://YOUR_TOKEN@github.com/billyburst/safemooncash.org.git .
   ```

6. **Switch to Your Branch:**
   ```bash
   git checkout claude/recreate-safemoon-cash-site-011CUN9vCAf8pHF56C3gfxbm
   ```

### To Update Your Site Later:
```bash
# SSH into your server
ssh username@yourdomain.com

# Navigate to site folder
cd public_html

# Pull latest changes
git pull origin claude/recreate-safemoon-cash-site-011CUN9vCAf8pHF56C3gfxbm
```

---

## Method 3: Automated Deployment (Advanced)

Set up GitHub Actions to automatically deploy when you push changes.

### Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Namecheap

on:
  push:
    branches:
      - claude/recreate-safemoon-cash-site-011CUN9vCAf8pHF56C3gfxbm

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Deploy to server
        uses: SamKirkland/FTP-Deploy-Action@4.3.0
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ./
          server-dir: /public_html/
```

Then add secrets in GitHub:
- Settings > Secrets > Actions > New repository secret
- Add: FTP_SERVER, FTP_USERNAME, FTP_PASSWORD

---

## Recommended Approach

**For Quick Setup:**
→ Use Method 1 (FTP Upload)

**For Ongoing Development:**
→ Use Method 2 (Git Deployment)

**For Multiple Developers/Automation:**
→ Use Method 3 (GitHub Actions)

---

## Troubleshooting

### Issue: Files uploaded but site not showing
- **Solution:** Make sure files are in `public_html` not in a subfolder
- Check file permissions (should be 644 for HTML files)

### Issue: SSH access not working
- **Solution:** Enable SSH in cPanel under "SSH Access"
- Contact Namecheap support to enable SSH on your hosting plan

### Issue: Git not found on server
- **Solution:** Ask Namecheap support to install Git
- Or use FTP method instead

---

## File Structure on Server

Your `public_html` folder should look like:
```
public_html/
├── index.html
├── about.html
└── contact.html
```

**Important:** Delete any default `index.html` or `index.php` files that Namecheap creates.

---

## Domain Configuration

Make sure your domain DNS is pointing to your Namecheap hosting:
1. Go to Namecheap Dashboard
2. Domain List > Manage > Advanced DNS
3. Add A Record pointing to your server IP
4. Wait 24-48 hours for DNS propagation

---

## Need Help?

- Namecheap Support: https://www.namecheap.com/support/
- Namecheap Knowledge Base: https://www.namecheap.com/support/knowledgebase/
