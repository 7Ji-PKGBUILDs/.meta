# Package guidelines
We have the following guidelines on packages maintained here:
## Arch official package guidelines
Please follow [Arch package guidelines](https://wiki.archlinux.org/title/Arch_package_guidelines), except the following modifications:

|Section|Arch official|Modified|
|-|-|-|
|[Architecture](https://wiki.archlinux.org/title/Arch_package_guidelines#Architectures)|The `arch` array should contain **`'x86_64'`** if the compiled package is architecture-specific. Otherwise, use `'any'` for architecture independent packages. |The `arch` array should contain **appropriate arches (e.g. `x86_64`, `aarch64`, etc)** if the compiled package is architecture-specific. Otherwise, use `'any'` for architecture independent packages.|
|[Licenses](https://wiki.archlinux.org/title/Arch_package_guidelines#Licenses)|Arch Linux uses [SPDX license identifiers](https://en.wikipedia.org/wiki/Software_Package_Data_Exchange#License_syntax)|We do not enforce this yet, the old format is still allowed|
|[Reproducible builds](https://wiki.archlinux.org/title/Arch_package_guidelines#Reproducible_builds)|Arch is working on making all packages [reproducible](https://wiki.archlinux.org/title/Reproducible_builds).|We do not enforce this yet, non-reproducible PKGBUILDs are still allowed|

## AUR syncing
If a package is allowed on AUR and submitted to there, please keep the AUR repo in sync. Otherwise, please write friendly comments on corresponding AUR pages to notify existing users of the migration of the package.

## -bin package guidelines
Please follow these extra guidelines on `-bin` packages
- The corresponding non-bin package must be either in official repo, or maintained on AUR / in here. An orphan `-bin` package without accompanying non-bin package is not welcomed.
- Do not provide pre-built `-bin` package in any form.
- Always list the non-bin counterpart in `provides` and `conflicts`.
- Never set these packages as dependency of other packages, they should depend on the non-bin counterpart.

## VCS package guidelines
Please follow these extra guidelines on VCS packages (`-git`, `-svn`, etc)
- Do not freeze/track VCS packages to tags, commit, or any non-dynamic references. Only track HEAD, branches, or other dynamic reference. VCS packages are meant to stick to whatever upstream provides. If it breaks, then fix the upstream, not freeze the package. Besides, you're not locking anything, as we only provide PKGBUILDs, and it's end users's priviledge and duty to choose the actual upstream state they want to follow.
- Always list the non-VCS counterpart in `provides` and `conflicts`, except for kernel packages (see below).
- Never set these packages as dependency of other packages, they should depend on the non-VCS counterpart.

## Kernel package guidelines
Please follow these extra guidelines on kernel packages.
- For Arch ports (e.g. Arch Linux ARM), especially those that run on platforms using DTB for bootup, please follow my blog [Non-nonflicting kernel and DTB layout on ARM for multi-boot](https://7ji.github.io/booting/2024/01/29/non-conflicting-kernel-dtb-layout-on-arm-for-multi-boot.html) to store kernel images and DTBs in a non-conflicting way, so multiple kernels could be installed side-by-side and switched around easily.
- Kernel packages should not follow `provides` and `conflicts` rules for `-bin` and VCS packages. They can co-exist file-system-wise anyway if you follow the last rule. This would make it much easier to jump between different kernel packages to debug.
- It's recommended to name kernel packages in the following way:
  ```
  linux(-[architecture])(-[vendor](-[device])(-[bsp ver]))(-[feature])(-[upstream])
  ```
  In which:
  - `architecture` shall be explicitely set if this kernel PKGBUILD supports a specific architecture, e.g. `linux-aarch64`, `linux-armv7h`, etc. This also serves as a namespace and avoids conflicting names.
  - `vendor` shall be the name of a vendor (e.g. `amlogic`, `rockchip`) if this kernel is tailored for a specific vendor's platform
    - if this kernel is only for a specific device, add that
    - if this kernel is a BSP kernel, please also add `-[bsp ver]` to mark the bsp version, e.g. `linux-rockchip-bsp6.1`. 
  - `feature` shall be set if a noticable feature is only available in such kernel package, e.g. `linux-panthor`.
  - `upstream` shall be set if the source does not come from mainline or vendor directly, but a third party maintainer (including manufacturer), e.g. `linux-aarch64-7ji` builds from ther kernel source maintained by 7Ji.

  An example name:
  ```
  linux-aarch64-rockchip-bsp6.1-panthor-joshua
  ```
  - `-aarch64` this is for `aarch64` architecture
  - `-rockchip` this is for platforms made by vendor `rockchip`
  - `-bsp6.1` this builds from bsp6.1 source
  - `-panthor` this is an experimental kernel that has backported `panthor`, this is set as there're multiple kernel trees maintained by `joshua`, and it would cause conflicts
  - `-joshua` this is not from bsp6.1 directly, but from Joshua-Riek's kernel repo

  Another example:
  ```
  linux-aarch64-amlogic-ogu-bsp4.14-odroid
  ```
  - `-amlogic` vendor
  - `-ogu` Odroid Go Ultra specific
  - `-bsp4.14` the BSP version
  - `-odroid` kernel source maintained by Odroid



