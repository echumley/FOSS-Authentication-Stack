# FOSS-Authentication-Stack
This project aims to create a free, open-source replacement for the Windows Active Directory software suite for mixed Linux and Windows environments. This suite is entirely Linux-based and offers Active Directory-style services like Kerberos-based authentication, SSO, and user/group management.

## 🛠️ Components

| Component       | Purpose |
|----------------|---------|
| **FreeIPA**    | LDAP, Kerberos, Host-Based Access Control (HBAC), Certificate Authority (CA) |
| **Keycloak**   | OIDC/SAML authentication, MFA, and Web SSO |
| **SSSD**       | System authentication for Linux clients via FreeIPA |
| **Traefik**    | Reverse proxy for secure routing of web applications |
| **CrowdSec**   | Behavior-based threat detection and protection |

---

## 🔑 Authentication Flow

1. **User requests access** → Routed through **Traefik**.
2. **If authentication is required**, request is sent to **Keycloak**.
3. **Keycloak validates credentials** via **FreeIPA (LDAP/Kerberos)**.
4. **Keycloak issues an OIDC/SAML token** → User gets authenticated.
5. **For Linux clients**, SSSD enforces authentication and access control via **FreeIPA**.
6. **CrowdSec analyzes traffic** for suspicious activity (e.g., brute-force attempts).

---

## 📌 Deployment Order

1️⃣ **Install FreeIPA** (LDAP, Kerberos, HBAC, CA)
2️⃣ **Configure FreeIPA Policies** (Users, Groups, Host-Based Access Control)
3️⃣ **Deploy Keycloak** (Integrate with FreeIPA via LDAP, enable OIDC/SAML)
4️⃣ **Set Up SSSD on Linux Clients** (Authenticate via FreeIPA)
5️⃣ **Deploy Traefik** (Reverse Proxy for secure web app routing)
6️⃣ **Deploy CrowdSec** (Monitor and block malicious traffic)

---

## 📜 Architecture Overview
```
                        ┌────────────────────────────┐
                        │       External User        │
                        └────────────┬───────────────┘
                                     │
                                     ▼
                        ┌────────────────────────────┐
                        │        Traefik (Proxy)     │  🔒 Handles SSL, routes traffic
                        └────────────┬───────────────┘
                                     │
        ┌────────────────────────────┼────────────────────────────┐
        ▼                            ▼                            ▼
┌───────────────────┐       ┌───────────────────┐       ┌───────────────────┐
│  Web Applications │       │   Keycloak (SSO)  │       │  FreeIPA (LDAP)   │
│  (Moodle, GitLab) │       │  OIDC/SAML Auth   │       │   Kerberos, HBAC  │
└───────────────────┘       └───────────────────┘       └───────────────────┘
               │                      │                           │
               ▼                      ▼                           ▼
      ┌─────────────────┐    ┌─────────────────────┐    ┌───────────────────┐
      │  Auth Redirect  │───▶│ LDAP Authentication │───▶│ Identity Services │
      └─────────────────┘    └─────────────────────┘    └───────────────────┘
                                      ▲
                                      │
                        ┌────────────────────────────┐
                        │     Linux Clients (SSSD)   │  🔒 HBAC rules applied
                        └────────────────────────────┘
                                      ▲
                                      │
                        ┌────────────────────────────┐
                        │       CrowdSec (WAF)       │  🔒 Blocks brute force attacks
                        └────────────────────────────┘
```

---

## How It Works:

1️⃣ **FreeIPA as Central Authentication Server**  
   FreeIPA will handle:
   - LDAP directory services: Stores users, groups, and their attributes.
   - Kerberos: Provides Single Sign-On (SSO) and integrated authentication for Linux clients.
   - Host-Based Access Control (HBAC): Manages which users can access which Linux hosts, based on group membership or roles.
   - Certificate Authority (CA): Issues internal certificates for encrypted communications.

2️⃣ **Keycloak for Web SSO (OIDC/SAML)**  
   Keycloak is used to manage modern web applications:
   - OAuth2 / OpenID Connect (OIDC) and SAML authentication protocols.
   - Users will authenticate against FreeIPA’s LDAP backend for credentials, but web apps will use Keycloak for SSO.
   - Can enforce MFA (TOTP, WebAuthn) for additional security.

3️⃣ **User & Group Management**  
   **FreeIPA manages:**
   - User accounts: Create, modify, or delete users.
   - Group management: Assign users to groups, and define roles/policies.
   - Access Control: Using HBAC, define which groups can access which machines.

4️⃣ **Access Control & Policies**
   - Use HBAC policies in FreeIPA to ensure only the correct users or groups can access specific systems.
   - Leverage SSSD (System Security Services Daemon) on Linux machines to enforce centralized authentication and manage sudoers, group memberships, and access control.
   - Ansible can be used to enforce configuration policies across your Linux machines.

---

## Key Features:

✅ **Complete Authentication & Identity Management**
   - FreeIPA provides a comprehensive solution for managing users, groups, hosts, and access control for Linux systems.
   - Kerberos is used to provide SSO across Linux systems, ensuring a seamless authentication experience.

✅ **Modern Web Application Authentication**
   - With Keycloak, you can enable OIDC/SAML for web apps, providing SSO across a wide range of services, including Docker containers, Kubernetes, and other cloud-native applications.

✅ **MFA & Secure Authentication**
   - Both FreeIPA and Keycloak can enforce multi-factor authentication (MFA) to enhance security, ensuring that your systems require more than just a password to authenticate.

✅ **Scalable & Easily Automatable**
   - Ansible helps manage configurations and policies across your infrastructure.
   - SSSD integrates seamlessly with Linux systems for user authentication and system access control.

✅ **Centralized Management**
   - FreeIPA’s web UI and CLI allow for efficient user/group management and access control.
   - Keycloak provides a unified platform for managing authentication for both on-premises and cloud applications.

✅ **Windows Support**
   - This stack allows administrators to support both Linux and Windows systems.

---

## Advantages of This Stack

✅ **Open-source and Free**
   - Fully open-source and free for use, with no licensing costs.
   - Actively maintained with a strong community of contributors.

✅ **Complete Authentication & Access Control**
   - Kerberos-based SSO and LDAP integration make it a powerful and secure alternative to Active Directory in Linux environments.

✅ **Modern Web Application Integration**
   - Seamless OIDC/SAML integration with Keycloak enables single sign-on for your entire infrastructure.

✅ **Easily Scalable**
   - This stack is easily scalable for environments with a large number of users and Linux systems. It can also be integrated with cloud services.

--- 

## 🔐 Security Features

✅ **Centralized Authentication & SSO** (Kerberos for Linux, OIDC/SAML for Web Apps)  
✅ **Host-Based Access Control (HBAC)** (Restrict users to specific machines)  
✅ **Multi-Factor Authentication (MFA)** (TOTP, WebAuthn via Keycloak)  
✅ **Automated Security Policies** (SSSD enforces sudo rules, system access)  
✅ **Real-Time Traffic Protection** (CrowdSec analyzes and blocks threats)  
✅ **Comprehensive Logging & Monitoring** (Graylog/ELK for security event tracking)  