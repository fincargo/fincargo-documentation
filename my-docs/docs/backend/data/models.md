---
sidebar_position: 1
---

# 🔗 Modèles relationnels

Cette section décrit les principaux modèles utilisés dans l’architecture multi-tenant de la plateforme.

---

## 🏢 `BusinessPartner`

Représente une **entreprise** (tenant) dans le système.  
Chaque utilisateur est toujours lié à une ou plusieurs entreprises via la table `UserBusinessPartner`.

> 💡 Même **Fincargo** est une entreprise enregistrée dans cette table, avec un `CompanyType` spécifique.

---

## 🏷️ `CompanyType`

Définit le **type** d’entreprise (ex : classification métier) :

- `carrier`
- `freight_forwarder`
- `fincargo` (interne)

Permet une séparation claire des comportements ou vues selon le type d'entité.

---

## 👥 `UserBusinessPartner`

Table d'association entre :

- un `User`
- un `BusinessPartner`
- un `Role`

C’est ici que se joue l'appartenance d’un utilisateur à une entreprise et le rôle qu’il y occupe.

> ℹ️ Si un utilisateur appartient à plusieurs entreprises, un champ `is_primary` peut désigner l'entreprise principale.

---

## 🛡️ `Role`

Rôles spécifiques assignables à un utilisateur **dans une entreprise**.  
Exemples : `Admin Carrier`, `RH Freight Forwarder`, `Manager`.

- Centralisé dans une table unique
- Réutilisable entre entreprises
- Chaque rôle a un `normalized_name` utilisé dans la sécurité (JWT, RBAC, etc.)

---

## 🌍 `UserRole` (optionnel)

> ⚠️ Actuellement **non utilisé**, mais utile si besoin de rôles globaux.

Tu ne gardes `UserRole` que si :

- un utilisateur peut avoir un rôle **global**, indépendant de l’entreprise  
  (ex : `Super Auditeur`, `Viewer multi-tenants`)
- tu veux accorder des **permissions transversales**, en dehors d’un tenant

---

