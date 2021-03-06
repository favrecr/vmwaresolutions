---

copyright:

  years:  2016, 2020

lastupdated: "2020-04-02"

keywords: HyTrust KeyControl, HTKC, tech specs HTKC

subcollection: vmware-solutions


---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# HyTrust KeyControl overview
{: #htkc_considerations}

The HyTrust KeyControl service simplifies the management of encrypted workloads. This service automates and simplifies the lifecycle of encryption keys, including key storage, key distribution, key rotation, and key revocation. Using FIPS 140-2 compliant encryption, enterprises can easily manage encryption keys at scale.
{: shortdesc}

This service is available only to instances that are running VMware vSphere 6.5. The current HyTrust KeyControl version that is installed is 5.0.1.
{:note}

## Technical specifications for HyTrust KeyControl
{: #htkc_considerations-specs}

The following components are ordered and included in the HyTrust KeyControl service:

### HyTrust KeyControl appliance
{: #htkc_considerations-appliance}

* CPU: 2 vCPU
* RAM: 8 GB
* Disk: 20 GB VMDK resident on vSAN in converged cluster
* Network: Placed on VLAN-backed private portable network specified for management

### High availability
{: #htkc_considerations-ha}

By default, two KeyControl appliances are deployed in an active-active clustered configuration.

Optionally, you can specify to deploy two KeyControl appliances in a stand-alone unclustered configuration.

### Licenses and fees
{: #htkc_considerations-licenses}

A HyTrust KeyControl license is ordered for each instance installation.

## Considerations when you install HyTrust KeyControl
{: #htkc_considerations-install}

* Before the service is installed in your environment, a check is performed against the available capacity of the default cluster in the environment to ensure that the service components can fit.
* If the capacity check fails, the service is not installed and the service state is set to **Capacity Validation Failed** on the console. In addition, a console message with more details is displayed and you are notified by email.
* To install the service, you must increase the capacity in your default cluster by either adding more hosts or by freeing up RAM, CPU, or disk space, and then add the service again in the console. After that, you can remove the existing service in the **Capacity Validation Failed** state by clicking the delete icon next to it.

## Considerations when you remove HyTrust KeyControl
{: #htkc_considerations-remove}

Before you remove the HyTrust KeyControl service, ensure that you decouple all clients from using KeyContol. After you remove the service, the keys might be deleted and you might be locked out of your VMs.

## Related links
{: #htkc_considerations-related}

* [Ordering HyTrust KeyControl](/docs/vmwaresolutions?topic=vmware-solutions-htkc_ordering)
* [Managing HyTrust KeyControl](/docs/vmwaresolutions?topic=vmware-solutions-managinghtkc)
* [Contacting {{site.data.keyword.IBM}} Support](/docs/vmwaresolutions?topic=vmware-solutions-trbl_support)
* [FAQ](/docs/vmwaresolutions?topic=vmware-solutions-faq-vmwaresolutions)
* [HyTrust website](https://www.hytrust.com/){:external}
