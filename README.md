# Digital Fortress Lab

This repository documents my self-hosted security lab: a Proxmox-based virtualization stack running on enterprise hardware and fronted by Cisco and SonicWall firewalls. The lab is built and operated as a small production-style environment rather than a casual homelab. It uses VLAN segmentation, deep packet inspection, centralized logging, and a Suricata/SELKS SOC node to tie everything together.

The rack is built around a Dell PowerEdge R710 and a Dell EqualLogic FS7610 pair, backed by an Avid 18-bay storage chassis. A Dell X1052P switch, dual Cat6 patch panels, an OpenGear console manager, and a rackmount KVM provide structured, serviceable access to every device. Perimeter security is handled by Cisco ASA and a SonicWall SRA 4200 for VPN and remote access, with management, server, DMZ, honeypot, and guest networks separated by VLANs.

This project shows how I approach infrastructure: treat network, compute, storage, and security as a single system; design with clear traffic flows and failure boundaries; and document decisions so the environment can be understood, changed, and recovered later. Example configs, diagrams, and runbooks are added over time to demonstrate firewall policy design, incident response workflows, and day-to-day operations in a realistic but safely isolated lab.

All IP addresses, hostnames, keys, and credentials are sanitized examples only. Real secrets and identifying details are never stored in this repository, and all experimentation in the lab is conducted within legal and ethical boundaries, isolated from any production or customer systems.
