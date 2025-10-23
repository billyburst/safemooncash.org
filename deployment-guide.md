# Deploying Safemoon Cash Website to Namecheap

## Method 1: FTP/File Manager Upload (Easiest)

### Steps:
1. Log into your Namecheap account
2. Go to cPanel (usually at yourdomain.com/cpanel)
3. Click on **File Manager**
4. Navigate to `public_html` folder (or `public_html/subdomain` if using subdomain)
5. Delete any default index.html files
6. Upload your HTML files:
   - index.html
   - about.html
   - contact.html
   - gate-io.html
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
   git checkout main
   # Or if using development branch:
   # git checkout claude/setup-github-deployment-011CUQ8T83jukHJYMXnHDckY
   ```

### To Update Your Site Later:
```bash
# SSH into your server
ssh username@yourdomain.com

# Navigate to site folder
cd public_html

# Pull latest changes
git pull origin main
```

---

## Method 3: Automated Deployment (Advanced) ⭐ RECOMMENDED

Set up GitHub Actions to automatically deploy when you push changes. The workflow file is already created at `.github/workflows/deploy.yml`!

### Setup Instructions:

1. **Get Your FTP Credentials from Namecheap:**
   - Log into Namecheap cPanel
   - Look for "FTP Accounts" or "File Manager"
   - Note your FTP details:
     - **Server:** Usually `ftp.yourdomain.com` or your domain name
     - **Username:** Your cPanel username (or FTP account username)
     - **Password:** Your cPanel password (or FTP account password)

2. **Add Secrets to GitHub Repository:**
   - Go to your GitHub repository: https://github.com/billyburst/safemooncash.org
   - Click **Settings** (top menu)
   - In left sidebar, click **Secrets and variables** > **Actions**
   - Click **New repository secret** for each:
     - Name: `FTP_SERVER`, Value: `ftp.yourdomain.com` (your FTP host)
     - Name: `FTP_USERNAME`, Value: Your FTP username
     - Name: `FTP_PASSWORD`, Value: Your FTP password

3. **Test the Deployment:**
   - Make any change to your site (e.g., edit index.html)
   - Commit and push to the `claude/setup-github-deployment-011CUQ8T83jukHJYMXnHDckY` branch or `main` branch
   - Go to **Actions** tab in GitHub to watch the deployment
   - Your site will automatically update in 1-2 minutes!

### How It Works:
- Every time you push to the configured branches, GitHub Actions automatically:
  1. Checks out your code
  2. Connects to your Namecheap hosting via FTP
  3. Uploads all HTML files to `public_html`
  4. Excludes unnecessary files (.git, README, etc.)

### Viewing Deployment Status:
- GitHub repo > **Actions** tab
- Click on any workflow run to see logs
- Green checkmark = successful deployment
- Red X = failed (check logs for details)

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
├── contact.html
└── gate-io.html
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
