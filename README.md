Qualcomm Repo Manifest README
=============================

This repo is used to download manifests for Advantech Qualcomm Modular BSP releases fixing versions with the `repo` utility.

Pre-requisites
--------------------------

To use this manifest repo, the `repo` tool must be installed first.

```
$ mkdir ~/bin
$ curl https://commondatastorage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$ chmod a+x ~/bin/repo
$ PATH=${PATH}:~/bin
```

For additional pre-requisites, e.g. `kas` installation, supported boards, and extra information check [README in Advantech BSP for Qualcomm based boards Yocto layer](https://github.com/Advantech-EECC/meta-modular-bsp-qcom).

Example
-------

To download the qli-2.0 release (for qli-2.1 use qcom-qli-2.1-adv.xml):

```
repo init -u https://github.com/Advantech-EECC/qcom-manifest -b qcom-linux-wrynose-adv -m qcom-qli-2.0-adv.xml
repo sync -c -j8
```

Setup and build example
-----------------------

```
QC_R=meta-qcom-releases
QC_M=meta-modular-bsp-qcom
QC_CI=meta-qcom/ci
IMAGE=qcom-multimedia-proprietary-image
KAS_LIST=$QC_CI/base.yml:$QC_CI/qcom-distro.yml:$QC_CI/lock.yml:$QC_CI/$QC_M.yml

kas checkout $QC_R/lock.yml

cp "$QC_R/lock.yml" "$QC_M/$QC_M.yml" "$QC_CI/"

KAS_MACHINE=aom5721-qcs6490-db6700 kas shell "$KAS_LIST" -c "bitbake $IMAGE"
```

Flash resulting image
---------------------

```
# Set boot mode in SW2 (AOM-DB6700) to: 100100 (recovery) + power cycle
cd build/tmp/deploy/images/aom5721-qcs6490-db6700/qcom-multimedia-proprietary-image-aom5721-qcs6490-db6700.rootfs.qcomflash
qdl --storage ufs prog_firehose_ddr.elf rawprogram[0-9].xml patch[0-9].xml
# Set boot mode in SW2 (AOM-DB6700) to: 100000 (UFS boot) + power cycle
```
