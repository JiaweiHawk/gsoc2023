# Add virtio-net Control Virtqueue state restore support

**GSoC contributor**: Jiawei Yin

**Mentors**: Eugenio Perez Martin

## Project Description

Virtio-net device uses Control Virtqueue(CVQ) for changing device parameters. For some devices, such as vDPA, its CVQ is passed from Guest’s driver directly to Host driver, which makes it difficult for QEMU to track the status of these devices.

To solve this problem, QEMU introduces Shadow Virtqueue(SVQ) for vdpa device, which shadows the CVQ via QEMU instead of being assigned directly to Guest. During QEMU Live migration, to restore the vdpa device in the destination VM to the state in the source VM, SVQ can use CVQ’s interface and send the state as regular CVQ commands.

Due to some CVQ commands for vDPA are missing at present, this project aims to achieve support for a subset of CVQ commands, including VIRTIO_NET_CTRL_RX family commands, VIRTIO_NET_CTRL_GUEST_OFFLOADS_SET command, VIRTIO_NET_CTRL_VLAN_ADD command, VIRTIO_NET_CTRL_MQ_HASH_CONFIG command and VIRTIO_NET_CTRL_MQ_RSS_CONFIG command for vDPA.

What's more, I also fixed some bugs and implemented performance improvements as part of this project.

## Patches

### QEMU

#### New Features

| Status | Link |
| :-: | :-: |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [Vhost-vdpa Shadow Virtqueue Offloads support](https://lore.kernel.org/all/cover.1685704856.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [Vhost-vdpa Shadow Virtqueue _F_CTRL_RX commands support](https://lore.kernel.org/all/cover.1688743107.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [Vhost-vdpa Shadow Virtqueue _F_CTRL_RX_EXTRA commands support](https://lore.kernel.org/all/cover.1688797728.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Pending-00A8E1" /> | [Vhost-vdpa Shadow Virtqueue VLAN support](https://lore.kernel.org/all/cover.1690106284.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [Vhost-vdpa Shadow Virtqueue Hash calculation Support](https://lore.kernel.org/all/cover.1693297766.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [Vhost-vdpa Shadow Virtqueue RSS Support](https://lore.kernel.org/all/cover.1693299194.git.yin31149@gmail.com/) |

#### Performance Improvements

| Status | Link |
| :-: | :-: |
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [vdpa: Send all CVQ state load commands in parallel](https://lore.kernel.org/all/cover.1693287885.git.yin31149@gmail.com/) |

#### Bug Fixes

| Status | Link |
| :-: | :-: |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [vhost: fix possible wrap in SVQ descriptor ring](https://lore.kernel.org/all/20230509084817.3973-1-yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [vdpa: Return -EIO if device ack is VIRTIO_NET_ERR](https://lore.kernel.org/all/cover.1688438055.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [vdpa: Fix possible use-after-free for VirtQueueElement](https://lore.kernel.org/all/cover.1688746840.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [vdpa: Refactor vdpa_feature_bits array](https://lore.kernel.org/all/cover.1688130570.git.yin31149@gmail.com/) |

### Kernel

#### Bug Fixes

| Status | Link |
| :-: | :-: |
| <img src="https://img.shields.io/badge/Merged-4EAA25" /> | [virtio-net: Zero max_tx_vq field for VIRTIO_NET_CTRL_MQ_HASH_CONFIG case](https://lore.kernel.org/all/20230810110405.25558-1-yin31149@gmail.com/) |

## Usage

### Apply the patches

1. clone the QEMU from gitlab by `git clone git@gitlab.com:qemu-project/qemu.git`
2. checkout to the commit d669b7bba22d45cb9e5926d63541e52bde1655dd by `git checkout -b d669b7bba22d45cb9e5926d63541e52bde1655dd`
3. download all **pending** patches according to the link above
4. apply patches in the order of dependencies by `git am XX.patch`

### Build the Qemu

1. install libbpf dependency by `sudo apt install libbpf-dev` in Ubuntu
2. configure the Qemu with `bpf` and `vhost-vdpa` features enabled by `/path/to/qemu/configure --enable-bpf --enable-vhost-vdpa`
3. compile the Qemu by `make -j $(nproc)`

### Run the guest

TODO

## Summary

I am incredibly thankful to my mentor, Eugenio, whose unwavering support helped me a lot during this project. Throughout my internship, his guidance has been invaluable, providing clarity to my inquiries and deepening my understanding through insightful discussions on pertinent subjects. Thanks to Eugenio's mentorship, I was able to quickly get up to speed on this project and ultimatly complete it successfully.

Furthermore, I would like to extend my heartfelt appreciation to the diligent QEMU community developers, whose constructive feedback greatly enhanced the quality of my patches. Their generous contributions have significantly contributed to the refinement of my work.

Over the course of this internship, I had the opportunity to learn the knowledge about the vDPA framework and gained profound insights into the process of state changing within QEMU's vhost-vdpa backend. It brings me immense joy to successfully implement the state restore feature for QEMU's vhost-vdpa device. This experience has been exceptionally enriching and has undoubtedly broadened my horizons!
