# Gramine Curated MariadDB
In the following two sections, we explain how a Docker image for the MariaDB version with
protected data directory can be built and how the image can be executed.
[Prerequisites](https://github.com/gramineproject/contrib/tree/master/Curated-Apps/README.md) for
both the phases are assumed to be met.

## Build a confidential compute image for MariadDB
Execute the below commands on the VM.

1. Clone the Gramine Contrib repository:

       $ git clone --depth 1 https://github.com/gramineproject/contrib.git

2. Move to the Curated-Apps folder:

       $ cd contrib/Curated-Apps

3. User is expected to first have a base image `<base_image_with_mariadb>` ready with MariadDB and
   the necessary application files built into this image. The directory
  `workloads/mariadb/base_image_helper` contains sample dockerfiles and instructions to create a
   test MariadDB base image. This base image is then passed to the curation application `curate.py`
   as shown below.

4. To generate a preconfigured non-production test confidential compute image for MariaDB,  follow
   the below steps:
   1. Generate a sample MariadDB application image `mariadb-base`:

          $ /bin/bash workloads/mariadb/base_image_helper/helper.sh

   2. Generate the test confidential compute image based on the `mariadb-base` image  as shown
      below:

          $ python3 ./curate.py mariadb mariadb-base test

5. Or, to generate a custom confidential compute image based on a user-provided MariadDB image,
   execute the following to launch an interactive setup script:

       $ python3 ./curate.py mariadb <base_image_with_mariadb>

## Run the confidential compute image for MariadDB

- This example was tested on a Standard_DC8s_v3 Azure VM.
- Follow the output of the `curate.py` script and mount encrypted data directory volume
  /var/lib/mysql by adding -v /var/lib/mysql:/var/lib/mysql to run the generated Docker image(s).

## Contents
This sub-directory contains artifacts which help in creating curated GSC MariadDB image, as
explained below:

    .
    |-- mariadb-gsc.dockerfile.template   # Template used by `curation_script.sh` to create a
    |                                       wrapper dockerfile `mariadb-gsc.dockerfile` that
    |                                       includes user-provided inputs such as `ca.cert`
    |                                       file and command-line arguments into the graminized
    |                                       MariadDB image.
    |-- mariadb.manifest.template         # Template used by `curation_script.sh` to create a
    |                                       user manifest file (with basic set of values defined
    |                                       for graminizing MariadDB images), that will be passed
    |                                       to GSC.
    |-- base_image_helper/                # `base_image_helper` directory contains artifacts which
    |                                       helps in generating a base image.
