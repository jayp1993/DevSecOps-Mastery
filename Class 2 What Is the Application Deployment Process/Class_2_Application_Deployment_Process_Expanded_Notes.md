# Class 2: What Is the Application Deployment Process?

**Cloud Tech Hacks — Expanded Notes**  
**Focus:** Application deployment fundamentals and ReactJS frontend deployment on an Ubuntu VM using Nginx.

## Learning Objectives

By the end of this class, students should be able to:

- Explain the application deployment process.
- Identify the prerequisites for deployment.
- Understand the roles of a VM, operating system, web server, frontend, backend, and database.
- Explain client-server communication over HTTP/HTTPS.
- Build a ReactJS frontend and deploy its build files to Nginx on Ubuntu.
- Troubleshoot common frontend deployment issues.

---

## 1. What Is Application Deployment?

Application deployment is the process of making an application available in a target environment so that users can access it.

A typical deployment workflow includes:

- Obtaining the application source code.
- Preparing the target server or virtual machine.
- Installing required software and dependencies.
- Building the application, if required.
- Publishing the application files.
- Configuring the web server and network access.
- Testing the application.
- Monitoring and maintaining the deployed version.

### Example from Class 2

The class diagram shows the **HospitalManagementApp UI** source code stored in GitHub. The UI uses ReactJS. A build process generates browser-ready files such as HTML, CSS, JavaScript, and static assets. Nginx on an Ubuntu VM can serve these files to users through a browser.

> **Important:** The class diagram also shows a backend and database. This document's hands-on procedure focuses on deploying the **ReactJS frontend**. Backend and database deployment require their own configuration.

---

## 2. Prerequisites for Application Deployment

### 2.1 Application Code and GitHub

The original class notes reference this repository:

