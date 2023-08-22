# Add virtio-net Control Virtqueue state restore support

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
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [Vhost-vdpa Shadow Virtqueue Hash calculation Support](https://lore.kernel.org/all/cover.1691762906.git.yin31149@gmail.com/) |
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [Vhost-vdpa Shadow Virtqueue RSS Support](https://lore.kernel.org/all/cover.1691766252.git.yin31149@gmail.com/) |

#### Performance Improvements

| Status | Link |
| :-: | :-: |
| <img src="https://img.shields.io/badge/Pending-00A8E1" />  | [vdpa: Send all CVQ state load commands in parallel](https://lore.kernel.org/all/cover.1689748694.git.yin31149@gmail.com/) |

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
