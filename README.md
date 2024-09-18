Voici une version adaptée du **README** en fonction des changements dans le fichier `defaults/main.yml`.

# Ansible Role: restic-exporter

Ce rôle Ansible déploie et configure **restic-exporter**, un exportateur Prometheus pour le système de sauvegarde **Restic**. Le rôle gère l'installation du projet, la configuration des dépendances, la gestion des secrets et met en place un service **systemd** pour le processus.

### Prérequis

- Ansible 2.9 ou supérieur
- Python 3 installé sur l'hôte cible
- Un utilisateur avec des droits sudo sur l'hôte cible

### Fonctionnalités principales

- Clone le dépôt Git **restic-exporter** avec une branche et un dépôt personnalisables.
- Crée un environnement virtuel Python pour isoler les dépendances.
- Configure les variables d'environnement pour **Restic** et **AWS**.
- Stocke le mot de passe Restic dans un fichier sécurisé avec des permissions appropriées.
- Crée et configure un service **systemd** pour gérer le processus de **restic-exporter**.

### Variables disponibles

Le rôle est entièrement personnalisable à l'aide des variables suivantes :

- `restic_exporter_repo` : URL du dépôt Git (par défaut : "https://github.com/ngosang/restic-exporter.git").
- `restic_exporter_branch` : Branche à cloner (par défaut : "main").
- `restic_exporter_path` : Chemin local où cloner le projet (par défaut : "/opt/restic-exporter").
- `restic_exporter_venv` : Chemin de l'environnement virtuel (par défaut : "/opt/restic-exporter-venv").
- `restic_password_file` : Chemin vers le fichier contenant le mot de passe Restic (par défaut : "{{ ansible_env.HOME }}/.restic_password").
- `restic_repository` : URL ou chemin local du dépôt Restic (par défaut : "/data").
- `restic_password` : Mot de passe pour accéder au dépôt Restic (par défaut : "your-default-password").
- `restic_aws_access_key_id` : Clé d'accès AWS (par défaut : "your-aws-access-key-id").
- `restic_aws_secret_access_key` : Clé secrète AWS (par défaut : "your-aws-secret-access-key").

### Exemple d'utilisation

Voici un exemple de playbook utilisant ce rôle :

```yaml
---
- hosts: restic-exporter
  become: yes
  roles:
    - role: restic-exporter
      vars:
        restic_repository: "/backups/restic-repo"
        restic_password: "super-secret-password"
        restic_aws_access_key_id: "my-aws-access-key-id"
        restic_aws_secret_access_key: "my-aws-secret-key"
```

### Commandes systemd

Le rôle configure un service **systemd** pour **restic-exporter**. Utilisez les commandes suivantes pour gérer le service :

- **Démarrer le service** : `systemctl start restic-exporter`
- **Arrêter le service** : `systemctl stop restic-exporter`
- **Redémarrer le service** : `systemctl restart restic-exporter`
- **Vérifier le statut du service** : `systemctl status restic-exporter`

### Sécurité

Les fichiers contenant des secrets, comme le mot de passe Restic, sont protégés avec des permissions strictes (`0600`), et le service **restic-exporter** est exécuté sous l'utilisateur **root** pour garantir un accès correct aux ressources critiques.

### Contributions

Les contributions sont les bienvenues. Pour signaler un problème ou proposer une amélioration, créez une **issue** ou soumettez une **pull request**.

### Licence

Le rôle est distribué sous la licence **MIT**. Consultez le fichier `LICENSE` pour plus d'informations.
