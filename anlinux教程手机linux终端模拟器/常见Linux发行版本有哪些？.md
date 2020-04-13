04.13 17:08
常见Linux发行版本有哪些？
常见Linux发行版本有哪些？
< 上一页Linux的优缺点这么多Linux发行版，哪个最适合初学者？下一页 >
新手往往会被 Linux 众多的发行版本搞得一头雾水，我们首先来解释一下这个问题。

从技术上来说，李纳斯•托瓦兹开发的 Linux 只是一个内核。内核指的是一个提供设备驱动、文件系统、进程管理、网络通信等功能的系统软件，内核并不是一套完整的操作系统，它只是操作系统的核心。一些组织或厂商将 Linux 内核与各种软件和文档包装起来，并提供系统安装界面和系统配置、设定与管理工具，就构成了 Linux 的发行版本。

在 Linux 内核的发展过程中，各种 Linux 发行版本起了巨大的作用，正是它们推动了 Linux 的应用，从而让更多的人开始关注 Linux。因此，把 Red Hat、Ubuntu、SUSE 等直接说成 Linux 其实是不确切的，它们是 Linux 的发行版本，更确切地说，应该叫作“以Linux为核心的操作系统软件包”。

Linux 的各个发行版本使用的是同一个 Linux 内核，因此在内核层不存在什么兼容性问题，每个版本有不一样的感觉，只是在发行版本的最外层（由发行商整合开发的应用）才有所体现。

Linux 的发行版本可以大体分为两类：
商业公司维护的发行版本，以著名的 Red Hat 为代表；
社区组织维护的发行版本，以 Debian 为代表。

很难说大量 Linux 版本中哪一款更好，每个版本都有自己的特点。下面为大家介绍几款常用的 Linux 发行版本。
1) Red Hat Linux

 
Red Hat（红帽公司）创建于 1993 年，是目前世界上资深的 Linux 厂商，也是最获认可的 Linux 品牌。

Red Hat 公司的产品主要包括 RHEL（Red Hat Enterprise Linux，收费版本）和 CentOS（RHEL 的社区克隆版本，免费版本）、Fedora Core（由 Red Hat 桌面版发展而来，免费版本）。

Red Hat 是在我国国内使用人群最多的 Linux 版本，资料丰富，如果你有什么不明白的地方，则容易找到人来请教，而且大多数 Linux 教程是以 Red Hat 为例来讲解的（包括本教程）。

本教程以我国国内互联网公司常用的 Linux 发行版本 CentOS 为例讲解，它是基于 Red Hat Enterprise Linux 源代码重新编译、去除 Red Hat 商标的产物，各种操作使用和付费版本没有区别，且完全免费。缺点是不向用户提供技术支持，也不负任何商业责任。有实力的公司可以选择付费版本。
2) Ubuntu Linux

 
Ubuntu 基于知名的 Debian Linux 发展而来，界面友好，容易上手，对硬件的支持非常全面，是目前最适合做桌面系统的 Linux 发行版本，而且 Ubuntu 的所有发行版本都免费提供。

Ubuntu 的创始人 Mark Shuttleworth 是非常具有传奇色彩的人物。他在大学毕业后创建了一家安全咨询公司，1999 年以 5.75 亿美元被收购，由此一跃成为南非最年轻有为的本土富翁。作为一名狂热的天文爱好者，Mark Shuttleworth 于 2002 年自费乘坐俄罗斯联盟号飞船，在国际空间站中度过了 8 天的时光。之后，Mark Shuttleworth 创立了 Ubuntu 社区，2005 年 7 月 1 日建立了 Ubuntu 基金会，并为该基金会投资 1000 万美元。他说，太空的所见正是他创立 Ubuntu 的精神之所在。如今，他最热衷的事情就是到处为自由开源的 Ubuntu 进行宣传演讲。
3) SuSE Linux

 
SuSE Linux 以 Slackware Linux 为基础，原来是德国的 SuSE Linux AG 公司发布的 Linux 版本，1994 年发行了第一版，早期只有商业版本，2004 年被 Novell 公司收购后，成立了 OpenSUSE 社区，推出了自己的社区版本 OpenSUSE。

