---
layout: post
title: Which GNU/Linux distro? [unix]
---

## Which GNU/Linux distro should you use?

#### Introduction
The never ending debate regarding which GNU/Linux distro to use. There is no right answer to this question,
one simply has to use the distro that suits ones needs. One can argue that it doesn't really matter, and
that any GNU/Linux distro is better than using Windows or Mac. Should you switch to another GNU/Linux distro?
Probably not.

However, there are a couple of things to consider when choosing a distro for long term use, especially if you
are switching to GNU/Linux from another OS.

* Distros are sticky. When you start using a distro you tend to stick to it.
* Stability.
* Privacy and ethics. Is there any proprietary software in the distro and any privacy/security concerns?
* To what degree you want to customize the OS. Do you e.g. want to switch kernel, minimize bloat, etc?

#### Stickiness
There are hundreds of GNU/Linux distributions and they are sorted into categories depending on what core
distribution they are based on or what package management system they use. Which package manager the distro
is using is a huge factor that distinguishes how you will use your system on a day to day basis.

E.g. Kali Linux, PureOS,
Ubuntu, and so on, are based on Debian. These systems use the Debian package management tool dpkg to handle packages
locally on a low level. Furthermore, Debian based systems use the APT (Advanced Packaging Tool) as a high level
tool to install packages from online repositories, install dependencies, etc. APT uses dpkg to install.
(Apt should not be mixed with Aptitude which is another package management tool for Debian.)
Debian based systems also have GDebi which is an APT/dpkg tool that installs .deb
files (with dpkg) but can also access repositories to get dependencies (which GDebi will install with APT).

Another package manager is called RPM (Red Hat Package Manager) and instead of having .deb extensions it uses .rpm. RPM-based distros are
e.g. RedHat, CentOS, and Fedora. In these systems the low level tool is called rpm and the high level tool is
called yum.

There are ways to install, say a .rpm file, on a Debian based system, one way is with a tool called alien.
The alien tool converts the .rpm file to a .deb. However, it is always advisable to use the package manager
for your system.

Arch based systems use a package management tool called pacman. If you run Arch or any Arch based system such
as Manjaro or Parabola, this is what you will use. Gantoo uses Portage Package Manager.

#### Stability
Some distros are considered more stable than others. It usually has to do with the package manager (what it allows you to install from it) and how often the distro is updated. E.g. Debian is considered a stable distribution with older software, whereas e.g. Arch is considered a more unstable distribution but with newer software. Thus, depending on what you'll use the machine for stability might be a priority or not.

#### Free software
One should always opt for using free software and control as much as possible of the system. Note that there
is a difference between "open source" and "free software". It might not
be possible to Core/Libre-boot (free software version of BIOS) your machine, but running a free OS might
be possible. When running a completely free software OS one might have problems with proprietary drivers for
some devices such as WiFi-cards, so this must be checked.

There is usually a trade off between free software and optimal performance. E.g. Nvidia's drivers are proprietary,
however there are free/libre alternatives provided by [Nouveau](https://nouveau.freedesktop.org/wiki/) which might
work for you.

A compromise could be to run e.g. Debian that is based on free software but has access to proprietary repositories
and in that way install only what is needed for your system.

If one wants a completely free/libre software (non proprietary) distro there is a list at the
[GNU.org webpage](https://www.gnu.org/distros/free-distros.html).

Although free software is the gold standard, a step in the right direction is still better than none.
This means that user friendly distros such as Ubuntu or one of its derivatives, where everything works right out of the box,
are still good alternatives.

#### Customizability
If customizability is of high concern there are distros that can be customized from the ground up. Arch and
Gentoo are examples of these. You can decide exactly what software to install and you can configure
your system in detail. This can be a great learning experience to understand how a GNU/Linux system is installed from
scratch.

However, if you do not care about specific customizability and just want a system that runs so you can go on
with your projects, then it might better to run a more user friendly system.

One thing to point out is that the window manager/desktop environment does not depend on the distribution, e.g. i3
can be used with nearly any system, and thus should not be a criterion when choosing distribution.

#### Note on security and Qubes OS
Some people might need higher security on their system. There is a very interesting distribution
called Qubes OS. Qubes OS runs several virtual machines inside your system, such that you have one VM for each
category of tasks you use your computer for. You can e.g. have one VM for work, one for social media, one for
financial tasks, and so on. This means that if your system would get infected only that VM would be compromised.
In a regular system, if your computer gets infected your whole system is compromised. Drawback with Qubes OS
are the hardware requirements, and the slightly tedious but necessary way to operate it.

#### Note on privacy and Ubuntu
First, I want to emphasize my appreciation of Canonical. In fact the first Linux distro I ever used was something like
version 5.* or 6.* of Ubuntu back in 2004-2005. I got, for free, posted to me CDs with the distro.
They have clearly had a huge impact on the Linux community and we have a lot to be grateful for.

However, in older versions of Ubuntu when the Unity desktop environment was used, the operating system logged user data from the
desktop environment search function and sold it to Amazon. Even if Canonical has stopped logging data since versions 16.* it is still a concern, and
since proprietary software is used in the OS one cannot be completely sure what is going on.

#### Conclusion
* There is no right system, so pick one for your needs.
* Ubuntu is fine and any step towards free software is great.
