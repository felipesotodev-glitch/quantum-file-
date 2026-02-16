# CI/CD con GitHub Actions y OpenShift

Este repositorio está configurado para desplegar automáticamente en OpenShift cuando se hace push a la rama `master`.

## Configuración inicial

### 1. Obtener el token de OpenShift

```bash
oc whoami -t
```

### 2. Configurar el secret en GitHub

1. Ve a: https://github.com/felipesotodev-glitch/quantum-file-/settings/secrets/actions
2. Click en "New repository secret"
3. Name: `OPENSHIFT_TOKEN`
4. Secret: (pega el token obtenido en el paso 1)
5. Click "Add secret"

## Uso

### Despliegue automático

Cada push a `master` o `main` disparará automáticamente:
1. Compilación del proyecto con Maven
2. Build de imágenes Docker en OpenShift
3. Despliegue de file-ingester y chunk-processor
4. Verificación del estado

### Despliegue manual

También puedes ejecutar el workflow manualmente desde:
https://github.com/felipesotodev-glitch/quantum-file-/actions

## Monitoreo

Ver el progreso de los despliegues en:
- GitHub Actions: https://github.com/felipesotodev-glitch/quantum-file-/actions
- OpenShift Console: https://console-openshift-console.apps.rm1.0a51.p1.openshiftapps.com

## Arquitectura del CI/CD

```
GitHub Push → GitHub Actions
    ↓
Maven Build (Java 21)
    ↓
OpenShift Build (file-ingester)
    ↓
OpenShift Build (chunk-processor)
    ↓
Deployment & Rollout
    ↓
Verification
```

## Troubleshooting

### Error: "OPENSHIFT_TOKEN not found"
Verifica que hayas configurado el secret en GitHub Settings → Secrets → Actions

### Error de permisos en OpenShift
Verifica que tu token tenga permisos para crear builds y deployments en el proyecto `felipesoto-dev`

### Build falla
Revisa los logs en GitHub Actions y verifica que el código compile localmente con `mvn clean package`
