# Cybersecurity ## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- Git
- A GitHub ac## 🎯 What You'll Learnount (for deployment)

### Local Development

1. **Clone the repository**

   ```bash
   git clone https://github.com/yourusername/simple-cybersec.git
   cd simple-cybersec
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Serve locally**

   ```bash
   mkdocs serve
   ```

   The site will be available at `http://localhost:8000`

4. **Build the site** (optional)

   ```bash
   mkdocs build
   ```

   Static files will be generated in the `site/` directory.

### GitHub Pages Deployment

This repository is configured for automatic deployment to GitHub Pages using GitHub Actions.

#### First-time setup:

1. **Fork or clone this repository** to your GitHub account

2. **Update configuration**:

   - Edit `mkdocs.yml` and replace placeholder URLs:
     - `site_url: https://yourusername.github.io/simple-cybersec`
     - `repo_url: https://github.com/yourusername/simple-cybersec`
     - `repo_name: yourusername/simple-cybersec`
   - Replace `yourusername` with your actual GitHub username

3. **Enable GitHub Pages**:

   - Go to your repository Settings → Pages
   - Set Source to "Deploy from a branch"
   - Select branch `gh-pages` (this will be created by the first deployment)
   - Click Save

4. **Trigger deployment**:
   - Push any change to the `main` branch
   - GitHub Actions will automatically build and deploy your site
   - Check the Actions tab to monitor deployment progress

#### Manual deployment (alternative):

```bash
# Deploy directly from your local machine
mkdocs gh-deploy
```

### GitHub Codespaces

This repository is fully configured to work with GitHub Codespaces:

1. Click "Code" → "Codespaces" → "Create codespace on main"
2. Wait for the environment to set up
3. Run `pip install -r requirements.txt`
4. Run `mkdocs serve`
5. Open the preview URL when promptedndations

Welcome to **Cybersecurity Foundations** — a beginner-friendly, hands-on guide designed for absolute newcomers. If you've ever wondered how hackers break into systems, how defenders stop them, or how the internet stays secure, this course is your starting point.

This course is written so clearly that even a motivated 16‑year‑old with no technical background can follow along.

## 📖 Documentation

This project is built with [MkDocs](https://www.mkdocs.org/) and the [Material theme](https://squidfunk.github.io/mkdocs-material/). The documentation is automatically deployed to GitHub Pages.

**🌐 [View the full documentation →](https://yourusername.github.io/simple-cybersec/)**ro to Cybersecurity

Welcome to **Intro to Cybersecurity** — a beginner-friendly, hands-on guide designed for absolute newcomers. If you’ve ever wondered how hackers break into systems, how defenders stop them, or how the internet stays secure, this course is your starting point.

This course is written so clearly that even a motivated 16‑year‑old with no technical background can follow along.

---

## 🎯 What You’ll Learn

By the end of this course, you’ll understand:

By the end of this course, you'll understand: 2. **Operating Systems** — Linux & Windows fundamentals for cybersecurity. 3. **Security Fundamentals** — encryption, authentication, firewalls, and more. 4. **Threats & Vulnerabilities** — the many ways attackers exploit systems. 5. **Tools & Labs** — an introduction to Wireshark, Nmap, Burp Suite, and others. 6. **The Security Mindset** — how to think like both an attacker and defender. 7. **Legal & Ethics** — the rules and responsibilities of practicing cybersecurity.

You’ll also complete hands-on labs, capture your first packets, scan local networks, and play simple Capture the Flag (CTF) challenges.

---

## 🛠 Prerequisites

- A computer with internet access.
- Willingness to install **VirtualBox** or **VMware** for running virtual machines (VMs).
- Curiosity and patience — no prior knowledge required!

## 📂 Course Structure

The course is organized into modules:

- **[00. Basics](modules/00_basics/)** — Cybersecurity fundamentals and the CIA triad
- **[01. Networking](modules/01_networking/)** — How devices communicate and network security
- **[02. Operating Systems](modules/02_operating_systems/)** — Linux & Windows fundamentals
- **[03. Security Fundamentals](modules/03_security_fundamentals/)** — Encryption, authentication, firewalls
- **[04. Threats & Vulnerabilities](modules/04_threats_vulnerabilities/)** — Attack vectors and exploits
- **[05. Tools & Labs](modules/05_tools_&_labs/)** — Wireshark, Nmap, Burp Suite, and more
- **[06. Security Mindset](modules/06_security_mindset/)** — Thinking like attackers and defenders
- **[07. Legal & Ethics](modules/07_legal_&_ethics/)** — Responsible disclosure and cybersecurity law

**[Hands-on Labs](simple_labs/)** — Step-by-step practical exercises and CTF challenges.

---

## ⚠️ Safety & Legal Notice

Cybersecurity skills are powerful. Use them responsibly:

- **Do not attack systems you don’t own or have permission to test.**
- Practice only in labs, VMs, and safe environments provided here.
- Read [Legal & Ethics](docs/07-legal-and-ethics.md) before doing hands-on activities.

For safe puzzle practice, we recommend [OverTheWire Wargames](https://overthewire.org/wargames/), starting with **Bandit**.

## 🤝 Contributing

Want to improve this course? We welcome contributions!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test locally with `mkdocs serve`
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

Beginners are especially welcome to contribute!

## 📜 License

This course is licensed under the MIT License — free to use, copy, and share.

---

## 🚀 Next Step

Ready to start learning? **[Visit the full documentation →](https://yourusername.github.io/simple-cybersec/)**
