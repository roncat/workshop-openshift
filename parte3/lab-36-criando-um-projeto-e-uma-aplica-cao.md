#### LAB06 - Storage Persistente {#_lab06_storage_persistente}

Usando a ferramenta OC para conectar ao cluster e visualizar configuracoes de rede

```
1 - Acessa Interface Web
2 - Acessa Deployment config e seleciona Actions -> Add Storage
3 - Cria um Storage Claim
4 - Lista claims usando oc get pvc
5 - Visualiza pod travado
6 - Cria-se pv para dar suporte ao pvc
```

Sintaxe criacao PV

```
apiVersion: v1
kind: PersistentVolume
metadata:
 name: <login>
spec:
 capacity:
   storage: 5Gi
 accessModes:
 - ReadWriteOnce
 nfs:
   path: /exports/pvs/<login>
   server: nfs.example.com
 persistentVolumeReclaimPolicy: Recycle
```

Para criacao do pv:

```
oc create -f pv.yaml
```

 

