# doca_telemetry_diag guide

## prerequiste
1. HW supported
	- CX7
	- BF3 nic-mode
2. doca-2.9.0 or above installed

3. install fwctl kernel modules
	- check if the kernel version on which fwctl was build matches the current kernel version.
		- cd ${MLNX_OFED_package}/
		- uname -r
		- rpm -ql fwctl-24.10-OFED.24.10.0.6.7.1.kver.${uname -r}.rpm

			/lib/modules/5.14.0-427.13.1.el9_4.x86_64/extra/fwctl
			/lib/modules/5.14.0-427.13.1.el9_4.x86_64/extra/fwctl/fwctl.ko
			/lib/modules/5.14.0-427.13.1.el9_4.x86_64/extra/fwctl/mlx5
			/lib/modules/5.14.0-427.13.1.el9_4.x86_64/extra/fwctl/mlx5/mlx5_fwctl.ko
	- if kernel version mismatches, recompile fwctl kernel drivers
		- mlnx_add_kernel_support.sh -m . --make-tgz --skip-repo

	- install fwctl rpm
		- rpm -ihv fwctl-24.10-OFED.24.10.0.6.7.1.kver.${uname -r}.rpm

	- load fwctl kernel modules
		- modprobe -a fwctl mlx5_fwctl

## build doca_telemetry_diag
1. clone repo
	- git clone git@github.com:lichq3553/doca_telemetry_diag.git
	- git reset --hard origin/licq/doca-2.9.0

	- we can also copy the source code from /opt/mellanox/doca/samples/doca_telemetry/telemetry_diag and change meson.build correspondingly.

2. build
	- rm -rf build; meson build --buildtype release; ninja -C build

## run doca_telemetry_diag
1. run
	- ./build/doca_telemetry_diag -p ca:00.0 -o output.cvs -di data_ids.json

2. check the output
	- vim output.cvs

3. add more sampling items
	- refer to the section 'Supported Data IDs' in the following document and select the items needed
		- https://docs.nvidia.com/doca/sdk/doca+telemetry/index.html

	- add the selected items in data_ids.json and rerun doca_telemetry_diag

