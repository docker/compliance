# Docker Enterprise Edition Compliance Controls [![CircleCI](https://circleci.com/gh/docker/compliance/tree/master.svg?style=svg&circle-token=daeaf5acd7ac08000ea727cbf8ec9baa8ded8da4)](https://circleci.com/gh/docker/compliance/tree/master) [![codecov](https://codecov.io/gh/docker/compliance/branch/master/graph/badge.svg?token=WiRPQcno3c)](https://codecov.io/gh/docker/compliance)

This repository contains compliance information and complementary tooling for [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition) as it pertains to [NIST 800-53](https://nvd.nist.gov/800-53) Rev. 4 security controls at the [FedRAMP](https://www.fedramp.gov/) Moderate and High baselines. This data adheres to the [OpenControl](http://open-control.org/) schema for building compliance documentation and can be used to support your own authority to operate (ATO) review process. The system security plan (SSP) documentation that can be generated from this content can be used to assist your organization in authorizing Docker Enterprise Edition on both on-premises/private cloud infrastructures and in public cloud providers.

> **DISCLAIMER:** This content is provided for informational purposes only and has not been vetted by any third-party security assessors. You are solely responsible for developing, implementing, and managing your applications and/or subscriptions running on your own platform in compliance with applicable laws, regulations, and contractual obligations. The documentation is provided "as-is" and without any warranty of any kind, whether express, implied or statutory, and Docker, Inc. expressly disclaims all warranties for non-infringement, merchantability or fitness for a particular purpose.

## Overview

In order to satisfy the entirety of applicable security controls included in this repository, you must have installed all of the components of Docker Enterprise Edition Advanced. This includes Docker EE Engine, Docker Trusted Registry and Universal Control Plane. Each component is associated with a single `component.yaml` text file which contains the pre-written security narratives. These components, and the versions at which the security narratives are currently based, are listed in the table below:

|Component Name|Folder|Version|
|--------------|------|-------|
|Docker EE Engine|[`opencontrol/components/Engine-EE/`](opencontrol/components/Engine-EE)|17.06-ee|
|Docker Trusted Registry (DTR)|[`opencontrol/components/DTR/`](opencontrol/components/DTR)|2.3|
|Docker Security Scanning (DSS)|[`opencontrol/components/DSS/`](opencontrol/components/DSS)|2.3|
|Universal Control Plane (UCP)|[`opencontrol/components/UCP/`](opencontrol/components/UCP)|2.2|
|Authentication and Authorization Service (eNZi)|[`opencontrol/components/eNZi/`](opencontrol/components/eNZi)|2.2|

> **NOTE:** Both the UCP and DTR components leverage the eNZi service component for authentication and authorization across an entire Docker Enterprise Edition Advanced cluster.

You can download this security content for previously released versions of Docker EE, UCP and DTR on our [Releases](https://github.com/docker/compliance/releases) page.

### Roles and responsibilities matrix

One or more control originations have been denoted for each component security control. These roles have been defined as follows:

|Control Origination|Definition|Example|
|-------------------|----------|-------|
|Service provider corporate|A control that originates from agency's corporate network|DNS from the corporate network provides address resolution services for the information system and the service offering|
|Docker EE system|A control specific to Docker EE|Docker EE LDAP configuration|
|Service provider hybrid|A control that makes use of both corporate controls and additional controls specific to Docker EE|There are scans of the corporate network infrastructure; scans of Docker images via DTR would be included|
|Configured by customer|A control where the Docker EE end-user's application needs to apply a configuration in order to meet the control requirement|User profiles, policy/audit configurations, enable/disabling key switches (e.g., enable/disable http or https, etc), entering an IP range specific to the end-user's organization are configurable by the customer|
|Provided by customer|A control where the Docker EE end-user's application needs to provide additional hardware or software in order to meet the control requirement|The customer provides a SAML SSO solution to implement two-factor authentication|
|Shared|A control that is managed and implemented partially by the Docker EE system and partially by the Docker EE end-user|Security awareness training must be conducted by both the Docker EE operators and end-users|
|Inherited from pre-existing Provisional Authorization|A control that is inherited from another CSP system that has already received a Provisional Authorization|Docker EE inherites PE controls from an IaaS provider|

## Generating SSP docs for Docker EE

The [Compliance Masonry](https://github.com/opencontrol/compliance-masonry/) command-line tool is required to generate SSP documentation based on the pre-written Docker EE narratives in this repository. You can either download and run the Compliance Masonry tool directly from your local workstation or run it with Docker.

The [`examples/opencontrol/DockerEE-Moderate-ATO`](examples/opencontrol/DockerEE-Moderate-ATO) folder contains an example that you can use as a starting point for generating an SSP at the FedRAMP Moderate baseline. It also includes additional placeholder `component.yaml` files that can be used to document your organization's adherence to the appropriate controls and that which aren't satisfied by the functionality of Docker Enterprise Edition. These have been organized in to separate directories representing each control family (e.g. `AC_Policy/`, `MA_POLICY/`, etc).

### Download and run Compliance Masonry

You can download the [Compliance Masonry](https://github.com/opencontrol/compliance-masonry/) command-line tool for your specific OS from the releases page [here](https://github.com/opencontrol/compliance-masonry/releases).

After you've cloned or downloaded the contents of this repository to your machine, you can generate your SSP docs based on the [DockerEE-Moderate-ATO](examples/opencontrol/DockerEE-Moderate-ATO) example as follows:

1. Navigate to directory containing the example

    ```sh
    cd examples/opencontrol/DockerEE-Moderate-ATO
    ```

2. Get Compliance Masonry dependencies

    ```sh
    compliance-masonry get
    ```

    or with Docker:

    ```sh
    docker run --rm -v "$PWD":/opencontrol -w /opencontrol opencontrolorg/compliance-masonry get
    ```

3. Generate SSP as a GitBook at the FedRAMP Moderate baseline

    ```sh
    compliance-masonry docs gitbook FedRAMP-moderate
    ```

    or with Docker:

    ```sh
    docker run --rm -v "$PWD":/opencontrol -w /opencontrol opencontrolorg/compliance-masonry docs gitbook FedRAMP-moderate
    ```

If you prefer to generate a Word doc based on the official FedRAMP SSP template, you can follow the instructions at https://github.com/opencontrol/fedramp-templater.

## Precompiled SSP templates for Docker EE

Docker also provides precompiled System Security Plan (SSP) templates for authorizing Docker Enterprise Edition on various FedRAMP P-ATO'd IaaS providers, as indicated in the table below. These can be obtained by contacting [compliance@docker.com](mailto:compliance@docker.com). These templates are **not** the official cloud providers' SSP templates but rather highlight both the controls inherited from that IaaS provider's P-ATO and the controls applicable to Docker Enterprise Edition Advanced. When conducting an ATO, it is still your responsibility to request the provider's official SSP package as appropriate and conduct your own security analysis and audit.

|Provider|Format|Baselines|Status|Last Updated|
|--------|------|---------|------|------------|
|[Microsoft Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)|[Azure Blueprint](https://docs.microsoft.com/en-us/azure/azure-government/documentation-government-plan-compliance) (.docx)|Moderate<br>High<br>DoD L4<br>DoD L5|Available<br>Coming Soon<br>Coming Soon<br>Coming Soon|December 2016|

Note that even if a precompiled template for Docker EE is not available for your chosen cloud provider, you can still use the OpenControl-formatted content in this repository to generate your own SSP templates. Much of the content in this repository is identical to that which is provided in the pre-built templates. This repository also contains the most up-to-date information on Docker EE and that which may not be reflected in the last update to the pre-built SSP templates.

## InSpec profiles for Docker EE

The [`validation/inspec/`](validation/inspec/) directory contains [InSpec](https://inspec.io) audit profiles for Docker EE. These can be used to continuously audit a running Docker EE cluster and validate its configuration against applicable controls at both the FedRAMP Moderate and High baselines.

Instructions for using these profiles can be found in the [`validation/inspec/`](validation/inspec) directory.

## Contributing to Docker compliance resources

Refer to the [Contributing Guide](CONTRIBUTING.md) for instructions on contributing to this project.

### Component validation

The OpenControl schema is defined by the [Kwalify](http://www.kuwata-lab.com/kwalify/) schema validator and YAML parser. Each component definition in the Docker Enterprise Edition Advanced tier is tested against this schema using the [PyKwalify](https://github.com/Grokzen/pykwalify) Python port of the Kwalify specification. The Dockerfile in the root of this repository is used only by CircleCI for running the component tests within a container.

### Natural language processing [experimental]

Thorough documentation of the relevant security controls for each of the Docker EE components is a critical aspect of this project. It's imperative that not only is each control satisfied, but that the contents of the actual narratives match that which is defined by NIST 800-53. As such, we've started to experiment with natural language processing tooling. We've included a set of utilities in the project backed by [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services) that demonstrate ways that the security assessment process can be enhanced with artificial intelligence.

The [`nlp/`](nlp) directory contains a few utilities that we've developed. Contributions welcome!

The potential use cases for natural language processing in documentation efforts are pretty wide-ranging. As such, our goal with this example is to open the door to new and exciting ways to build security and compliance documentation.
