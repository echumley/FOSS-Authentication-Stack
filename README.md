# FOSS-Authentication-Stack
This project aims to create a free, open-source replacement for the Windows Active Directory software suite for mixed Linux and Windows environments. This suite is entirely Linux-based and offers Active Directory-style services like Kerberos-based authentication, SSO, and user/group management.

## Components: Software Choices - Purpose
- **Identity Management & Directory Services:** FreeIPA - Provides LDAP, Kerberos, HBAC, and certificates for Linux.
- **Authentication & SSO:** Keycloak - Provides OAuth2, OIDC, SAML authentication for web apps and modern services.
- **User/Group Management:** FreeIPA - User and group management, including policies and roles.
- **Host-Based Access Control (HBAC):** FreeIPA Enforces who can log in to which hosts based on group membership.
- **Configuration Management:** Ansible + SSSD - Manages Linux configurations, sudo rules, and access control.
- **Multi-Factor Authentication (MFA):** FreeIPA + Keycloak - Enforces MFA for users connecting to Linux machines and web apps.
- **Certificate Authority (CA):** FreeIPA (built-in) - Issues certificates for internal authentication.
- **Logging & Monitoring:** Graylog / ELK Stack - Collect logs and monitor authentication events and errors.

---

## Authentication Diagram

```

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

5️⃣ **Logging & Monitoring**
   - Use a centralized log server (e.g., Graylog or ELK Stack) to collect and monitor authentication events, failed login attempts, and other critical security logs.
   - Ensure that all FreeIPA, Keycloak, and Linux system logs are sent to a central log server for monitoring and auditing.

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

## How to Set It Up
1️⃣ **Install FreeIPA**  
   1. Set up FreeIPA on a central server.  
   2. Enable Kerberos, LDAP, and HBAC in the FreeIPA configuration.  
   3. Configure user/group management policies and host access control rules.

2️⃣ **Install Keycloak**  
   1. Install Keycloak on a separate server or alongside FreeIPA.  
   2. Integrate Keycloak with FreeIPA via LDAP for centralized user authentication.  
   3. Enable OIDC/SAML for web-based SSO.

3️⃣ **Configure MFA**  
   1. Enable MFA in Keycloak to add another layer of security.  
   2. Optionally, configure FreeIPA to enforce TOTP (Time-based One-Time Password) for command-line access (e.g., SSH).

4️⃣ **Use SSSD on Linux Clients**  
   1. Configure SSSD on Linux systems to authenticate against FreeIPA.  
   2. Use HBAC policies to enforce who can log into which machines based on group membership.  
   3. Use SSSD for managing sudo access, PAM (Pluggable Authentication Modules), and system login policies.

5️⃣ **Configure Logging & Monitoring**  
   1. Install and configure Graylog or ELK to collect and store logs from FreeIPA and Keycloak.  
   2. Set up alerts for suspicious activity like failed login attempts or unauthorized access.

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
