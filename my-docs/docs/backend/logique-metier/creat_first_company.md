---
sidebar_position: 1
---


# 📄 Processus de création publique d'une entreprise

### 🔒 Accès

* **Endpoint public** (accessible sans authentification)
* But : enregistrer une nouvelle entreprise dont le numéro de TVA n'existe pas encore dans la base.

---

### 🧾 Étapes de création

#### 1. **L'utilisateur remplit un formulaire de création**

**Champs requis :**

| Champ          | Description                      |
| -------------- | -------------------------------- |
| `company_name` | Nom de l'entreprise              |
| `vat_number`   | Numéro de TVA (doit être unique) |
| `first_name`   | Prénom du référent               |
| `last_name`    | Nom du référent                  |
| `city`         | Ville de l’entreprise            |
| `address`      | Adresse de l’entreprise          |
| `postal_code`  | Code postal de l’entreprise      |
| `company_type` | `carrier` ou `freight_forwarder` |

> 📧 Aucun champ lié à l’utilisateur (email, mot de passe, téléphone) n’est encore fourni ici. Cela interviendra plus tard.

---

#### 2. **Vérification**

* Si le `vat_number` existe déjà → retour `409 Conflict` avec message d’erreur.
* Sinon, on continue.

---

#### 3. **Création d’une `BusinessPartner` avec statut `PRE_REGISTERED`**

```sql
INSERT INTO business_partners (...) VALUES (..., status = 'PRE_REGISTERED')
```

---

#### 4. **Création d’un `User` de type “référent”**

* Créé **sans mot de passe**.
* Son `status_user` est aussi mis à `PRE_REGISTERED`.
* Il est lié à l’entreprise via `UserBusinessPartner` avec :

  * `is_primary = True`
  * rôle par défaut selon le type d’entreprise :

    * `ADMIN_CARRIER` si `company_type = carrier`
    * `ADMIN_FREIGHTFORWARDER` si `company_type = freight_forwarder`

---

#### 5. **Envoi d’un email de confirmation**

* Contient un lien sécurisé (ex: `/complete-registration?token=xxx`) permettant au référent de :

  * définir un mot de passe
  * renseigner ses informations personnelles (email, téléphone, etc.)
  * éventuellement **uploader les documents justificatifs** de l’entreprise

---

### ✅ Objectifs métier atteints

* ✅ Pas de création de doublon de TVA
* ✅ Aucune authentification requise pour initier le process
* ✅ Contrôle des rôles par défaut
* ✅ Activation contrôlée via validation d’email
* ✅ Statuts `PRE_REGISTERED` permettent de distinguer les entités "en attente"

---

### 🔄 Statuts prévus à gérer

| Élément                  | Statut           | Description                                     |
| ------------------------ | ---------------- | ----------------------------------------------- |
| `User.status_user`       | `PRE_REGISTERED` | Pas encore activé ni mot de passe               |
|                          | `APPROVED`       | Utilisateur actif                               |
|                          | `REJECTED`       | Refusé manuellement                             |
| `BusinessPartner.status` | `PRE_REGISTERED` | Créée mais documents manquants ou en validation |
|                          | `APPROVED`       | Société validée par le back-office              |
|                          | `REJECTED`       | Société refusée                                 |



