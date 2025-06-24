# 🔐 concurrencyStamp

Le champ `concurrency_stamp` est utilisé pour éviter les conflits de modification concurrente sur des ressources critiques comme les rôles, les utilisateurs, etc.

## ✨ Objectif
Permettre à un client de détecter si une ressource a été modifiée depuis qu'il l’a chargée.

## 🧠 Principe
- Chaque enregistrement (ex: `Role`) possède un champ `concurrency_stamp` de type UUID.
- Lorsqu’un client veut mettre à jour une ressource, il doit inclure le `concurrency_stamp` courant.
- Si le `concurrency_stamp` en base a changé, la modification est refusée avec une erreur `409 Conflict`.

## 📌 Exemple de flux

1. `GET /api/v1/Roles/123` → retourne un `concurrencyStamp: "abcd-1234"`
2. L’utilisateur modifie le rôle en frontend
3. `PUT /api/v1/Roles/123` avec `"concurrencyStamp": "abcd-1234"`
4. ⚠️ Si la base a `"concurrencyStamp": "efgh-5678"` → erreur `409`

---

