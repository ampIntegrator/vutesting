# Organisation des images

## Structure recommandée

```
public/images/
├── logos/           # Logos du site
│   ├── logo-green.svg
│   └── logo-green-scroll.svg
├── icons/           # Icônes
├── illustrations/   # Illustrations
└── photos/          # Photos
```

## Utilisation dans les composants

```vue
<!-- Utilisation simple -->
<img src="/images/logos/logo-green.svg" alt="Logo" />

<!-- Ou avec des paths organisés -->
<img src="/images/icons/user.svg" alt="User icon" />
```

**Important :** Les fichiers dans `public/` sont accessibles directement sans le `/public/` dans le chemin.
