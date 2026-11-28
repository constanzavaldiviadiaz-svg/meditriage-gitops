# Validación de reconciliación automática de Argo CD

**Fecha:** 27/11/2026  
**Issue:** #79 — ADR de estrategia GitOps  
**Entorno:** kind-meditriage  
**Application:** meditriage-dev  
**Revisión desplegada:** bff43b4

## Configuración

- Sincronización automática: habilitada.
- Prune: habilitado.
- SelfHeal: habilitado.
- RespectIgnoreDifferences: habilitado.

## Prueba realizada

Se modificó manualmente la etiqueta `app` del Service `meditriage-api`, desde `meditriage` a `meditriage-test`, sin alterar sus selectores ni puertos.

Argo CD detectó la divergencia y restauró automáticamente el valor declarado en Git, sin intervención manual ni nuevos commits.

## Eventos registrados

```text
OperationStarted    Initiated automated sync
ResourceUpdated     Synced -> OutOfSync
ResourceUpdated     OutOfSync -> Synced
OperationCompleted  Sync operation succeeded
```

## Resultado final

| Componente | Resultado |
|---|---|
| Sincronización | Synced |
| Salud | Healthy |
| Etiqueta del Service | app=meditriage |
| Pod de la API | 1/1 Running |
| SelfHeal | Validado funcionalmente |

## Limitaciones

Prune se encuentra habilitado, pero su comportamiento de eliminación automática todavía no ha sido probado.

La compatibilidad efectiva con HPA deberá verificarse tras integrar FinOps.

No se realizaron mediciones de disponibilidad HTTP durante la prueba.