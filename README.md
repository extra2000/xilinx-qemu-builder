# xilinx-qemu-builder

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/56j168i7go8ex3o4/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/xilinx-qemu-builder/branch/master) |

Building Xilinx QEMU using Podman.


## Building image

Build image:
```
podman build -t extra2000/xilinx-qemu-builder .
```


## Building Xilinx QEMU

Clone Xilinx QEMU repository:
```
git clone --recursive --branch xilinx_v2021.2 https://github.com/Xilinx/qemu.git src/qemu
```

Label source codes:
```
chcon -R -v -t container_file_t ./src/qemu
```

Launch Xilinx QEMU Builder container:
```
podman run -it --rm -v ./src/qemu:/opt/qemu --workdir /opt/qemu extra2000/xilinx-qemu-builder bash
```

Configure and build (change `-j16` according to your CPU cores):
```
mkdir build
cd build
../configure --target-list="aarch64-softmmu,microblazeel-softmmu" --enable-fdt --disable-kvm --disable-xen --enable-gcrypt
make -j16
```

After finished building, the binaries are located at `./src/qemu/build/aarch64-softmmu/qemu-system-aarch64` for AARCH64 and `src/qemu/build/microblazeel-softmmu/qemu-system-microblazeel` for Microblaze.


## Building Xilinx QEMU Device Trees

Clone device trees repository:
```
git clone git://github.com/Xilinx/qemu-devicetrees.git src/qemu-devicetrees
```

Label source code:
```
chcon -R -v -t container_file_t ./src/qemu-devicetrees
```

Launch Xilinx QEMU Builder container:
```
podman run -it --rm -v ./src/qemu-devicetrees:/opt/qemu-devicetrees --workdir /opt/qemu-devicetrees extra2000/xilinx-qemu-builder bash
```

After finished building, the output files can be found in `src/qemu-devicetrees/LATEST/*`.
