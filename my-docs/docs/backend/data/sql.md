---
sidebar_position: 2
---

# SQL


## Données de base pour la table permission

```SQL

INSERT INTO permissions (name, description) VALUES
-- TRANSACTIONS
('transactions.all', 'Full access to transactions module'),
('transactions.view', 'View transactions'),
('transactions.create', 'Create a transaction'),
('transactions.update', 'Update a transaction'),
('transactions.delete', 'Delete a transaction'),

-- INVOICES
('invoices.all', 'Full access to invoices module'),
('invoices.view', 'View invoices'),
('invoices.create', 'Create an invoice'),
('invoices.update', 'Update an invoice'),
('invoices.delete', 'Delete an invoice'),

-- CARD
('card.all', 'Full access to card module'),
('card.view', 'View cards'),
('card.create', 'Create a card'),
('card.update', 'Update a card'),
('card.delete', 'Delete a card'),

-- REFERRALS
('referrals.all', 'Full access to referrals module'),
('referrals.view', 'View referrals'),
('referrals.create', 'Create a referral'),
('referrals.update', 'Update a referral'),
('referrals.delete', 'Delete a referral'),

-- USER
('user.all', 'Full access to user module'),
('user.view', 'View users'),
('user.create', 'Create a user'),
('user.update', 'Update a user'),
('user.delete', 'Delete a user'),

-- COMPANY
('company.all', 'Full access to company module'),
('company.view', 'View companies'),
('company.create', 'Create a company'),
('company.update', 'Update a company'),
('company.delete', 'Delete a company'),

-- PERSONAL_INFORMATION
('personal_information.all', 'Full access to personal information module'),
('personal_information.view', 'View personal information'),
('personal_information.create', 'Create personal information'),
('personal_information.update', 'Update personal information'),
('personal_information.delete', 'Delete personal information'),

-- AUDIT
('audit.all', 'Full access to audit module'),
('audit.view', 'View audits'),
('audit.create', 'Create an audit entry'),
('audit.update', 'Update an audit entry'),
('audit.delete', 'Delete an audit entry'),

-- GEOMAP
('geomap.all', 'Full access to geomap module'),
('geomap.view', 'View geomap data'),
('geomap.create', 'Create geomap entry'),
('geomap.update', 'Update geomap entry'),
('geomap.delete', 'Delete geomap entry'),

-- FIND_CARRIER
('find_carrier.all', 'Full access to find carrier module'),
('find_carrier.view', 'View carrier suggestions'),
('find_carrier.create', 'Create carrier search'),
('find_carrier.update', 'Update carrier preferences'),
('find_carrier.delete', 'Delete carrier search'),

-- AI
('ai.all', 'Full access to AI module'),
('ai.view', 'Use AI features'),
('ai.create', 'Create AI queries'),
('ai.update', 'Update AI settings'),
('ai.delete', 'Delete AI data');

```


## Table role permission


```SQL

INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name LIKE '%.all'
WHERE r.normalized_name IN (
  'ADMIN_FINCARGO',
  'ADMIN_CARRIER',
  'ADMIN_FREIGHT_FORWARDER'
)
-- éviter les doublons si la table a déjà des entrées
AND NOT EXISTS (
  SELECT 1
  FROM role_permissions rp
  WHERE rp.role_id = r.id AND rp.permission_id = p.id
);


```