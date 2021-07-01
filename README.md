# Ejemplo de uso de instalador IPI para openshift 4.5.
### Sobre install-config.yaml
### Requisitos mínimos
[Documentación Oficial](https://docs.openshift.com/container-platform/4.6/installing/installing_vsphere/installing-vsphere-installer-provisioned-customizations.html)

- Dhcp activo para instancias que entregue no mas de 2 servidores dns.

- Binarios de instalación ( oc - openshift-install ) ambos descargables desde [Cloud Red Hat](https://cloud.redhat.com) -> Cluster Manager -> Create Cluster -> VMWare Vsphere.

- Pull Secret para usar en install-config.yaml, se obtiene de la misma url que el punto previo.

- Una llave ssh para usar en install-config.yaml, la misma puede ser generada con el comando ssh-keygen

- Una dirección IP para la api del cluster que debera resolver "api.<cluster_name>.<base_domain>.", siendo la misma asignada en install-config.yaml.

- Una dirección IP para en ingress del cluster que deberá resolver "*.apps.<cluster_name>.<base_domain>.", siendo la misma asignada en install-config.yaml.

### Pasos.

- Instalacion de certificados de servidor vcenter en instancia de Bastion.
    ```
    wget https://example.vcenter.url/certs/download.zip --no-check-certificate
    unzip download.zip
    cp certs/lin/* /etc/pki/ca-trust/source/anchors
    update-ca-trust extract
    ```

- Completar datos en el archivo install-config.yaml como se demuestra en el ejemplo ( install-config_example.yaml ).

- Crear archivos de instalación del cluster.
    ```
    openshift-install create install-config --dir=<directorio_instalación>
    ```

- Crear backup del archivo install-config.yaml el mismo es consumido por el instalador.

- Instalación del cluster.
    ```
    openshift-install create cluster --dir=<directorio de instalación> --log-level=info
    ```

#### El proceso de instalación también puede ser observado en otra sesión de consola realizando los siguientes pasos

```
    export KUBECONFIG=/<directorio_instalación>/auth/kubeconfig
    oc get clusteroperators
```

#### El proceso toma aproximadamente 2 horas en completarse.
