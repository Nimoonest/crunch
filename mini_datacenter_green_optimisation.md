# Mini Datacenter Green — Modélisation d’un problème d’optimisation énergétique

**Réduire la consommation énergétique tout en respectant les contraintes de performance, de sécurité et de disponibilité.**

---

## 1. Contexte du projet

Dans le projet **Mini Datacenter Green**, plusieurs services doivent être déployés : sites web clients, DNS, reverse proxy, VPN, supervision, etc. Ces services peuvent être hébergés sur plusieurs plateformes : serveur physique, Raspberry Pi ou serveur externe. L’objectif est de choisir la meilleure répartition des services afin de minimiser la consommation énergétique totale.


---

## 2. Fonction objectif

L’objectif du modèle est de minimiser la consommation énergétique totale du mini-datacenter.

### Formule

```math
min Σ_{j∈J} (P_idle_j · y_j + P_load_j · charge_j)
```

### Signification des termes

- **P_idle_j** : consommation de base de la plateforme *j* lorsqu’elle est activée
- **y_j** : variable binaire indiquant si la plateforme *j* est utilisée
- **P_load_j** : consommation supplémentaire liée à la charge
- **charge_j** : taux d’utilisation de la plateforme *j*


---

## 3. Ensembles et variables

### Ensembles

- **I** : ensemble des services à déployer
- **J** : ensemble des plateformes disponibles
- **Z** : ensemble des zones réseau

### Variables

- **x_ij = 1** si le service *i* est déployé sur la plateforme *j*, 0 sinon
- **y_j = 1** si la plateforme *j* est activée, 0 sinon
- **charge_j ∈ [0,1]** : taux de charge de la plateforme *j*

---

## 4. Contraintes principales

### 4.1 Affectation des services

**Formule :**

```math
Σ_{j∈J} x_ij = 1, ∀ i∈I
```

**Explication :**
Chaque service doit être déployé sur une seule plateforme.

---

### 4.2 Activation des plateformes

**Formule :**

```math
x_ij ≤ y_j, ∀ i∈I, ∀ j∈J
```

**Explication :**
Un service ne peut être placé que sur une plateforme activée.

---

### 4.3 Capacité des ressources

**Formules :**

```math
Σ_i cpu_i x_ij ≤ CPU_j y_j
Σ_i ram_i x_ij ≤ RAM_j y_j
Σ_i sto_i x_ij ≤ STO_j y_j
Σ_i net_i x_ij ≤ NET_j y_j
```

**Explication :**
Les besoins des services ne doivent pas dépasser les capacités de la plateforme.

---

### 4.4 Définition de la charge

**Formule :**

```math
Σ_i load_i x_ij ≤ CPU_j charge_j
0 ≤ charge_j ≤ y_j
```

**Explication :**
La charge représente l’utilisation effective de chaque plateforme.

---

### 4.5 Compatibilité et sécurité réseau

**Formules :**

```math
x_ij ≤ M_ij
x_ij = 0 si zone_j ∉ A_i
```

**Explication :**
Certains services ne peuvent être déployés que sur certaines plateformes ou certaines zones réseau, par exemple DMZ ou LAN.

---

### 4.6 Performance minimale

**Formule :**

```math
Σ_j perf_ij x_ij ≥ perf_i
```

**Explication :**
Chaque service doit respecter un niveau minimal de performance.

---

## 5. Schéma logique simplifié

### Services
- Site web 1
- Site web 2
- DNS
- Reverse Proxy
- VPN
- Monitoring

### Plateformes
- Serveur physique
- Raspberry Pi 1
- Raspberry Pi 2
- Serveur externe

### Zones réseau
- WAN
- DMZ
- LAN

### Idée d’optimisation
L’optimisation consiste à affecter les services aux plateformes tout en respectant les contraintes de ressources, de compatibilité réseau, de performance et de consommation énergétique.


---

## 6. Comparaison de scénarios

### Scénario A : Tout sur serveur physique
- **Avantage** : performance élevée
- **Inconvénient** : consommation importante

### Scénario B : Services légers sur Raspberry Pi
- **Avantage** : faible consommation
- **Inconvénient** : performance limitée

### Scénario C : Architecture hybride
- **Avantage** : compromis entre performance et sobriété énergétique
- **Inconvénient** : configuration plus complexe

---

## 7. Conclusion

Ce modèle permet de comparer plusieurs architectures d’hébergement et d’identifier le meilleur compromis entre consommation énergétique, performance, sécurité et flexibilité. Il transforme le choix technique du mini-datacenter en un problème d’optimisation sous contraintes.



---

## 8. Mots-clés

- Green IT
- Mini Datacenter
- Optimisation énergétique
- Affectation de services
- Contraintes de capacité
- Sécurité réseau
- Performance
- Architecture hybride
