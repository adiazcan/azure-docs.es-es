# Información general
## [Primeros pasos](get-started.md)
## [Certificaciones](sap-certifications.md)
# SAP HANA en Azure (instancias grandes)
## Información general
### [¿Qué es SAP HANA en Azure (instancias grandes)?](hana-overview-architecture.md)
### [Conocer los términos](hana-know-terms.md)
### [Certificación](hana-certification.md)
### [SKU disponibles para HLI](hana-available-skus.md)
### [Ajuste de tamaño](hana-sizing.md)
### [Requisitos de incorporación](hana-onboarding-requirements.md)
### [Nodos de extensión y capas de datos de SAP HANA](hana-data-tiering-extension-nodes.md)
### [Modelo de operaciones y responsabilidades](hana-operations-model.md)
## Arquitectura
### [Arquitectura general](hana-architecture.md)
### [Arquitectura de red](hana-network-architecture.md)
### [Arquitectura de almacenamiento](hana-storage-architecture.md)
### [Escenarios admitidos de HLI](hana-supported-scenario.md)
## Infraestructura y conectividad
### [Implementación de HLI](hana-overview-infrastructure-connectivity.md)
### [Conexión de las máquinas virtuales de Azure a HANA (instancias grandes)](hana-connect-azure-vm-large-instances.md)
### [Conexión de una red virtual a ExpressRoute de HANA (instancias grandes)](hana-connect-vnet-express-route.md)
### [Requisitos de red adicionales](hana-additional-network-requirements.md)
## Instalación de SAP HANA
### [Validación de la configuración](hana-installation.md)
### [Instalación de ejemplo de HANA](hana-example-installation.md)
## Alta disponibilidad y recuperación ante desastres
### [Opciones y consideraciones](hana-overview-high-availability-disaster-recovery.md)
### [Copia de seguridad y restauración](hana-backup-restore.md)
### [Principios y preparación](hana-concept-preparation.md)
### [Procedimiento de conmutación por error de recuperación ante desastres](hana-failover-procedure.md)
## Solución de problemas y supervisión
### [Supervisión de HLI](troubleshooting-monitoring.md)
### [Supervisión y solución de problemas en el lado de HANA](hana-monitor-troubleshoot.md)
## Procedimientos
### [Configurar alta disponibilidad con STONITH](ha-setup-with-stonith.md)
### [Copia de seguridad del sistema operativo para SKU de tipo II](os-backup-type-ii-skus.md)
### [Actualización del sistema operativo para instancias grandes de HANA](os-upgrade-hana-large-instance.md)
### [Configuración de servidor SMT para SUSE Linux](hana-setup-smt.md)
# SAP HANA en Azure Virtual Machines
## [Instalación de instancia individual de SAP HANA](hana-get-started.md)
## [Guía de implementación de S/4 HANA o BW/4 HANA SAP CAL](cal-s4h.md)
## [Configuraciones y operaciones de infraestructura de SAP HANA en Azure](hana-vm-operations.md)
## Disponibilidad de SAP HANA en Azure Virtual Machines
### [Introducción a la disponibilidad de SAP HANA en Azure](sap-hana-availability-overview.md)
### [Disponibilidad de SAP HANA en Azure dentro de una región de Azure](sap-hana-availability-one-region.md)
### [Disponibilidad de SAP HANA en Azure entre regiones de Azure](sap-hana-availability-across-regions.md)
### [Configuración de la replicación del sistema de SAP HANA en SUSE Linux Enterprise Server](sap-hana-high-availability.md)
### [Configuración de la replicación del sistema de SAP HANA en RHEL](sap-hana-high-availability-rhel.md)
### [Solución de problemas de escalabilidad horizontal de SAP HANA y Pacemaker en SLES](hana-vm-troubleshoot-scale-out-ha-on-sles.md)
## [SAP HANA: información general sobre copias de seguridad](sap-hana-backup-guide.md)
## [SAP HANA: copia de seguridad en el nivel de archivo](sap-hana-backup-file-level.md)
## [SAP HANA: copias de seguridad de instantáneas de almacenamiento](sap-hana-backup-storage-snapshots.md)
# SAP NetWeaver y Business One en Azure Virtual Machines
## [Lista de comprobación de planeación e implementación de carga de trabajo de SAP](sap-deployment-checklist.md)
## [Planeación e implementación de SAP NetWeaver en Azure](planning-guide.md)
## [Guía de implementación de SAP NetWeaver](deployment-guide.md)
## Guías de implementación de DBMS para la carga de trabajo de SAP
### [Implementación general de DBMS de Azure Virtual Machines para una carga de trabajo de SAP](dbms_guide_general.md)
### [Implementación de DBMS de Azure Virtual Machines de SQL Server para una carga de trabajo de SAP](dbms_guide_sqlserver.md)
### [Implementación de DBMS de Azure Virtual Machines de Oracle para una carga de trabajo de SAP](dbms_guide_oracle.md)
### [Implementación de DBMS de Azure Virtual Machines de IBM DB2 para una carga de trabajo de SAP](dbms_guide_ibm.md)
### [Implementación de DBMS de Azure Virtual Machines de SAP ASE para una carga de trabajo de SAP](dbms_guide_sapase.md)
### [Implementación de SAP MaxDB, liveCache y del servidor de contenido en Azure](dbms_guide_maxdb.md)
### Disponibilidad de SAP HANA en Azure Virtual Machines
### [Introducción a la disponibilidad de SAP HANA en Azure](sap-hana-availability-overview.md)
### [Disponibilidad de SAP HANA en Azure dentro de una región de Azure](sap-hana-availability-one-region.md)
### [Disponibilidad de SAP HANA en Azure entre regiones de Azure](sap-hana-availability-across-regions.md)
## [SAP Business One en Azure Virtual Machines](business-one-azure.md)
## [SAP IDES en la guía de implementación de Windows o SQL Server SAP CAL](cal-ides-erp6-erp7-sp3-sql.md)
## [SAP NetWeaver en máquinas virtuales Linux de Azure](suse-quickstart.md)
## [Conector de SAP LaMa para Azure](lama-installation.md)
## Alta disponibilidad en Windows y Linux
### [Información general](sap-high-availability-guide-start.md)
### Arquitectura de alta disponibilidad
#### [Arquitectura y escenarios de alta disponibilidad](sap-high-availability-architecture-scenarios.md)
#### [Arquitectura y escenarios de mayor disponibilidad](sap-higher-availability-architecture-scenarios.md)
#### [Alta disponibilidad en Windows con disco compartido para instancias de (A)SCS](sap-high-availability-guide-wsfc-shared-disk.md)
#### [Alta disponibilidad en Windows con uso compartido de archivos SOFS para instancias de (A)SCS](sap-high-availability-guide-wsfc-file-share.md)
#### [Alta disponibilidad en SUSE Linux para instancias de (A)SCS](high-availability-guide-suse.md)
#### [Alta disponibilidad en Red Hat Enterprise Linux para una instancia de (A)SCS](high-availability-guide-rhel.md)
### Preparación de la infraestructura de Azure
#### [Windows con disco compartido para instancias de (A)SCS](sap-high-availability-infrastructure-wsfc-shared-disk.md)
#### [Windows con uso compartido de archivos SOFS para instancias de (A)SCS](sap-high-availability-infrastructure-wsfc-file-share.md)
#### [Alta disponibilidad para NFS en máquinas virtuales de Azure en SLES](high-availability-guide-suse-nfs.md)
#### [GlusterFS en Azure Virtual Machines en Red Hat Enterprise Linux para SAP NetWeaver](high-availability-guide-rhel-glusterfs.md)
#### [Pacemaker en SLES](high-availability-guide-suse-pacemaker.md)
#### [Pacemaker en RHEL](high-availability-guide-rhel-pacemaker.md)
### Instalación de SAP
#### [Windows con disco compartido para instancias de (A)SCS](sap-high-availability-installation-wsfc-shared-disk.md)
#### [Windows con uso compartido de archivos SOFS para instancias de (A)SCS](sap-high-availability-installation-wsfc-file-share.md)
#### [SUSE Linux con NFS para instancias de (A)SCS](high-availability-guide-suse.md)
#### [Alta disponibilidad para SAP NetWeaver en Red Hat Enterprise Linux](high-availability-guide-rhel.md)
### Varios SID de SAP
#### [Windows con disco compartido para instancias de (A)SCS](sap-ascs-ha-multi-sid-wsfc-shared-disk.md)
#### [Windows con uso compartido de archivos SOFS para instancias de (A)SCS](sap-ascs-ha-multi-sid-wsfc-file-share.md)
##  [Azure Site Recovery para recuperación ante desastres SAP](../../../site-recovery/site-recovery-workload.md#protect-sap)
# Integración de identidades de AAD SAP e inicio de sesión único
## [Integración con la nube de SAP](../../../active-directory/saas-apps/sap-customer-cloud-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Integración de AAD con SAP Cloud Platform Identity Authentication](../../../active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Configuración del inicio de sesión único con SAP Cloud Platform](../../../active-directory/saas-apps/sap-hana-cloud-platform-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Integración de AAD con SAP NetWeaver](../../../active-directory/saas-apps/sap-netweaver-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Integración de AAD con SAP Business ByDesign](../../../active-directory/saas-apps/sapbusinessbydesign-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Integración de AAD con SAP HANA DBMS](../../../active-directory/saas-apps/saphana-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
##[Inicio de sesión único basado en SAML para SAP Fiori Launchpad con Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad)
# Integración de servicios de Azure en SAP
## [Uso de SAP HANA en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
## [DirectQuery y SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
## [Uso del conector SAP BW en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector)
## [Azure Data Factory ofrece integración de datos de SAP HANA y Business Warehouse](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)
# Recursos
## [Azure Roadmap](https://azure.microsoft.com/roadmap/)

