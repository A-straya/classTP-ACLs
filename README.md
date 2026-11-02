# ClassTP-ACLs
# Cisco IOS Access Control Lists (ACL) Lab

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-049FD9?style=for-the-badge&logo=cisco&logoColor=white" alt="Cisco Badge"/>
  <img src="https://img.shields.io/badge/Networking-Routing_%26_Switching-00599C?style=for-the-badge&logo=wise&logoColor=white" alt="Networking Badge"/>
  <img src="https://img.shields.io/badge/Security-Access_Control-D9381E?style=for-the-badge&logo=fortinet&logoColor=white" alt="Security Badge"/>
  <img src="https://img.shields.io/badge/Protocol-IPv4_%2F_IPv6-007ACC?style=for-the-badge&logo=internetarchive&logoColor=white" alt="Protocol Badge"/>
</p>

Ce dépôt contient la documentation, la théorie et les configurations de travaux pratiques concernant le filtrage réseau avec les **Listes de Contrôle d'Accès (ACL)** sur les équipements Cisco IOS.

---

##  Tech Stack & Tools

* **Network Simulator:** Cisco Packet Tracer / GNS3
* **OS Target:** Cisco IOS (Routers & Switches)
* **Protocols Covered:** IPv4, IPv6, TCP, UDP, ICMP, Telnet (23), HTTP (80), HTTPS (443), FTP (20/21)

---

##  Aperçu Théorique des ACL

Les ACL (Access Control Lists) permettent de filtrer et de sécuriser le flux de données en fonction de règles séquentielles d'autorisation (`permit`) ou de blocage (`deny`).

### Principes Clés
1. **Évaluation Séquentielle:** L'IOS analyse les règles de haut en bas. Dès qu'une correspondance est trouvée, l'action s'applique et l'évaluation s'arrête.
2. **Deny Any Implicite:** Tout trafic qui ne correspond à aucune règle explicite est automatiquement rejeté en fin de liste.
3. **Positionnement Stratégique:**
   * **ACL Standard:** À placer au plus près de la **destination**.
   * **ACL Étendue:** À placer au plus près de la **source**.

---

##  Synthèse des Types d'ACL

| Type d'ACL | Plage Numérique | Critères de Filtrage | Emplacement Recommandé |
| :--- | :--- | :--- | :--- |
| **ACL Standard** | `1-99` / `1300-1999` | Adresse IP Source | Près de la destination |
| **ACL Étendue** | `100-199` / `2000-2699` | IP Source/Dest, Protocole, Ports | Près de la source |
| **ACL Nommée** | Alphanumérique | Identique aux versions numérotées | Permet la modification par ligne |
| **ACL IPv6** | Nommée uniquement | IP Source/Dest, Protocole, Ports | Près de la source (`traffic-filter`) |

---

##  Exemples de Configuration (Cisco IOS)

### 1. ACL Standard Numérotée & Nommée (IPv4)
> **Objectif:** Bloquer le sous-réseau `172.16.5.0/24` d'accéder au réseau destination `172.16.2.0/24`.

* **ACL Numérotée:**
```text
R1(config)# access-list 1 deny 172.16.5.0 0.0.0.255
R1(config)# access-list 1 permit any
R1(config)# interface FastEthernet 1/0
R1(config-if)# ip access-group 1 out