- **Repository:** [HospitalManagementApp-UI](https://github.com/jayp1993/HospitalManagementApp-UI.git)

Key concepts:

- **Repository:** The project location containing source files and version history.
- **Source code:** React components, styles, configuration, and assets written by developers.
- **Git:** A version-control system used to track changes.
- **GitHub:** A platform for hosting Git repositories and collaborating on code.
- **Version/release:** The particular code version selected for deployment.

### 2.2 Virtual Machine (VM)

A virtual machine is a software-based computer with allocated compute, memory, storage, an operating system, and network connectivity.

Before deploying, confirm:

- The VM is running.
- The OS is compatible with the application and deployment plan.
- The VM has sufficient CPU, RAM, and disk space.
- You know the VM's public IP address or DNS name, if public access is intended.
- Required network rules are configured.
- Administrative access is restricted to trusted sources.

### 2.3 Operating System

The original diagram contrasts Linux and Windows and marks Mac separately. For this lab, the target server is **Ubuntu Linux**.

- **Linux:** Commonly used for cloud servers and web hosting.
- **Windows Server:** Often used for workloads that require Windows-specific software or IIS.
- **macOS:** Commonly used as a developer workstation OS; it is not the target OS in this lab.

### 2.4 Web Server Options

| Software | Typical role | Common environment |
|---|---|---|
| Nginx | Serves static frontend files and can act as a reverse proxy | Linux |
| Apache HTTP Server | Serves web content and can proxy requests | Linux |
| IIS | Microsoft web server | Windows |
| Tomcat | Java servlet/container runtime for Java web applications | Linux or Windows |

**Remember:** A VM is the machine; Nginx, Apache, IIS, and Tomcat are software components. They are not interchangeable in every use case.

---

## 3. Client-Server Communication

The original class diagram illustrates a laptop/browser communicating with a server.

Typical request flow:

1. A user enters a URL in a browser.
2. If a domain name is used, the browser resolves it to an IP address.
3. The browser connects to the server endpoint.
4. The browser sends an HTTP or HTTPS request.
5. The server returns web files or routes the request to an application/backend service.
6. The browser renders the page.
7. Further user interactions may generate additional API requests.

### HTTP and HTTPS

- **HTTP:** Commonly uses TCP port `80`.
- **HTTPS:** Commonly uses TCP port `443` and protects traffic with TLS.
- **Production recommendation:** Use HTTPS for user-facing applications.

---

## 4. Frontend, Backend, and Database

The original class diagram labels `HospitalManagementApp-UI` and `HospitalManagementApp-Backend`, and also shows a database.

| Component | Responsibility | Typical communication |
|---|---|---|
| Frontend / UI | Screens, forms, buttons, and user interactions | The browser downloads UI files from a web server |
| Backend / API | Business rules, validation, authentication, and data operations | The frontend sends API requests to backend endpoints |
| Database | Persistent application records | The backend reads and writes data |

Important architecture points:

- The frontend is what users interact with in the browser.
- The backend implements server-side logic and APIs.
- The database stores application data.
- A public frontend endpoint does **not** mean the database should be publicly exposed.
- In a secure design, expose only required backend endpoints and keep the database on a private network where possible.
- The frontend must use the correct backend API URL for API calls to succeed.

---

## 5. ReactJS Frontend Deployment on Ubuntu VM

### Deployment Flow

```text
GitHub Repository
       |
       v
ReactJS Source Code
       |
       v
Install Dependencies
       |
       v
ReactJS Build Process
       |
       v
Build Artifact (HTML, CSS, JavaScript, Assets)
       |
       v
Ubuntu Virtual Machine
       |
       v
Nginx Web Server
       |
       v
Browser / User
```

The following commands are examples. Confirm the project's `README`, `package.json`, Node.js version requirements, build script, and actual output folder before running them.

### Step 1 — Connect to the Ubuntu VM

From a terminal on your local computer, connect using an authorized SSH key:

```bash
ssh -i /path/to/private-key.pem username@VM_PUBLIC_IP
```

- Replace the key path, username, and IP with your actual values.
- Use an authorized key and a valid VM username.
- Never share your private key.

### Step 2 — Update Packages and Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable --now nginx
```

Check the service:

```bash
sudo systemctl status nginx --no-pager
curl -I http://localhost
```

Expected result: Nginx should be active, and the local HTTP request should return an HTTP response.

### Step 3 — Allow HTTP Traffic

In the cloud network security rules:

- Allow inbound TCP port `80` from the intended test sources.
- Check the Ubuntu firewall if it is enabled.
- Do not open all ports.
- Restrict SSH access to trusted sources.
- For production, plan HTTPS on port `443` with a valid TLS certificate.

### Step 4 — Install Git and Clone the Repository

The original class notes reference the following repository:

```bash
sudo apt install git -y
git clone https://github.com/jayp1993/HospitalManagementApp-UI.git
cd HospitalManagementApp-UI
```

If the repository is private, use an approved authentication method. Avoid putting personal access tokens directly into shell history or sharing them with students.

### Step 5 — Inspect the ReactJS Project

```bash
ls -la
cat package.json
```

Check for:

- `README` or project setup instructions.
- `package.json`.
- The required Node.js version.
- The package manager and lock file.
- The build script under `scripts`.
- Any required environment variables.

Do not assume every ReactJS project uses the same build tool or output directory.

### Step 6 — Install a Compatible Node.js Version

Install a Node.js version compatible with the project, following the project's documented requirements. Then verify:

```bash
node --version
npm --version
```

Use the project's recommended Node.js version rather than selecting a version blindly.

### Step 7 — Install Project Dependencies

If the repository contains `package-lock.json`, the usual reproducible npm install command is:

```bash
npm ci
```

If there is no npm lock file, follow the repository's instructions; the project may use:

```bash
npm install
```

Do not run both commands without understanding the project's setup.

### Step 8 — Build the ReactJS Frontend

First, confirm that `package.json` defines a build script. Then run:

```bash
npm run build
```

The build tool determines the output directory. Common examples are:

- `build/`
- `dist/`

Inspect the project after building:

```bash
ls -la
ls -la build dist 2>/dev/null
```

If the build fails:

- Read the first meaningful error message.
- Check Node.js compatibility.
- Check whether dependencies installed successfully.
- Review environment variables and project instructions.
- Confirm that the build script exists.

Do not proceed until the build succeeds and the actual output directory is identified.

### Step 9 — Identify the Nginx Document Root

The default Nginx document root on Ubuntu is commonly `/var/www/html`, but the active site configuration may use another directory.

Inspect enabled sites and configuration:

```bash
sudo find /etc/nginx/sites-enabled -maxdepth 1 -type l -ls
sudo nginx -T
```

Look for the relevant `server` block and its `root` directive.

### Step 10 — Copy the Build Artifact to the Web Root

If the build output directory is `build/` and the active Nginx document root is `/var/www/html`, an example is:

```bash
sudo mkdir -p /var/www/html
sudo cp -r build/. /var/www/html/
sudo chown -R www-data:www-data /var/www/html
```

If the actual output folder is `dist/`, replace `build/` with `dist/`.

Important points:

- Copy the **contents of the build output**, not the entire source repository.
- Confirm the document root before copying.
- Back up existing files before replacing a live deployment.
- Ensure the deployed directory contains `index.html` and the generated assets.

### Step 11 — Configure React Single-Page Application Routing (If Required)

A React single-page application may use client-side routes. If a user directly opens a route such as `/patients`, Nginx may return `404` unless the configuration falls back to `index.html`.

A common Nginx location pattern is:

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

Use this in the correct frontend site's `server` block. If API routes or other services use the same host, configure those paths separately so API requests are not incorrectly rewritten to `index.html`.

### Step 12 — Test Configuration and Reload Nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
curl -I http://localhost
```

- `nginx -t` checks the Nginx configuration syntax.
- Reload Nginx only after the configuration test succeeds.
- `curl -I` checks the local HTTP response headers.

### Step 13 — Test from Your Browser

Open the following URL, replacing the placeholder with your VM's actual public IP:

```text
http://VM_PUBLIC_IP
```

The original class diagram includes this example:

```text
http://20.235.72.144:80
```

This is only the address shown in the class notes; it may no longer be active. Use the IP assigned to your own VM.

Verify that:

- The frontend loads.
- CSS and JavaScript assets load successfully.
- Browser developer tools show no major loading errors.
- UI navigation works.
- Backend API calls work if the backend is configured.

### Step 14 — Verify Frontend-to-Backend Communication

If the UI loads but data does not appear, check:

- The configured backend API base URL.
- Whether the backend service is running.
- DNS resolution and network connectivity.
- Cloud network rules and host firewall rules.
- CORS configuration where applicable.
- Authentication and authorization.
- Browser Developer Tools → Network and Console tabs.

**Security note:** Never put database passwords, private keys, or other secrets in React code. Values bundled into browser-delivered JavaScript can be inspected by users.

---

## 6. Build Artifact vs Source Code

- **Source code:** The original project files developers edit.
- **Build process:** Transforms source code and dependencies into output suitable for deployment.
- **Build artifact:** The output passed to the deployment step.
- **Frontend artifact:** Commonly includes HTML, CSS, JavaScript bundles, images, fonts, and other static assets.
- **Deployment:** Publishes the artifact to the target environment and configures the application to serve it.

For a static React frontend, Nginx serves the generated files. It does not normally execute the React source code as a server-side application.

---

## 7. Common Deployment Problems and Troubleshooting

| Symptom | What to check |
|---|---|
| Browser times out | VM state, public IP, routing, cloud NSG/firewall, and service listener |
| Connection refused | Nginx service status and whether port 80 is listening |
| Default Nginx page appears | Active document root and whether build files were copied to the correct site |
| `404` on a frontend route | URL path, Nginx root, and single-page application fallback |
| CSS or JavaScript is missing | Asset paths, build base/public path, file permissions, and browser Network tab |
| UI loads but API fails | API URL, backend availability, CORS, DNS, network rules, and authentication |
| `502` or `504` | Reverse-proxy destination, backend port, service status, logs, and timeouts |

### Useful Commands

```bash
# Check Nginx service
sudo systemctl status nginx --no-pager

# Validate Nginx configuration
sudo nginx -t

# Inspect recent Nginx service logs
sudo journalctl -u nginx -n 100 --no-pager

# Inspect Nginx error log
sudo tail -n 100 /var/log/nginx/error.log

# Check listening TCP ports
sudo ss -lntp

# Test local HTTP response
curl -I http://localhost

# Check deployed files
ls -la /var/www/html
```

---

## 8. Production Security Checklist

- Use HTTPS with a valid TLS certificate for production.
- Allow only the inbound ports that are required.
- Restrict SSH/RDP administration to trusted sources or a secure access method.
- Keep the OS and web-server packages updated.
- Do not place secrets in frontend source code or browser-delivered bundles.
- Keep database access private and allow only authorized services to connect.
- Use logs and monitoring to investigate failures.
- Keep backups and a known-good release so you can roll back a failed deployment.

---

## 9. Quick Revision Questions

1. What is application deployment?
2. What is the difference between a VM and a web server?
3. What is the role of GitHub in the deployment workflow?
4. What does Nginx do for a ReactJS frontend?
5. Why does a React project commonly need a build step?
6. How can you determine whether the output directory is `build/` or `dist/`?
7. What is the difference between source code and a build artifact?
8. Which port is conventionally used for HTTP? Which for HTTPS?
9. Why might a React route return `404` after deployment?
10. What should you check if the UI loads but backend API requests fail?
11. Why should the database not normally be exposed publicly?
12. Why should you run `sudo nginx -t` before reloading Nginx?

---

## 10. Practical Lab — Student Checklist

- [ ] Connect to an Ubuntu VM using SSH.
- [ ] Install and start Nginx.
- [ ] Configure the required inbound network rule.
- [ ] Clone the approved UI repository.
- [ ] Inspect `README` and `package.json`.
- [ ] Install the compatible Node.js version.
- [ ] Install dependencies using the project's documented method.
- [ ] Run the ReactJS build command.
- [ ] Identify the actual output directory.
- [ ] Copy the build files to the active Nginx document root.
- [ ] Test Nginx configuration and reload it.
- [ ] Open the VM endpoint in a browser.
- [ ] Verify frontend assets and routes.
- [ ] Troubleshoot any errors using browser tools and server logs.

### Evidence to Capture

- Repository URL and project folder.
- Successful build command and output.
- Build artifact directory contents.
- Nginx service status.
- Active web root and deployed files.
- Required network rule for HTTP.
- Frontend open in a browser.
- Any troubleshooting command and the result.

---

**End of Class 2 — Application Deployment Process**
