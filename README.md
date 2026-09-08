# SecureMailScope 🛡️
> **AI-Assisted Cryptographic Security Posture Assessment for Secure Email Communications**  
> *Developed for National Technical Research Organisation (NTRO) | Smart India Hackathon (SIH 2026)*

---

## 📌 Overview

**SecureMailScope** is a passive, offline-first network forensic application that ingests `.pcap` / `.pcapng` capture files containing SMTP, IMAP, and POP3 mail traffic. It produces a prioritized cryptographic security posture report — combining deterministic protocol/crypto parsing with an AI anomaly detection model for risk scoring and vulnerability identification.

### Key Problem Addressed
Email infrastructure relies on legacy protocols (SMTP, IMAP, POP3) upgraded dynamically via `STARTTLS`. Insecure cipher suites (RC4, 3DES), deprecated protocol versions (TLS 1.0/1.1), expired X.509 certificates, and active **STARTTLS command injection / stripping attacks** routinely bypass generic scanners. SecureMailScope passively inspects traffic streams without active network probing to detect and score these vulnerabilities.

---

## ✨ Features

- **Passive PCAP Ingestion**: Parses offline packet captures (`.pcap`/`.pcapng`/`.json`) with zero live network interaction — ideal for digital forensics and incident response.
- **STARTTLS State Machine & Anomaly Detector**: Reconstructs TCP streams to detect unencrypted command injection before TLS handshakes (CVE-2011-0411 class) and STARTTLS stripping.
- **TLS & X.509 Certificate Examiner**: Decodes negotiated TLS versions, ciphers (RC4, 3DES, AES-GCM, ChaCha20), key exchange (RSA vs. ECDHE), forward secrecy (PFS), certificate expiry, key length (1024-bit vs 2048/4096-bit), and signature algorithms.
- **Hybrid Risk Fusion Engine**: Integrates **NIST SP 800-52r2** and **IETF RFC 8996** compliance rules with statistical AI anomaly scoring to yield an overall **Cryptographic Security Posture Score (0–100)**.
- **Interactive Handshake Timeline**: Renders milestone steps (Plaintext Banner → STARTTLS Request → Server Response → TLS Upgrade) with encryption boundary highlights.
- **Audit-Ready Exporting**: Instant PDF audit report generation (`jspdf`) and SIEM-compatible JSON exports.
- **Pre-Loaded Forensic Scenarios**:
  1. *Vulnerable Legacy Mail Infrastructure* (TLS 1.0, RC4, 1024-bit expired cert, STARTTLS command injection).
  2. *Hardened NTRO Enterprise Infrastructure* (TLS 1.3, ECDHE-P384, MTA-STS).
  3. *Active STARTTLS Stripping Attack* (Cleartext credential interception).

---

## 🚀 Quick Start

### Prerequisites
- [Node.js](https://nodejs.org/) (v18+) & `npm`

### Installation & Local Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/securemailscope.git

# Navigate into the project directory
cd securemailscope

# Install dependencies
npm install

# Start the Vite development server
npm run dev
```

The application will be accessible locally at `http://localhost:5173/`.

### Build for Production

```bash
npm run build
```

---

## 🏗️ Architecture

```
src/
├── engine/
│   ├── starttlsStateMachine.ts   # Plaintext mail command parser & injection detector
│   ├── tlsParser.ts              # TLS Handshake & X.509 certificate validator
│   └── riskFusionEngine.ts       # NIST SP 800-52r2 + RFC 8996 + AI Risk Fusion Engine
├── components/
│   ├── Navbar.tsx                # Header bar, live tap status, dataset switcher, upload
│   ├── PostureOverviewCard.tsx   # Cryptographic Security Posture Score (0-100) & sub-scores
│   ├── TimelineVisualizer.tsx    # Reconstructed TCP cryptographic timeline
│   ├── SessionsTable.tsx         # Searchable/filterable forensic sessions table
│   ├── SessionDetailModal.tsx    # Deep packet inspector with raw hex/ASCII payload dumps
│   ├── AnalyticsCharts.tsx       # Recharts analytics (Risk Donut, TLS Version, Cipher Suites)
│   └── ReportExportModal.tsx     # PDF audit & SIEM JSON export modal
├── data/
│   └── samplePcaps.ts            # Realistic pre-loaded forensic PCAP datasets
└── types/
    └── pcap.ts                   # Core TypeScript domain models
```

---

## 🛡️ Standards Compliance & References

- **IETF RFC 8996**: Deprecating TLS 1.0 and TLS 1.1
- **NIST SP 800-52r2**: Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations
- **IETF RFC 7465**: Prohibiting RC4 Cipher Suites
- **CVE-2011-0411**: STARTTLS Command Injection Vulnerability Class
- **IETF RFC 8461**: SMTP MTA Strict Transport Security (MTA-STS)

---

## 📜 License

Distributed under the MIT License. Developed for NTRO Smart India Hackathon (SIH 2026).
