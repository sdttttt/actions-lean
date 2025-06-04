# 2025.4.2 啊哈又崩了，永生永世再也不用Lean

LEAN'S LEDE固件主线编译，6.12内核(开启RealTime抢占模式)，自带OpenClash, O3优化。**可吃鹅**


> 最近不吃鹅了，换了更强劲的设备，没有性能焦虑了又用回小猫咪了。

## 和Immortalwrt区别感受

- 性能上Lean好像确实更快一点，speedtest测速的时候发现Lean比immortalwrt更快拉满.
- 编译速度Lean会更快，可能是我配置文件的原因，速度要快1倍左右.
- Lean没有打版本号，感觉不太稳定，使用主线固件基本就是随缘.
- Lean的软件仓库比Immortalwrt少很多.
- Lean目前总是会有偶发性断网的情况，不知道是我编译问题还是什么，暂时没怎么用了

## 一些修改

主要是为了吃鹅改造的，
常规的开启一些关于eBPF的内核选项。

```
# eBPF
CONFIG_DEVEL=y
CONFIG_BPF_TOOLCHAIN_HOST=y
# CONFIG_BPF_TOOLCHAIN_NONE is not set
CONFIG_KERNEL_BPF_EVENTS=y
CONFIG_KERNEL_CGROUP_BPF=y
CONFIG_KERNEL_DEBUG_INFO=y
CONFIG_KERNEL_DEBUG_INFO_BTF=y
# CONFIG_KERNEL_DEBUG_INFO_REDUCED is not set
CONFIG_KERNEL_XDP_SOCKETS=y
```

添加`xdp-sockets-diag`模块。

```bash
# 添加 xdp-sockets-diag 内核模块
echo '

define KernelPackage/xdp-sockets-diag
  SUBMENU:=$(NETWORK_SUPPORT_MENU)
  TITLE:=PF_XDP sockets monitoring interface support for ss utility
  KCONFIG:= \
	CONFIG_XDP_SOCKETS=y \
	CONFIG_XDP_SOCKETS_DIAG
  FILES:=$(LINUX_DIR)/net/xdp/xsk_diag.ko
  AUTOLOAD:=$(call AutoLoad,31,xsk_diag)
endef

define KernelPackage/xdp-sockets-diag/description
 Support for PF_XDP sockets monitoring interface used by the ss tool
endef

$(eval $(call KernelPackage,xdp-sockets-diag))
' >> package/kernel/linux/modules/netsupport.mk
```

## Credits

- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub Actions](https://github.com/features/actions)
- [OpenWrt](https://github.com/openwrt/openwrt)
- [Lean's OpenWrt](https://github.com/coolsnowwolf/lede)
- [tmate](https://github.com/tmate-io/tmate)
- [mxschmitt/action-tmate](https://github.com/mxschmitt/action-tmate)
- [csexton/debugger-action](https://github.com/csexton/debugger-action)
- [Cowtransfer](https://cowtransfer.com)
- [WeTransfer](https://wetransfer.com/)
- [Mikubill/transfer](https://github.com/Mikubill/transfer)
- [softprops/action-gh-release](https://github.com/softprops/action-gh-release)
- [ActionsRML/delete-workflow-runs](https://github.com/ActionsRML/delete-workflow-runs)
- [dev-drprasad/delete-older-releases](https://github.com/dev-drprasad/delete-older-releases)
- [peter-evans/repository-dispatch](https://github.com/peter-evans/repository-dispatch)

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © [**P3TERX**](https://p3terx.com)