SuSE Linux 在欧洲较为流行，在我国国内也有较多应用。值得一提的是，它吸取了 Red Hat Linux 的很多特质。

SuSE Linux 可以非常方便地实现与 Windows 的交互，硬件检测非常优秀，拥有界面友好的安装过程、图形管理工具，对于终端用户和管理员来说使用非常方便。
4) Gentoo Linux

 
Gentoo 最初由 Daniel Robbins（FreeBSD 的开发者之一）创建，首个稳定版本发布于 2002 年。Gentoo 是所有 Linux 发行版本里安装最复杂的，到目前为止仍采用源码包编译安装操作系统。

不过，它是安装完成后最便于管理的版本，也是在相同硬件环境下运行最快的版本。自从 Gentoo 1.0 面世后，它就像一场风暴，给 Linux 世界带来了巨大的惊喜，同时也吸引了大量的用户和开发者投入 Gentoo Linux 的怀抱。

有人这样评价 Gentoo：快速、设计干净而有弹性，它的出名是因为其高度的自定制性（基于源代码的发行版）。尽管安装时可以选择预先编译好的软件包，但是大部分使用 Gentoo 的用户都选择自己手动编译。这也是为什么 Gentoo 适合比较有 Linux 使用经验的老手使用。
要注意的是，由于编译软件需要消耗大量的时间，所以，如果你所有的软件都由自己编译，并安装 KDE 桌面系统等比较大的软件包，则可能需要花费很长时间。

5) 其他 Linux 发行版
除以上 4 种 Linux 发行版外，还有很多其他版本，表 1 罗列了几种常见的 Linux 发行版以及它们各自的特点：

表 1 Linux 发行版及特点汇总
版本名称	网 址	特 点	软件包管理器
Debian Linux	www.debian.org	开放的开发模式，且易于进行软件包升级	apt
Fedora Core	www.redhat.com	拥有数量庞人的用户，优秀的社区技术支持. 并且有许多创新	up2date（rpm），yum （rpm）
CentOS	www.centos.org	CentOS 是一种对 RHEL（Red Hat Enterprise Linux）源代码再编译的产物，由于 Linux 是开发源代码的操作系统，并不排斥样基于源代码的再分发，CentOS 就是将商业的 Linux 操作系统 RHEL 进行源代码再编译后分发，并在 RHEL 的基础上修正了不少已知的漏洞	rpm
SUSE Linux	www.suse.com	专业的操作系统，易用的 YaST 软件包管理系统	YaST（rpm），第三方 apt （rpm）软件库（repository）
Mandriva	www.mandriva.com	操作界面友好，使用图形配置工具，有庞大的社区进行技术支持，支持 NTFS 分区的大小变更	rpm
KNOPPIX	www.knoppix.com	可以直接在 CD 上运行，具有优秀的硬件检测和适配能力，可作为系统的急救盘使用	apt
Gentoo Linux	www.gentoo.org	高度的可定制性，使用手册完整	portage
Ubuntu	www.ubuntu.com	优秀已用的桌面环境，基于 Debian 构建	apt
Linux 发行版本的选择
Linux 的发行版本众多，在此不逐一介绍，下面给选择 Linux 发行版本犯愁的朋友一点建议：
如果你需要的是一个服务器系统，而且已经厌烦了各种 Linux 的配置，只是想要一个比较稳定的服务器系统，那么建议你选择 CentOS 或 RHEL。
如果你只是需要一个桌面系统，而且既不想使用盗版，又不想花大价钱购买商业软件，不想自己定制，也不想在系统上浪费太多时间，则可以选择 Ubuntu。
如果你想深入摸索一下 Linux 各个方面的知识，而且还想非常灵活地定制自己的 Linux 系统，那就选择 Gentoo 吧，尽情享受 Gentoo 带来的自由快感。
如果你对系统稳定性要求很高，则可以考虑 FreeBSD。
如果你需要使用数据库高级服务和电子邮件网络应用，则可以选择 SuSE。

以上纯属个人化建议，非官方指导意见。其实 Linux 的发行版本众多，但是系统的核心——内核却系出同门，所以只要学会使用其中一种，即可触类旁通。
