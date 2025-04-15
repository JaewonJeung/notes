# crypto plan

- Blog review. One blog per day. Review if needed
- serious crypto 
- Bulletproof TLS

## PQC
### G
- 24/03/11 https://bughunters.google.com/blog/5108747984306176/google-s-threat-model-for-post-quantum-cryptography
    - [[Google threat model PQC]]
    - Recommendation is to use classical/PQC in hybrid fashion so that the attacker has to break both
    - Uses cases: 
        - Encrypt in transit. ephemeral PQC for init key agreement
        - PKI. Need community consensus. Currently expensive deployment due to signature size
        - Firmware sign. Devices with long life-span need urgent replacement
        - Software sign. Hybrid approach
- 24/05/21 https://bughunters.google.com/blog/5266882047639552/why-hybrid-deployments-are-key-to-secure-pqc-migration
- 24/05/23 https://blog.chromium.org/2024/05/advancing-our-amazing-bet-on-asymmetric.html
- 24/06/24 https://bughunters.google.com/blog/6182336647790592/cryptographic-agility-and-key-rotation
- 24/08/13 https://security.googleblog.com/2024/08/post-quantum-cryptography-standards.html
- 24/08/19 https://bughunters.google.com/blog/6038863069184000/formally-verified-post-quantum-algorithms

### M
- 24/05/22 https://engineering.fb.com/2024/05/22/security/post-quantum-readiness-tls-pqr-meta/
- 24/08/28 https://engineering.fb.com/2024/08/28/security/post-quantum-cryptography-meta/
- 24/11/12 https://engineering.fb.com/2024/11/12/security/how-meta-built-large-scale-cryptographic-monitoring/

### AMZN
- 24/12/05 https://aws.amazon.com/blogs/security/aws-post-quantum-cryptography-migration-plan/
- 25/04/07 https://aws.amazon.com/blogs/security/ml-kem-post-quantum-tls-now-supported-in-aws-kms-acm-and-secrets-manager/

### Cloudflare 
- 21/11/08 https://blog.cloudflare.com/sizing-up-post-quantum-signatures/
- 25/03/21 https://blog.cloudflare.com/lattice-crypto-primer/

### Signal
- 23/09/19 https://signal.org/blog/pqxdh/
- 23/05/24 https://signal.org/docs/specifications/pqxdh/

### A
- 24/02/21 https://security.apple.com/blog/imessage-pq3/